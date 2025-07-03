# 🧠 About NestJS

## 개념
1. **NestJS란?**
    - Nest는 `TypeScript`를 완전히 지원하며 `OOP`, `FP`, `FRP`등의 요소를 결합하여 효율적이고 확장가능한 `Node.js` 서버 개발을 도와주는 프레임 워크이다.
    - `Express`(default)와 `Fastify`와 같은 Node.js 프레임워크 위의 추상화를 제공한다. 또한, 그 자체의 API도 지원하여 개발자들의 유연한 사용을 지원한다.

2. **Nest의 철학**
    - 아키텍처에 있어 효율적이지 못하다는 기존 문제점을 보완하고자 testable, scalable, loosely coupled,and easily maintainable한 application 아키텍처를 제공하고자 함.
    - Angular의 아키텍처에서 영향을 받음.

3. **First Step for NestJS**
    - 언어 : `TypeScript` 기반이지만 `JavaScript` 또한 지원한다. 단, 최신 기능을 JS에서 사용하려면 Babel 등 추가 설정이 필요할 수 있다.
    - Node.js `v20` 이상이 설치되어야한다.
    - `@nestjs/cli` : Nest CLI를 사용하여 프로젝트 셋업이 가능하다.
    - `main.ts`에서는 `NestFactory`를 통해 app 인스턴스를 생성하고, `bootstrap()` 함수 내에서 이를 실행해 서버를 시작한다.
    - `platform-express`, `platform-fastify` 지원.
    - Running the application
        ```bash
        $ npm run start

        $ npm run start:dev
        ```
    - Nest는 기본적으로 Linter와 formatter를 포함하여 코드 검사, 자동 정렬 등의 기능을 사용가능하다.
        ```bash
        $ npm run lint

        $ npm run format
        ```
---
## 요약
- NestJS는 타입스크립트를 완전히 지원하며 구조, 로직, 비동기처리를 유연하게 섞어 쓸 수 있는 프레임워크이다.
- NestJS는 개발자들에게 일관된 아키텍처를 제공한다.
- controller, module, service 구조를 가진다.

## 레퍼런스
- [INTRODUCTION](https://docs.nestjs.com/)
- [OVERVIEW/first steps](https://docs.nestjs.com/first-steps)