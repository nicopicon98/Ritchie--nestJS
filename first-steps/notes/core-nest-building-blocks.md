
# 🏗️ Elementos Fundamentales de NestJS

## 🌟 Introducción a los Bloques de Construcción de NestJS

NestJS es un framework poderoso para construir aplicaciones del lado del servidor que aprovecha TypeScript, y está diseñado con la modularidad y la escalabilidad en mente. Comprender los bloques de construcción fundamentales de NestJS es crucial para crear aplicaciones robustas y mantenibles.

## 🎯 ¿Cuáles son los Bloques de Construcción Fundamentales en NestJS?

### 1. **Módulos**:
   Los módulos son los elementos de organización fundamentales en una aplicación NestJS. Un módulo encapsula una parte específica de la funcionalidad de la aplicación y agrupa los controladores, proveedores, y otros módulos relacionados.

   - **`@Module()`**: Decorador que define una clase como un módulo de NestJS.
   - Los módulos ayudan a mantener el código organizado y modular, facilitando la escalabilidad y el mantenimiento.

   ```typescript
   import { Module } from '@nestjs/common';
   import { UsersController } from './users/users.controller';
   import { UsersService } from './users/users.service';

   @Module({
     controllers: [UsersController],
     providers: [UsersService],
   })
   export class UsersModule {}
   ```

### 2. **Controladores**:
   Los controladores en NestJS manejan las solicitudes entrantes y devuelven las respuestas al cliente. Son responsables de definir las rutas y asociar estas rutas con métodos específicos que procesan las solicitudes.

   - **`@Controller()`**: Decorador que define una clase como un controlador.
   - Los controladores manejan la lógica relacionada con las solicitudes HTTP, como GET, POST, PUT y DELETE.

   ```typescript
   import { Controller, Get, Post, Body, Param } from '@nestjs/common';
   import { UsersService } from './users.service';

   @Controller('users')
   export class UsersController {
     constructor(private readonly usersService: UsersService) {}

     @Get()
     findAll() {
       return this.usersService.findAll();
     }

     @Post()
     create(@Body() createUserDto: any) {
       return this.usersService.create(createUserDto);
     }

     @Get(':id')
     findOne(@Param('id') id: string) {
       return this.usersService.findOne(id);
     }
   }
   ```

### 3. **Servicios (Providers)**:
   Los servicios encapsulan la lógica de negocio de la aplicación. Pueden ser inyectados en controladores u otros servicios, y permiten separar la lógica de negocio de la lógica de presentación.

   - **`@Injectable()`**: Decorador que marca una clase como un servicio que puede ser inyectado en otras partes de la aplicación.
   - Los servicios manejan tareas como la interacción con la base de datos, la lógica de negocio, y la integración con otros sistemas.

   ```typescript
   import { Injectable } from '@nestjs/common';

   @Injectable()
   export class UsersService {
     private users = [];

     findAll() {
       return this.users;
     }

     findOne(id: string) {
       return this.users.find(user => user.id === id);
     }

     create(user) {
       this.users.push(user);
       return user;
     }
   }
   ```

### 4. **Pipes**:
   Los pipes son usados para transformar y validar datos antes de que lleguen al controlador. Son útiles para asegurar que los datos de entrada sean del tipo correcto o cumplan con ciertos criterios.

   - **`@UsePipes()`**: Decorador que aplica uno o más pipes a un método o a todo un controlador.
   - Pipes comunes incluyen `ValidationPipe`, `ParseIntPipe`, entre otros.

   ```typescript
   import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

   @Injectable()
   export class ParseIntPipe implements PipeTransform {
     transform(value: any, metadata: ArgumentMetadata) {
       const val = parseInt(value, 10);
       if (isNaN(val)) {
         throw new BadRequestException('Validation failed');
       }
       return val;
     }
   }
   ```

### 5. **Guardias (Guards)**:
   Los guardias determinan si se permite que una solicitud alcance una ruta o método específico. Son útiles para implementar lógica de autorización.

   - **`@UseGuards()`**: Decorador que aplica uno o más guardias a un método o a todo un controlador.
   - Los guardias pueden verificar tokens JWT, roles de usuario, permisos, entre otros.

   ```typescript
   import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
   import { Observable } from 'rxjs';

   @Injectable()
   export class AuthGuard implements CanActivate {
     canActivate(
       context: ExecutionContext,
     ): boolean | Promise<boolean> | Observable<boolean> {
       const request = context.switchToHttp().getRequest();
       const token = request.headers.authorization;
       return token && token === 'valid-token'; // Lógica de autorización simple
     }
   }
   ```

### 6. **Interceptors**:
   Los interceptores permiten transformar la respuesta antes de enviarla al cliente o intervenir en el proceso de una solicitud para añadir lógica adicional, como el manejo de errores o la creación de logs.

   - **`@UseInterceptors()`**: Decorador que aplica uno o más interceptores a un método o a todo un controlador.
   - Los interceptores pueden modificar la respuesta, añadir cabeceras, entre otras cosas.

   ```typescript
   import {
     Injectable,
     NestInterceptor,
     ExecutionContext,
     CallHandler,
   } from '@nestjs/common';
   import { Observable } from 'rxjs';
   import { map } from 'rxjs/operators';

   @Injectable()
   export class TransformInterceptor implements NestInterceptor {
     intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
       return next.handle().pipe(map(data => ({ data })));
     }
   }
   ```

### 7. **Filtros de Excepciones (Exception Filters)**:
   Los filtros de excepciones manejan errores que se producen durante la ejecución de una solicitud. Permiten controlar cómo se envían las excepciones al cliente, proporcionando mensajes de error más amigables o personalizados.

   - **`@Catch()`**: Decorador que marca una clase como un filtro de excepciones.
   - Los filtros de excepciones pueden manejar errores específicos o generales, y personalizar la respuesta en función del error.

   ```typescript
   import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';

   @Catch(HttpException)
   export class HttpErrorFilter implements ExceptionFilter {
     catch(exception: HttpException, host: ArgumentsHost) {
       const ctx = host.switchToHttp();
       const response = ctx.getResponse();
       const status = exception.getStatus();

       response.status(status).json({
         statusCode: status,
         message: exception.message,
       });
     }
   }
   ```

## 🎓 Conclusión

Comprender estos bloques de construcción fundamentales es esencial para cualquier desarrollador que quiera dominar NestJS. Los módulos, controladores, servicios, pipes, guardias, interceptores y filtros de excepciones trabajan juntos para crear aplicaciones robustas, escalables y fáciles de mantener. Al dominar estos conceptos, estarás mejor preparado para construir aplicaciones que no solo funcionen bien, sino que también sean fáciles de mantener y escalar a medida que crecen.

