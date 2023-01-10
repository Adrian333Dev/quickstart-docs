# Commonly Used HTTP Related Techniques

> 💡 **Note**: Read the [Controllers](https://docs.nestjs.com/controllers) chapter to learn more about controllers.

```typescript

## Sending Response Status Codes

- `@HttpCode()` decorator can be used to set the HTTP status code for a given route handler.
- `HttpStatus` enum can be used to set the HTTP status code for a given route handler.

```typescript
import { /* ... */ HttpStatus, HttpCode } from '@nestjs/common';

@Post()
@HttpCode(HttpStatus.GONE) // 👈
create(@Body() body) {
    // ...
}
```

---

## Implementing Pagination with Query Parameters

- `@Query()` decorator can be used to access query parameters.

```typescript boards.controller.ts
import { /* ... */ Query } from '@nestjs/common';

@Controller('boards')
export class BoardsController {
    @Get()
    getAll(@Query() query) {
        const { page, limit } = query;
        return `This action returns all boards. Page: ${page}, Limit: ${limit}`;
    }
}
```

---

## Sending User-Friendly Error Messages

- `@HttpException()` can be used to throw an exception with a **custom message** as a first argument and a **status code** as a second argument.

```typescript boards.service.ts
import { /* ... */ HttpException, HttpStatus } from '@nestjs/common';

@Injectable()
export class BoardsService {
    async getBoardById(id: number): Promise<Board> {
        const board = await this.boardRepository.findOne(id);
        if (!board) {
            throw new HttpException('Board not found', HttpStatus.NOT_FOUND);
        }
        return board;
    }
}
```

- NestJS also has helper methods for all the common error responses. For example, `NotFoundException()` can be used to throw a `404` error.

```typescript boards.service.ts
import { /* ... */ NotFoundException } from '@nestjs/common';

if (!board) {
    throw new NotFoundException(`Board with ID "${id}" not found`);
}

```
