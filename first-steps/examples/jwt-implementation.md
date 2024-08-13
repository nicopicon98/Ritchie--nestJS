
# Implementación de JWT en NestJS

## Estructura de Archivos Sugerida
La estructura de archivos dentro de src/ para una aplicación NestJS que implementa JWT podría verse de la siguiente manera:

```
src/
│
├── auth/
│   ├── auth.controller.ts
│   ├── auth.module.ts
│   ├── auth.service.ts
│   ├── jwt.strategy.ts
│   ├── jwt-auth.guard.ts
│
├── users/
│   ├── users.controller.ts
│   ├── users.module.ts
│   ├── users.service.ts
│   ├── users.entity.ts
│
├── app.module.ts
└── main.ts
```

### Descripción de Archivos:
- **auth/auth.controller.ts**: Controlador para manejar las solicitudes de autenticación (login, registro).
- **auth/auth.module.ts**: Módulo que agrupa todos los componentes relacionados con la autenticación.
- **auth/auth.service.ts**: Servicio que contiene la lógica de autenticación.
- **auth/jwt.strategy.ts**: Estrategia JWT para manejar la validación de tokens.
- **auth/jwt-auth.guard.ts**: Guardia que protege rutas usando la estrategia JWT.
- **users/users.controller.ts**: Controlador para manejar las solicitudes relacionadas con los usuarios.
- **users/users.module.ts**: Módulo que agrupa todos los componentes relacionados con los usuarios.
- **users/users.service.ts**: Servicio que contiene la lógica relacionada con los usuarios.
- **users/users.entity.ts**: Entidad que define la estructura de la tabla de usuarios en la base de datos.
- **app.module.ts**: Módulo raíz de la aplicación que importa y organiza otros módulos.
- **main.ts**: Punto de entrada de la aplicación.

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
Instala las librerías necesarias para trabajar con JWT:
```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
npm install --save-dev @types/passport-jwt
```

## 3. Crear un Módulo de Autenticación
Genera un módulo de autenticación que agrupará todos los componentes relacionados con la autenticación:
```bash
nest g mo auth
```

## 4. Crear un Servicio de Autenticación
Crea un servicio que manejará la lógica de autenticación:
```bash
nest g s auth
```

Dentro del servicio, configura JWT:
```typescript
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';

@Injectable()
export class AuthService {
  constructor(private readonly jwtService: JwtService) {}

  async login(user: any) {
    const payload = { username: user.username, sub: user.userId };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}
```

## 5. Configurar el Módulo JWT
Configura el módulo JWT dentro del módulo de autenticación:
```typescript
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { AuthService } from './auth.service';
import { JwtStrategy } from './jwt.strategy';

@Module({
  imports: [
    PassportModule,
    JwtModule.register({
      secret: 'secretKey',
      signOptions: { expiresIn: '60s' },
    }),
  ],
  providers: [AuthService, JwtStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

## 6. Implementar la Estrategia JWT
Crea una estrategia JWT para manejar la validación del token:
```bash
nest g cl auth/jwt.strategy
```

Configura la estrategia:
```typescript
import { Strategy, ExtractJwt } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'secretKey',
    });
  }

  async validate(payload: any) {
    return { userId: payload.sub, username: payload.username };
  }
}
```

## 7. Proteger Rutas con JWT
Aplica el guardia JWT a las rutas que deseas proteger:
```bash
nest g gu auth/jwt-auth
```

Configura el guardia para usar la estrategia JWT:
```typescript
import { Injectable, ExecutionContext } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}
```

Finalmente, protege las rutas en un controlador:
```typescript
import { Controller, Get, UseGuards } from '@nestjs/common';
import { JwtAuthGuard } from './auth/jwt-auth.guard';

@Controller('profile')
export class ProfileController {
  @UseGuards(JwtAuthGuard)
  @Get()
  getProfile() {
    return { message: 'This is a protected route' };
  }
}
```

## 8. Crear un Controlador de Autenticación
Crea un controlador para manejar el login:
```bash
nest g co auth
```

Implementa el método de login:
```typescript
import { Controller, Post, Body } from '@nestjs/common';
import { AuthService } from './auth.service';

@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('login')
  async login(@Body() req) {
    return this.authService.login(req);
  }
}
```

## 9. Ejecutar la Aplicación y Probar la Autenticación
Ahora puedes ejecutar la aplicación y probar la autenticación JWT:
```bash
npm run start
```

Realiza una solicitud POST a `/auth/login` para recibir un token JWT y luego utiliza ese token para acceder a rutas protegidas.
