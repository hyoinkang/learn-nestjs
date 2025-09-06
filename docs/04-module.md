# 🧠 OVERVIEW - Modules

## 개념
1. **Module의 개요**
**Module : `@Module` 데코레이터로 주석이 달린 클래스**
- `@Module` 데코레이터는 Nest가 애플리케이션 구조를 효율적으로 조직하고 관리하는데 사용하는 메타데이터를 제공한다.
- 모든 Nest 애플리케이션은 하나 이상의 모듈을 가진다.(root module)
    - root module은 `application graph`의 starting point로 제공된다.
    - **application graph**: Nest가 모듈과 프로바이더 간의 의존성/관계를 해결하기 위해 사용하는 내부적인 구조
- 각 모듈은 provider들을 기본적으로 캡슐화하여 1.provider가 현재 모듈 안에서 사용되거나 2.다른 imported module에 명시적으로 export 되는 방법으로만 provider가 주입될 수 있도록 한다.
- `module re-exporting`: 각 모듈은 내부 provider뿐 아니라 import한 모듈을 re-export 할 수 있다.
- `dependency injection`: 모듈도 provider의 의존성 주입이 가능하다.(ex. for configuration purpose by consturctor)
    - 그러나, 순환 의존성 문제로 인해 모듈 클래스 자체는 의존성 주입이 불가능하다.
    - **circular dependency**: class A가 class B를 필요로 함과 동시에 class B 또한 class A를 필요로 하는 순환 참조 관계.

2. **@Module()의 속성**
- `providers`: Nest injector에 의해 인스턴스화 되고 해당 모듈 내에서 공유될 provider
- `controllers`: 모듈 내부에서 정의되고 인스턴스화되어야 할 컨트롤러의 집합
- `imports`: 해당 모듈에 provider의 export를 위해 import되어야 할 모듈의 집합
- `exports`: `providers` 속성의 부분집합으로, 다른 모듈에 export되어 사용될 provider를 정의

3. **Module의 종류**
- `Feature Module`: SOLID 원칙에 기거하여 특정한 feature와 관련한 코드를 feature module로 묶고 명확한 경계를 유지할 수 있다.
    - `nest g module cats` 커맨드를 사용해 CLI로 모듈을 만들 수 있다.
    - root module에 생성한 모듈을 import하는 과정이 필요하다.
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

- `Shared Module`: 모듈은 기본적으로 singletons이기 때문에, 하나의 인스턴스를 여러 모듈에서 공유하여 사용가능하다.
    - **singleton**: 특정 클래스의 인스턴스를 애플리케이션 내에 단 하나만 생성하도록 보장하는 디자인 패턴, 자원 절약 및 전역 상태 관리 목적에서 주로 사용한다.
    - `exports` 속성에 공유할 provider를 등록하면, module을 import한 모든 모듈에서 해당 프로바이더를 사용가능하다.
    - modularity와 dependecy injection의 장점을 활용하고자 직접 provider가 필요한 곳에 등록하여 사용하는 대신 모듈을 통해 provider를 캡슐화하여 사용하게 된다.
```JS
//cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService]
})
export class CatsModule {}
```

- `Global Module`: `@Global` 데코레이터를 사용하여 global-scoped module을 정의할 수 있다.
    - Nest에서는 provider를 encapsluate하여 제공하기 때문에 import와 동시에 전역 사용은 불가능하다.
    - 대신 `@Global` 데코레이터로 전역 모듈로 만들어 boilerplate 코드를 줄일 수 있으나, 디자인 패턴을 고려하였을 때 전역 모듈이 항상 좋은 것은 아니다.
    - 더욱 좋은 구조와 유지보수성, 불필요한 coupling을 피하기 위해서는 imports에 명시적으로 의존성을 표기하는 것을 공식문서에서는 추천하고 있다.
```JS
import { Module, Global } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Global()
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

- `Dynamic Module`: 런타임에 특정한 옵션 또는 configuration으로 모듈을 구성 및 생성할 수 있다.
    - `forRoot()` 메서드는 동기적/비동기적으로 dynamic module을 반환한다.
    - `@Module`에 기본적으로 포함되는 `Connection`과 같은 정적 프로바이더에 더해 `forRoot()`에 정의된 내용을 바탕으로 동적 프로바이더를 확장하여 `export`한다.
    - 즉, `override`가 아닌 `extend`로 두 가지의 프로바이더를 모두 모듈에서 export한다.
```JS
import { Module, DynamicModule } from '@nestjs/common';
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
  exports: [Connection],
})
export class DatabaseModule {
  static forRoot(entities = [], options?): DynamicModule {
    const providers = createDatabaseProviders(options, entities);
    return {
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
```
```JS
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
  exports: [DatabaseModule],
})
export class AppModule {}
```

---

## 레퍼런스
- [Modules](https://docs.nestjs.com/modules)