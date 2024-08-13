
# 🏷️ Primeros Decoradores en NestJS

## 🌟 ¿Qué son los Decoradores?

Los **decoradores** en NestJS son una característica avanzada de TypeScript que permite modificar el comportamiento de clases, métodos, propiedades o parámetros en tiempo de ejecución. Los decoradores proporcionan una manera declarativa de agregar metadatos y lógica adicional a las definiciones de clases y sus miembros. En el contexto de NestJS, los decoradores son fundamentales para definir rutas, inyección de dependencias, y mucho más.

## 🎯 ¿Por qué usar Decoradores en NestJS?

### 1. **Simplicidad y Claridad:**
   Los decoradores permiten definir comportamientos complejos de manera declarativa, haciendo que el código sea más fácil de leer y entender. Por ejemplo, en lugar de escribir código para manejar rutas HTTP manualmente, puedes simplemente usar un decorador como `@Get()` para definir una ruta GET.

### 2. **Consistencia y Mantenibilidad:**
   Usar decoradores ayuda a mantener una estructura de código consistente. Cada controlador, servicio o módulo que sigue la misma lógica se define de la misma manera, lo que facilita el mantenimiento y la escalabilidad del proyecto.

### 3. **Inyección de Dependencias:**
   Los decoradores juegan un papel crucial en la inyección de dependencias en NestJS. Decoradores como `@Injectable()` y `@Inject()` permiten a NestJS gestionar y proporcionar instancias de clases de manera eficiente y sin acoplamiento.

## 🔍 Explorando los Decoradores Comunes en NestJS

### 1. **Decoradores de Rutas**:

   Los decoradores de rutas son utilizados para definir las rutas HTTP y asociarlas a métodos en los controladores. Aquí algunos de los más utilizados:

   - **`@Get()`**: Define una ruta GET.
     ```typescript
     @Get('users')
     getUsers() {
       return this.userService.findAll();
     }
     ```

   - **`@Post()`**: Define una ruta POST.
     ```typescript
     @Post('users')
     createUser(@Body() createUserDto: CreateUserDto) {
       return this.userService.create(createUserDto);
     }
     ```

   - **`@Put()`**: Define una ruta PUT.
     ```typescript
     @Put('users/:id')
     updateUser(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
       return this.userService.update(id, updateUserDto);
     }
     ```

   - **`@Delete()`**: Define una ruta DELETE.
     ```typescript
     @Delete('users/:id')
     deleteUser(@Param('id') id: string) {
       return this.userService.remove(id);
     }
     ```

### 2. **Decoradores de Parámetros**:

   Los decoradores de parámetros permiten inyectar datos específicos de la solicitud directamente en los métodos del controlador.

   - **`@Param()`**: Extrae un parámetro de la URL.
     ```typescript
     @Get('users/:id')
     getUserById(@Param('id') id: string) {
       return this.userService.findById(id);
     }
     ```

   - **`@Body()`**: Extrae el cuerpo de la solicitud.
     ```typescript
     @Post('users')
     createUser(@Body() createUserDto: CreateUserDto) {
       return this.userService.create(createUserDto);
     }
     ```

   - **`@Query()`**: Extrae los parámetros de consulta (query parameters).
     ```typescript
     @Get('users')
     findUsers(@Query('role') role: string) {
       return this.userService.findByRole(role);
     }
     ```

   - **`@Headers()`**: Extrae los encabezados de la solicitud.
     ```typescript
     @Get('users')
     getUserByHeader(@Headers('authorization') authHeader: string) {
       return this.userService.findByAuthHeader(authHeader);
     }
     ```

   - **`@Session()`**: Extrae datos de la sesión del usuario (cuando se usa con gestión de sesiones).
     ```typescript
     @Get('profile')
     getProfile(@Session() session: any) {
       return session.user;
     }
     ```

