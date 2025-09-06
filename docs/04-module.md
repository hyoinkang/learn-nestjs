# 🧠 OVERVIEW - Modules

## 개념
1. **Module의 개요**
**Module : `@Module` 데코레이터로 주석이 달린 클래스**
- `@Module` 데코레이터는 Nest가 애플리케이션 구조를 효율적으로 조직하고 관리하는데 사용하는 메타데이터를 제공한다.
- 모든 Nest 애플리케이션은 하나 이상의 모듈을 가진다.(root module)
    - root module은 `application graph`의 starting point로 제공된다.
    - **application graph**: Nest가 모듈과 프로바이더 간의 의존성/관계를 해결하기 위해 사용하는 내부적인 구조
- 각 모듈은 provider들을 기본적으로 캡슐화하여 1.provider가 현재 모듈 안에서 사용되거나 2.다른 imported module에 명시적으로 export 되는 방법으로만 provider가 주입될 수 있도록 한다.

2. **@Module()의 속성**
- `providers`: Nest injector에 의해 인스턴스화 되고 해당 모듈 내에서 공유될 provider
- `controllers`: 모듈 내부에서 정의되고 인스턴스화되어야 할 컨트롤러의 집합
- `imports`: 해당 모듈에 provider의 export를 위해 import되어야 할 모듈의 집합
- `exports`: `providers` 속성의 부분집합으로, 다른 모듈에 export되어 사용될 provider를 정의

## 예시
```JS
// cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```
```JS
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

---

## 레퍼런스
- [Modules](https://docs.nestjs.com/modules)