# 🧠 OVERVIEW - Controllers

## 개념
**Controllers : request & response를 처리하는 역할**
- `@Controller()` 데코레이터를 사용하여 라우팅 가능.
- `@Get()`, `@Post`와 같은 데코레이터로 HTTP 메소드 정의
- `res.json()`과 같은 표기 없이 return으로 response를 클라이언트에게 JSON으로 전달 가능.
    - `@Res({ passthrough: true })` 옵션을 사용하여 부분적으로 Response를 처리하는 것도 가능함.
- `@Req`, `@Param(key?: string)`, `@Headers(name?: string)`과 같이 데코레이터를 사용하여 사용자 요청에 접근 가능.
- `*`를 route에 대한 wildcard로 사용할 수 있다.
- `@HttpCode(...)` : HTTP 요청에 대한 응답코드 설정
- `@Header()` : custom response header에 대한 설정
- `@Redirect(url, statusCode)` : 특정한 URL로의 redirect가 응답에 필요한 경우 사용
- `:id`와 같은 pathvariable에 대해서는 `@Params` 데코레이터를 사용
- `host` 옵션을 사용하여 sub-domain Routing이 가능, host 관련 정보는 `@HostParam` 데코레이터로 접근 가능
- 기본적으로 싱글스레드에서 Node.js가 돌기 때문에 싱글턴 객체의 사용이 안전, 그러나 요청마다 새로운 인스턴스가 필요한 경우에는 Injection scope을 조절하여 요청마다 인스턴스를 따로 둘 수 있음.
- NestJS는 비동기처리 관련하여 `async` function과 `Promise`, `Observable`을 지원한다.
- `@Body` 데코레이터로 **class로 정의한** DTO에 대하여 사용자 파라미터를 받아올 수 있다.
    - `ValidationPipe`로 DTO에 정의된 파라미터들만 넘어오거나 요청을 막는 Whitelist 기능을 사용할 수 있다.
- `@Query()` : 쿼리 파라미터 추출의 역할, 중첩된 필드나 복잡한 구조의 해석이 필요한 경우 `qs`와 같은 parser를 덧붙여 사용할 수 있다.
    - express에서는 `extended`로 parser를 설정한다.
- 컨트롤러는 항상 Module로 등록되어야 사용가능하다. by `@Module`
- Library-specific approach : `@Res` 데코레이터를 사용하여 조금 더 customized 된 response 처리가 가능하지만, `passthrough` 옵션을 잘 사용해야 부가적인 기능을 Nest가 처리하도록 넘겨줄 수 있다.

## 예시
```JS
import { Controller, Get, Query, Post, Body, Put, Param, Delete } from '@nestjs/common';
import { CreateCatDto, UpdateCatDto, ListAllEntities } from './dto';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(@Query() query: ListAllEntities) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns a #${id} cat`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return `This action updates a #${id} cat`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `This action removes a #${id} cat`;
  }
}
```

---

## 레퍼런스
- [Controllers](https://docs.nestjs.com/controllers)