### 3. **Decoradores de Controladores**:

   Los decoradores de controladores se utilizan para definir un controlador y sus rutas principales.

   - **`@Controller()`**: Define una clase como un controlador y asocia rutas base a todos los métodos dentro de la clase.
     ```typescript
     @Controller('users')
     export class UsersController {
       // Este controlador maneja todas las rutas que comienzan con /users
     }
     ```

### 4. **Decoradores de Inyección de Dependencias**:

   Los decoradores de inyección de dependencias son cruciales para la gestión de servicios y otras clases que deben ser inyectadas en tu aplicación.

   - **`@Injectable()`**: Marca una clase como un proveedor que puede ser inyectado en otras clases.
     ```typescript
     @Injectable()
     export class UsersService {
       // Este servicio ahora puede ser inyectado en controladores u otros servicios
     }
     ```

   - **`@Inject()`**: Inyecta una dependencia específica. Se utiliza cuando necesitas inyectar algo más que la clase por defecto.
     ```typescript
     constructor(@Inject('CustomService') private customService: CustomService) {}
     ```

### 5. **Decoradores de Excepciones**:

   Los decoradores de excepciones permiten manejar errores y excepciones de manera eficiente.

   - **`@Catch()`**: Define un filtro de excepciones personalizado.
     ```typescript
     @Catch(HttpException)
     export class HttpErrorFilter implements ExceptionFilter {
       catch(exception: HttpException, host: ArgumentsHost) {
         // Lógica de manejo de excepciones
       }
     }
     ```

### 6. **Decoradores de Pipes**:

   Los decoradores de pipes transforman y validan datos antes de que lleguen al método del controlador.

   - **`@UsePipes()`**: Aplica uno o varios pipes a un controlador o método específico.
     ```typescript
     @Post('users')
     @UsePipes(ValidationPipe)
     createUser(@Body() createUserDto: CreateUserDto) {
       return this.userService.create(createUserDto);
     }
     ```

   - **`@Pipe()`**: Define un pipe personalizado.
     ```typescript
     @Injectable()
     @Pipe()
     export class ParseIntPipe implements PipeTransform {
       transform(value: any, metadata: ArgumentMetadata): number {
         const val = parseInt(value, 10);
         if (isNaN(val)) {
           throw new BadRequestException('Validation failed');
         }
         return val;
       }
     }
     ```

## 🌐 Ejemplo Completo: Uso de Decoradores en un CRUD

Imagina que estás construyendo un CRUD (Create, Read, Update, Delete) para gestionar usuarios en tu aplicación. Aquí tienes cómo podrías usar decoradores para lograrlo:

```typescript
import { Controller, Get, Post, Put, Delete, Param, Body, UsePipes, ValidationPipe } from '@nestjs/common';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(id);
  }

  @Post()
  @UsePipes(new ValidationPipe())
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return this.usersService.update(id, updateUserDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.usersService.remove(id);
  }
}
```

### 💡 Detalles Clave:

- **`@Controller('users')`**: Define el controlador y asocia todas las rutas al prefijo `/users`.
- **`@Get(), @Post(), @Put(), @Delete()`**: Define las rutas HTTP y los métodos correspondientes para cada operación CRUD.
- **`@Param(), @Body()`**: Inyecta datos específicos en los métodos, como el ID del usuario o los datos de creación y actualización.
- **`@UsePipes(ValidationPipe)`**: Aplica un pipe de validación para asegurar que los datos del cuerpo de la solicitud cumplan con las reglas definidas en los DTOs.

## 🎓 Conclusión

Los decoradores en NestJS son esenciales para definir y estructurar el comportamiento de tu aplicación. Desde la definición de rutas hasta la inyección de dependencias y el manejo de excepciones, los decoradores permiten que tu código sea más declarativo, limpio y fácil de mantener. 

Dominar el uso de decoradores te permitirá construir aplicaciones NestJS robustas, bien organizadas y preparadas para escalar. ¡Explora más allá de los decoradores básicos y descubre todo lo que puedes lograr con NestJS!
