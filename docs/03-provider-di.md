# 🧠 OVERVIEW - Providers and Dependency Injection

## 개념
**Provider : Nest의 의존성 주입 시스템을 통해 컨트롤러나 다른 provider에 주입되는 객체(services, repositories, factories, helpers...etc)**
- `provider`들이 컨트롤러의 요청에 대해 더욱 복잡한 task들을 수행한다.
- NestJS module에 의해 providers로 선언되었을 때, Provider로 사용이 가능하다. 즉, Provider 역시 **Module에 의해 참조**되어야 사용가능하다.
    - NestJS는 OOP의 `SOLID` 원칙을 기저로 삼고 있다.
- `@Injectable` 데코레이터를 사용하여 class에 메타데이터를 붙이고 **Nest IoC 컨테이너**에게 해당 클래스의 관리 권한을 위임한다.
- **consturctor-based injection** : `constructor` 클래스로 컨트롤러에 서비스를 주입하여 사용할 수 있다.
- Provider는 application lifecycle 동안 lifetime, 즉 scope를 지닌다. 기본적으로 provider의 lifecycle은 appication의 bootstrap된 시점부터 shut down된 시점이지만, **request-scoped**([Injection Scopes](https://docs.nestjs.com/fundamentals/injection-scopes)) 또한 가능하다.
- 기본적으로 provider의 관리에 built-in IoC Container를 사용하지만 그 외에도 Provider의 Custom에 여러 방법이 있다.
- 조건부 의존성 구현 시에는 `@Optional()` 데코레이터를 사용하여 provider가 주입되지 않더라도 예외없이 `undefined`로 처리하도록 할 수 있다.
- **property-based injection** : 상속 구조에서 super()로 의존성을 넘겨야 하는 특수한 상황에서 번거로움을 줄이기 위해 @Inject() 데코레이터를 사용한 프로퍼티 기반 의존성 주입이 가능하다.
    - 다만, 상속 받는 다른 클래스가 없는 상황에서는 되도록 생성자 기반 의존성 주입을 사용하는 것이 좋다.
- 특수한 상황에서 DI를 사용하지 않고 직접 인스턴스를 가져와야 하는 경우가 있을 수 있다. 이러한 상황에서 다음 두가지 방식을 사용할 수 있다.
    1. [Module reference](https://docs.nestjs.com/fundamentals/module-ref)
    2. [bootstrap()](https://docs.nestjs.com/standalone-applications)

**Dependency Injection(의존성 주입) : 앱의 특정한 부분을 다른 부분에서 필요로 할 때, 이를 전달하고 생성하는 메커니즘이자 디자인 패턴**
- 필요한 기능이나 클래스, 모듈을 스스로 만들지 않고 외부에서 주입받음을 통해 더욱 **유연하고 모듈화되고 테스트하기 쉬운** 코드를 짤 수 있다.

## 예시
```JS
// service.ts
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}

```
```JS
// controller.ts

import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}

```

---

## 레퍼런스
- [Providers(Services)](https://docs.nestjs.com/providers)
- [Dependency Injection](https://angular.dev/guide/di)