
# 🚀 NestJS Ultimate Cheat Sheet 🚀

## 📦 **Install NestJS CLI**
```bash
npm i -g @nestjs/cli
```

## 🚀 **Create a New Project** in the current path
```bash
nest new project-name
```

## 🔧 **CLI Commands Overview**
- **Show help**: `nest -h` *(or add `-h` to any command for detailed options)*
- **Generate common components**: `nest g <command>`
  - Example: `nest g s <name> -h` *(for extra options)*

## ⚙️ **Commonly Used Generators**
- **Service**: 
    ```bash
    nest g s <path/name>
    ```
- **Controller**:
    ```bash
    nest g co <path/name>
    ```
- **Class**:
    ```bash
    nest g cl <path/name>
    ```
- **Decorator**:
    ```bash
    nest g d <path/name>
    ```
- **Guard**:
    ```bash
    nest g gu <path/name>
    ```
- **Interceptor**:
    ```bash
    nest g in <path/name>
    ```
- **Module**:
    ```bash
    nest g mo <path/name>
    ```
- **Pipe**:
    ```bash
    nest g pi <path/name>
    ```
- **Complete Resource**:
    ```bash
    nest g resource <name>
    ```

## 🛠️ **Additional CLI Flags**
- **Dry-run mode**: See what the command will do without making changes.
    ```bash
    nest g s <name> --dry-run | -d
    ```
- **No spec file**: Skip creating the test file.
    ```bash
    nest g s <name> --no-spec
    ```

## 🌐 **Built-in HTTP Methods & Arguments**
```typescript
import { Get, Post, Put, Patch, Delete } from '@nestjs/common';
```
- **Basic Get**
    ```typescript
    @Get()
    ```
- **Dynamic Segment**
    ```typescript
    @Get(':id')
    ```
- **Specific Route**
    ```typescript
    @Get('cats/breed')
    ```
- **Multiple Paths**
    ```typescript
    @Get(['cats','breed'])
    ```
- **Dynamic Paths**
    ```typescript
    @Get(':product/:size')
    ```
- **Extract Parameters/Segments**
    ```typescript
    @Param('id')
    ```
- **Extract Body**
    ```typescript
    @Body()
    ```
- **Extract Query Parameters**
    ```typescript
    @Query()
    ```
- **Response Object (Express/Fastify)**
    ```typescript
    @Res()
    ```
- **Parse :id segment to integer**
    ```typescript
    @Get(':id')
    async findOne(@Param('id', ParseIntPipe) id: number) {
        return this.catsService.findOne(id);
    }
    ```

## 🚀 **External Libraries: Useful Packages**
```bash
yarn add class-validator class-transformer
```
### **Class Validator Decorators**
- `@IsOptional()`
- `@IsPositive()`
- `@IsMongoId()`
- `@IsArray()`
- `@IsDecimal()`
- `@IsBoolean()`
- `@IsString()`
- `@IsDate()`
- `@IsEmail()`
- `@IsUUID()`
- `@IsDateString()`
- `@IsUrl()`

## ⚙️ **Global Pipes Configuration**
```typescript
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true, 
    forbidNonWhitelisted: true,
  })
);
```
- **whitelist**: Removes any properties not included in DTOs.
- **forbidNonWhitelisted**: Returns a bad request if there are unwanted properties in the request object.

## 🏗️ **Recommended Module Structure**
```
src
 ├── common
 │   ├── decorators
 │   ├── dtos
 │   ├── filters
 │   ├── guards
 │   ├── interceptors
 │   ├── middleware
 │   ├── pipes
 │   ├── common.controller.ts
 │   ├── common.module.ts
 │   └── common.service.ts
```

## ⚙️ **Global Configurations**
- Applicable to those that don’t require the “execution context”
```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalFilters(new Filtro1(), ...);
app.useGlobalGuards(new Guard1(), ...);
app.useGlobalInterceptors(new Inter1(), ...);
app.useGlobalPipes(new Pipe1(), ...);
```

## 🧩 **Building Blocks**
- **Guards**: Control access to a route (e.g., authorizing requests).
- **Interceptors**:
  - **Before**: Transform the request before it reaches the route (e.g., logging, caching).
  - **After**: Transform the response before it is sent (e.g., standardizing response structure).
- **Pipes**: Transform and validate incoming data (e.g., ensuring correct types, applying validation).
- **Controllers**: Handle routing and send responses (e.g., CRUD operations).
- **Decorators**: Apply additional functionality at different levels (e.g., `@Controller('users')`, `@Ip()`, `@CustomDecorator()`).
- **Services**: Contain business logic and are reusable across the application (e.g., `MoviesService` for handling movie data).
- **Exception Filters**: Handle exceptions and format error responses.
  - Example: `BadRequestException`, `NotFoundException`, `InternalServerErrorException`, etc.

## 🌐 **Enabling CORS**
```typescript
const app = await NestFactory.create(AppModule);
app.enableCors(); 
app.enableCors(options); 
@WebSocketGateway({ cors: true }); // For WebSockets
```

## 🍪 **Using Cookies**
```bash
yarn add cookie-parser 
yarn add -D @types/cookie-parser
```
```typescript
import * as cookieParser from 'cookie-parser';
// ...
const app = await NestFactory.create(AppModule);
app.use(cookieParser());
```

## 🔐 **Environment Variables**
- **Create a `.env` file** in the root of the project.
```bash
yarn add @nestjs/config
```
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule.forRoot()],
})
export class AppModule {}
```
- **Injecting Environment Variables**:
```typescript
constructor(private readonly configService: ConfigService) {}
```

## 📂 **Serving Static Content**
- **Create a `public` directory**
```bash
yarn add @nestjs/serve-static
```
```typescript
import { ServeStaticModule } from '@nestjs/serve-static';
import { join } from 'path';

@Module({
  imports: [
    ServeStaticModule.forRoot({
      rootPath: join(__dirname, '..', 'public'),
    }),
  ],
})
export class AppModule {}
```

---
**References:**
- [NestJS Documentation](https://docs.nestjs.com/)
- [Fernando Herrera's NestJS Guide](https://fernando-herrera.com)
