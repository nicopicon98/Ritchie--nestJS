
# 🧪 Pipes en NestJS

## 🌟 ¿Qué son los Pipes?

Los **Pipes** en NestJS son potentes herramientas que permiten transformar, validar y manipular datos antes de que lleguen a los controladores. Actúan como un filtro, asegurándose de que los datos sean correctos y estén en el formato adecuado para su procesamiento posterior.

## 🎯 ¿Por qué usar Pipes en NestJS?

### 1. **Validación de Datos Precisa**:
   Los pipes permiten validar los datos de entrada de manera centralizada y uniforme. Por ejemplo, puedes asegurarte de que un parámetro que se espera sea un número entero, realmente lo sea antes de que el controlador procese la solicitud.

### 2. **Transformación Dinámica de Datos**:
   Los pipes pueden transformar los datos antes de que lleguen al controlador, adaptándolos al formato o tipo de dato requerido. Esto es útil para convertir cadenas de texto a tipos más específicos, como números o booleanos, según sea necesario.

### 3. **Manejo Elegante de Errores**:
   Si los datos no cumplen con los criterios definidos, los pipes pueden lanzar excepciones personalizadas. Esto garantiza que los errores se manejen de manera uniforme y clara en toda la aplicación, mejorando la experiencia del usuario y la seguridad de la aplicación.

## 🔧 ¿Cómo funcionan los Pipes en NestJS?

### 1. **Creación de un Pipe Personalizado**:
   Para crear un pipe personalizado, debes implementar la interfaz `PipeTransform` y sobrescribir el método `transform`. Aquí te mostramos cómo crear un pipe que convierte y valida un número entero:

   ```typescript
   import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

   @Injectable()
   export class ParseIntPipe implements PipeTransform {
     transform(value: any, metadata: ArgumentMetadata) {
       const val = parseInt(value, 10);
       if (isNaN(val)) {
         throw new BadRequestException(`El valor ${value} no es un número entero válido`);
       }
       return val;
     }
   }
   ```

   🔍 **Explicación**:
   - `PipeTransform`: Define la estructura básica que cualquier pipe personalizado debe seguir.
   - `transform`: Este método recibe los datos de entrada y los convierte o valida. Si algo sale mal, lanza una excepción.

### 2. **Aplicación de Pipes**:
   Los pipes pueden aplicarse en varios niveles de tu aplicación: globalmente, a nivel de controlador, o incluso en métodos específicos. Aquí te mostramos cómo hacerlo:

   - **Aplicación Global**:
     Aplica un pipe globalmente, afectando a todas las rutas de tu aplicación:

     ```typescript
     import { ValidationPipe } from '@nestjs/common';

     async function bootstrap() {
       const app = await NestFactory.create(AppModule);
       app.useGlobalPipes(new ValidationPipe());
       await app.listen(3000);
     }
     bootstrap();
     ```

   - **Aplicación en un Controlador o Método Específico**:
     Aplica un pipe a nivel de controlador o método para mayor granularidad:

     ```typescript
     import { Controller, Get, Param, UsePipes } from '@nestjs/common';
     import { ParseIntPipe } from './parse-int.pipe';

     @Controller('items')
     export class ItemsController {
       @Get(':id')
       @UsePipes(ParseIntPipe)
       findOne(@Param('id') id: number) {
         return `Item con ID: ${id}`;
       }
     }
     ```

   💡 **Pro Tip**: Puedes combinar varios pipes para aplicar transformaciones y validaciones en cadena, asegurando que los datos pasen por múltiples filtros antes de ser procesados.

### 3. **Pipes Incorporados en NestJS**:
   NestJS proporciona una serie de pipes predefinidos que cubren casos comunes de uso, permitiéndote centrarte en la lógica de negocio:

   - **`ValidationPipe`**: Valida automáticamente las entradas basadas en decoradores y clases DTO.
   - **`ParseIntPipe`**: Convierte cadenas de texto en números enteros.
   - **`ParseBoolPipe`**: Convierte cadenas de texto como "true" o "false" en valores booleanos.
   - **`ParseArrayPipe`**: Convierte cadenas en arrays, permitiendo validar y transformar elementos dentro del array.

## 🚀 Ejemplo Avanzado: Combinación de Pipes

Imagina que tienes una ruta en tu API que debe recibir un array de números, convertirlos a enteros, y asegurarse de que cada uno de ellos es positivo. Aquí es donde los pipes brillan:

```typescript
import { Controller, Post, Body, UsePipes, ParseArrayPipe } from '@nestjs/common';
import { ValidationPipe, ParseIntPipe } from '@nestjs/common';

@Controller('numbers')
export class NumbersController {
  @Post()
  @UsePipes(new ParseArrayPipe({ items: Number }), ParseIntPipe, ValidationPipe)
  validateNumbers(@Body() numbers: number[]) {
    return `Numbers are valid: ${numbers}`;
  }
}
```

## 🎓 Conclusión

Los pipes en NestJS son más que simples herramientas de validación. Son una forma poderosa de garantizar que los datos entrantes sean correctos y estén en el formato adecuado antes de ser procesados por tu aplicación. Con su capacidad de transformar, validar y manejar errores, los pipes permiten construir aplicaciones más robustas, seguras y fáciles de mantener.

Aprovechar al máximo los pipes te permitirá crear aplicaciones que no solo sean funcionales, sino también elegantes y a prueba de fallos. ¡Domina los pipes y lleva tus aplicaciones NestJS al siguiente nivel!
