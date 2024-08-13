
# 🚀 Hoja de Atajos Definitiva de NestJS 🚀

## 📦 **Instalar NestJS CLI**
```bash
npm i -g @nestjs/cli
```

## 🚀 **Crear un Nuevo Proyecto** en el path actual
```bash
nest new nombre-del-proyecto
```

## 🔧 **Resumen de Comandos del CLI**
- **Mostrar ayuda**: `nest -h` *(o agrega `-h` a cualquier comando para ver opciones detalladas)*
- **Generar componentes comunes**: `nest g <comando>`
  - Ejemplo: `nest g s <nombre> -h` *(para opciones adicionales)*

## ⚙️ **Generadores Comunes**
- **Servicio**: 
    ```bash
    nest g s <ruta/nombre>
    ```
- **Controlador**:
    ```bash
    nest g co <ruta/nombre>
    ```
- **Clase**:
    ```bash
    nest g cl <ruta/nombre>
    ```
- **Decorador**:
    ```bash
    nest g d <ruta/nombre>
    ```
- **Guard**:
    ```bash
    nest g gu <ruta/nombre>
    ```
- **Interceptor**:
    ```bash
    nest g in <ruta/nombre>
    ```
- **Módulo**:
    ```bash
    nest g mo <ruta/nombre>
    ```
- **Pipe**:
    ```bash
    nest g pi <ruta/nombre>
    ```
- **Recurso Completo**:
    ```bash
    nest g resource <nombre>
    ```

## 🛠️ **Banderas Adicionales del CLI**
- **Modo dry-run**: Ve lo que hará el comando sin hacer cambios.
    ```bash
    nest g s <nombre> --dry-run | -d
    ```
- **Sin archivo de pruebas**: Omite la creación del archivo de pruebas.
    ```bash
    nest g s <nombre> --no-spec
    ```

## 🌐 **Métodos HTTP Integrados y Argumentos**
```typescript
import { Get, Post, Put, Patch, Delete } from '@nestjs/common';
```
- **Get Básico**
    ```typescript
    @Get()
    ```
- **Segmento Dinámico**
    ```typescript
    @Get(':id')
    ```
- **Ruta Específica**
    ```typescript
    @Get('gatos/raza')
    ```
- **Múltiples Rutas**
    ```typescript
    @Get(['gatos','raza'])
    ```
- **Rutas Dinámicas**
    ```typescript
    @Get(':producto/:tamaño')
    ```
- **Extraer Parámetros/Segmentos**
    ```typescript
    @Param('id')
    ```
- **Extraer Body**
    ```typescript
    @Body()
    ```
- **Extraer Parámetros de Query**
    ```typescript
    @Query()
    ```
- **Objeto de Respuesta (Express/Fastify)**
    ```typescript
    @Res()
    ```
- **Convertir el segmento :id a entero**
    ```typescript
    @Get(':id')
    async findOne(@Param('id', ParseIntPipe) id: number) {
        return this.catsService.findOne(id);
    }
    ```

## 🚀 **Librerías Externas: Paquetes Útiles**
```bash
yarn add class-validator class-transformer
```
### **Decoradores de Class Validator**
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

## ⚙️ **Configuración Global de Pipes**
```typescript
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true, 
    forbidNonWhitelisted: true,
  })
);
```
- **whitelist**: Elimina cualquier propiedad que no esté incluida en los DTOs.
- **forbidNonWhitelisted**: Retorna un bad request si hay propiedades no deseadas en el objeto de la solicitud.

## 🏗️ **Estructura de Módulo Recomendada**
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

## ⚙️ **Configuraciones Globales**
- Aplicable a aquellas que no requieren el "contexto de ejecución"
```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalFilters(new Filtro1(), ...);
app.useGlobalGuards(new Guard1(), ...);
app.useGlobalInterceptors(new Inter1(), ...);
app.useGlobalPipes(new Pipe1(), ...);
```

## 🧩 **Bloques de Construcción**
- **Guards**: Controlan el acceso a una ruta (e.g., autorizando solicitudes).
- **Interceptors**:
  - **Antes**: Transforman la solicitud antes de que llegue a la ruta (e.g., logs, caché).
  - **Después**: Transforman la respuesta antes de que sea enviada (e.g., estandarizar respuestas).
- **Pipes**: Transforman y validan datos entrantes (e.g., asegurando tipos correctos, aplicando validaciones).
- **Controllers**: Manejan rutas y envían respuestas (e.g., operaciones CRUD).
- **Decoradores**: Aplican funcionalidad adicional en diferentes niveles (e.g., `@Controller('usuarios')`, `@Ip()`, `@CustomDecorator()`).
- **Servicios**: Contienen la lógica de negocio y son reutilizables a través de la aplicación (e.g., `PeliculasService` para manejo de datos de películas).
- **Filtros de Excepción**: Manejan excepciones y formatean las respuestas de error.
  - Ejemplo: `BadRequestException`, `NotFoundException`, `InternalServerErrorException`, etc.

## 🌐 **Habilitar CORS**
```typescript
const app = await NestFactory.create(AppModule);
app.enableCors(); 
app.enableCors(options); 
@WebSocketGateway({ cors: true }); // Para WebSockets
```

## 🍪 **Uso de Cookies**
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

## 🔐 **Variables de Entorno**
- **Crea un archivo `.env`** en la raíz del proyecto.
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
- **Inyectando Variables de Entorno**:
```typescript
constructor(private readonly configService: ConfigService) {}
```

## 📂 **Servir Contenido Estático**
- **Crea un directorio `public`**
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
**Referencias:**
- [Documentación de NestJS](https://docs.nestjs.com/)
- [Guía de NestJS por Fernando Herrera](https://fernando-herrera.com)
