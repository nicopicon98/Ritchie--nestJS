
# 🚨 Uso de Exception Filters en NestJS

## ¿Qué son los Exception Filters?

Los **Exception Filters** en NestJS son mecanismos que permiten manejar de manera centralizada los errores que ocurren durante la ejecución de una aplicación. Cuando se produce un error en una ruta o controlador, los exception filters capturan ese error y definen cómo debe responder la aplicación al cliente.

## ¿Por qué utilizar Exception Filters?

### 1. **Manejo Centralizado de Errores:**
   Los exception filters permiten capturar y manejar todos los errores en un solo lugar, lo que facilita la gestión y el mantenimiento del código. En lugar de manejar errores individualmente en cada controlador, se pueden definir filtros que manejen diferentes tipos de errores de manera consistente.

### 2. **Mejora de la Experiencia del Usuario:**
   Al usar exception filters, puedes proporcionar respuestas de error claras y consistentes a los clientes. Esto es crucial en aplicaciones donde la experiencia del usuario es importante, ya que permite que los usuarios comprendan mejor qué salió mal y cómo pueden resolver el problema.

### 3. **Seguridad:**
   Los exception filters permiten controlar qué información se envía al cliente cuando ocurre un error. Esto ayuda a proteger detalles sensibles del sistema, evitando que se expongan a través de mensajes de error sin filtrar.

## ¿Cómo funcionan los Exception Filters?

### 1. **Creación de un Exception Filter:**
   Un exception filter se crea utilizando el decorador `@Catch()` y puede personalizarse para manejar diferentes tipos de excepciones:

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

### 2. **Aplicación del Exception Filter:**
   Una vez creado el filtro, puede aplicarse globalmente en la aplicación, a nivel de controlador, o en métodos específicos:

   - **Aplicación Global:**

     ```typescript
     import { NestFactory } from '@nestjs/core';
     import { AppModule } from './app.module';
     import { HttpErrorFilter } from './common/filters/http-error.filter';

     async function bootstrap() {
       const app = await NestFactory.create(AppModule);
       app.useGlobalFilters(new HttpErrorFilter());
       await app.listen(3000);
     }
     bootstrap();
     ```

   - **Aplicación en un Controlador:**

     ```typescript
     import { Controller, Get, UseFilters } from '@nestjs/common';
     import { HttpErrorFilter } from './common/filters/http-error.filter';

     @Controller('items')
     @UseFilters(HttpErrorFilter)
     export class ItemsController {
       @Get()
       findAll() {
         throw new HttpException('Not found', 404);
       }
     }
     ```

   - **Aplicación en un Método Específico:**

     ```typescript
     import { Get, UseFilters, Controller } from '@nestjs/common';
     import { HttpErrorFilter } from './common/filters/http-error.filter';

     @Controller('users')
     export class UsersController {
       @Get(':id')
       @UseFilters(HttpErrorFilter)
       findOne() {
         throw new HttpException('User not found', 404);
       }
     }
     ```

## Conclusión

Los exception filters son una herramienta poderosa en NestJS que permite manejar errores de forma centralizada, mejorar la experiencia del usuario, y aumentar la seguridad de la aplicación. Implementar exception filters correctamente no solo hace que tu aplicación sea más robusta, sino también más profesional y fácil de mantener.
