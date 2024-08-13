
# 💼 Servicios en NestJS

## 🌟 ¿Qué son los Servicios en NestJS?

Los **servicios** en NestJS son clases que encapsulan la lógica de negocio y pueden ser reutilizadas en diferentes partes de la aplicación. Los servicios permiten desacoplar la lógica de las operaciones de los controladores y otros componentes, facilitando la modularidad, mantenibilidad y testabilidad del código.

## 🎯 ¿Por qué usar Servicios en NestJS?

### 1. **Encapsulación de la Lógica de Negocio:**
   Los servicios permiten mantener la lógica de negocio separada de los controladores, lo que facilita la organización del código y su posterior mantenimiento. Esto es especialmente útil cuando la lógica es compleja o se utiliza en múltiples lugares.

### 2. **Reutilización del Código:**
   Al encapsular la lógica en servicios, puedes reutilizar el mismo código en diferentes controladores o módulos. Esto reduce la duplicación de código y mejora la coherencia de la aplicación.

### 3. **Facilidad de Pruebas:**
   Los servicios son fáciles de probar de manera aislada, lo que permite escribir tests unitarios efectivos para la lógica de negocio sin necesidad de configurar el entorno completo de la aplicación.

### 4. **Inyección de Dependencias:**
   Los servicios en NestJS aprovechan el sistema de inyección de dependencias, lo que permite gestionar de manera eficiente la creación y el ciclo de vida de las instancias de servicios. Esto hace que la aplicación sea más flexible y fácil de extender.

## 🔧 ¿Cómo funcionan los Servicios en NestJS?

### 1. **Creación de un Servicio:**
   Para crear un servicio en NestJS, puedes usar el CLI, que genera automáticamente la estructura básica de la clase de servicio:

   ```bash
   nest g s users
   ```

   Esto generará un archivo `users.service.ts` con el siguiente contenido básico:

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

     update(id: string, user) {
       const existingUser = this.findOne(id);
       if (existingUser) {
         Object.assign(existingUser, user);
         return existingUser;
       }
       return null;
     }

     remove(id: string) {
       const index = this.users.findIndex(user => user.id === id);
       if (index !== -1) {
         const removedUser = this.users.splice(index, 1);
         return removedUser[0];
       }
       return null;
     }
   }
   ```

   🔍 **Explicación**:
   - `@Injectable()`: El decorador `@Injectable()` marca la clase como un proveedor que puede ser inyectado en otras partes de la aplicación, como controladores u otros servicios.
   - **Métodos CRUD**: Este servicio `UsersService` contiene métodos para operaciones CRUD (Create, Read, Update, Delete) básicas sobre una colección de usuarios simulada en memoria.

### 2. **Inyección de un Servicio en un Controlador:**
   Para utilizar un servicio en un controlador, se inyecta en el constructor del controlador:

   ```typescript
   import { Controller, Get, Post, Body, Param, Put, Delete } from '@nestjs/common';
   import { UsersService } from './users.service';

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
     create(@Body() createUserDto: any) {
       return this.usersService.create(createUserDto);
     }

     @Put(':id')
     update(@Param('id') id: string, @Body() updateUserDto: any) {
       return this.usersService.update(id, updateUserDto);
     }

     @Delete(':id')
     remove(@Param('id') id: string) {
       return this.usersService.remove(id);
     }
   }
   ```

   💡 **Nota**: La inyección de servicios en NestJS es fundamental para mantener el código desacoplado y modular. El controlador no necesita conocer los detalles de cómo se implementa la lógica de negocio, solo utiliza el servicio para realizar sus tareas.

### 3. **Configuración de un Módulo:**
   Los servicios deben estar registrados en un módulo para que NestJS pueda gestionar su inyección de dependencias. Aquí se muestra cómo se configura un módulo para incluir tanto el servicio como el controlador:

   ```typescript
   import { Module } from '@nestjs/common';
   import { UsersService } from './users.service';
   import { UsersController } from './users.controller';

   @Module({
     controllers: [UsersController],
     providers: [UsersService],
   })
   export class UsersModule {}
   ```

   🔍 **Detalles**:
   - **`providers`**: Aquí es donde registramos los servicios que estarán disponibles para la inyección en este módulo. En este caso, `UsersService` es registrado y luego inyectado en `UsersController`.

## 🌐 Ejemplo Completo: Servicio Avanzado con Integración de Base de Datos

Imagina que necesitas integrar tu servicio con una base de datos en lugar de trabajar con datos en memoria. Aquí tienes un ejemplo de cómo podría ser un servicio más avanzado que se conecta a una base de datos usando TypeORM:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './entities/user.entity';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private usersRepository: Repository<User>,
  ) {}

  findAll(): Promise<User[]> {
    return this.usersRepository.find();
  }

  findOne(id: string): Promise<User> {
    return this.usersRepository.findOne(id);
  }

  async create(createUserDto: CreateUserDto): Promise<User> {
    const user = this.usersRepository.create(createUserDto);
    return this.usersRepository.save(user);
  }

  async update(id: string, updateUserDto: UpdateUserDto): Promise<User> {
    await this.usersRepository.update(id, updateUserDto);
    return this.findOne(id);
  }

  async remove(id: string): Promise<void> {
    await this.usersRepository.delete(id);
  }
}
```

### 💡 Detalles Clave:

- **`@InjectRepository(User)`**: Este decorador inyecta el repositorio TypeORM para la entidad `User`, permitiendo interactuar con la base de datos directamente desde el servicio.
- **CRUD con Base de Datos**: Los métodos CRUD ahora interactúan con una base de datos real, lo que hace que el servicio sea más robusto y escalable.

## 🎓 Conclusión

Los servicios en NestJS son el núcleo de la lógica de negocio en una aplicación. Al encapsular la lógica en servicios, puedes construir aplicaciones modulares, escalables y fáciles de mantener. La inyección de dependencias y la integración con bases de datos o APIs externas hacen que los servicios sean extremadamente poderosos y flexibles. Dominar los servicios en NestJS es crucial para cualquier desarrollador que desee construir aplicaciones robustas y profesionales.

