
# Implementación de CORS en NestJS

## Estructura de Archivos Sugerida
La estructura de archivos dentro de src/ para una aplicación NestJS con configuración de CORS podría verse de la siguiente manera:

```
src/
│
├── common/
│   ├── middleware/
│   │   ├── cors.middleware.ts
│
├── app.module.ts
└── main.ts
```

### Descripción de Archivos:
- **common/middleware/cors.middleware.ts**: Middleware que contiene la configuración de CORS.
- **app.module.ts**: Módulo raíz de la aplicación que importa y organiza otros módulos.
- **main.ts**: Punto de entrada de la aplicación donde se inicializa la configuración de CORS.

## 1. Crear un Nuevo Proyecto en NestJS
Primero, asegúrate de tener instalado el CLI de NestJS:
```bash
npm i -g @nestjs/cli
```

Luego, crea un nuevo proyecto:
```bash
nest new nombre-del-proyecto
```

## 2. Instalar Dependencias Necesarias
Para configurar CORS, no se requieren dependencias adicionales en NestJS, ya que esta funcionalidad está incorporada.

## 3. Configurar CORS en el archivo `main.ts`
Puedes habilitar CORS directamente en el archivo `main.ts`:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { CorsOptions } from '@nestjs/common/interfaces/external/cors-options.interface';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  const corsOptions: CorsOptions = {
    origin: 'http://example.com',  // Reemplaza con el origen permitido
    methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
    credentials: true,
  };
  
  app.enableCors(corsOptions);
  await app.listen(3000);
}
bootstrap();
```

## 4. Crear un Middleware para CORS (Opcional)
Si necesitas una configuración más compleja de CORS, puedes crear un middleware personalizado:

```bash
nest g mo common/middleware
```

Dentro del middleware:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class CorsMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    res.header('Access-Control-Allow-Origin', 'http://example.com');
    res.header('Access-Control-Allow-Methods', 'GET,HEAD,PUT,PATCH,POST,DELETE');
    res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
    next();
  }
}
```

Luego, en el módulo principal (`app.module.ts`):

```typescript
import { Module, MiddlewareConsumer } from '@nestjs/common';
import { CorsMiddleware } from './common/middleware/cors.middleware';

@Module({
  imports: [],
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(CorsMiddleware)
      .forRoutes('*');
  }
}
```

## 5. Ejecutar la Aplicación y Probar la Configuración de CORS
Ahora puedes ejecutar la aplicación y probar la configuración de CORS:
```bash
npm run start
```

Prueba realizando solicitudes desde un origen diferente para verificar que CORS esté configurado correctamente.
