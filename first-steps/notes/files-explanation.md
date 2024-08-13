# 📁 Estructura de Archivos en una Aplicación NestJS

## **src/**: Carpeta principal del código fuente.
- **app.controller.ts**: 
    - **Descripción**: Define un controlador básico que maneja las rutas y métodos HTTP.
    - **Rol en la Arquitectura Limpia**: **Capa de Presentación**
    - **Propósito**: Dentro de la Arquitectura Limpia, el controlador actúa como la interfaz entre el mundo exterior y la lógica interna de la aplicación. Se encarga de recibir las solicitudes HTTP, delegar la lógica de negocio a los servicios y retornar las respuestas al cliente. No debería contener lógica de negocio, su función es coordinar y orquestar la interacción entre las capas.
    - **Ejemplo Real**: Imagina una aplicación de reserva de vuelos. El controlador podría manejar una solicitud `GET /vuelos` para listar todos los vuelos disponibles. El controlador recibe la solicitud, llama a un servicio que obtiene los datos de vuelos, y luego envía la respuesta con esos datos.

- **app.module.ts**: 
    - **Descripción**: El módulo raíz de la aplicación que organiza otros módulos necesarios.
    - **Rol en la Arquitectura Limpia**: **Capa de Configuración**
    - **Propósito**: Este archivo agrupa y organiza la configuración y los módulos de la aplicación. En la Arquitectura Limpia, actúa como la capa de configuración que ensambla los módulos, controladores, y servicios, definiendo cómo se interrelacionan sin involucrarse en la lógica de negocio.
    - **Ejemplo Real**: Siguiendo con la aplicación de reserva de vuelos, `app.module.ts` podría importar módulos como `VuelosModule`, `UsuariosModule`, y `ReservasModule`, asegurándose de que todas las partes de la aplicación estén correctamente conectadas.

- **app.service.ts**: 
    - **Descripción**: Un servicio básico que contiene la lógica de negocio de la aplicación.
    - **Rol en la Arquitectura Limpia**: **Capa de Aplicación (Reglas de Negocio)**
    - **Propósito**: Los servicios se sitúan en el núcleo de la lógica de negocio. Este archivo se encarga de implementar las reglas de negocio, procesar la lógica interna, y manipular los datos antes de que sean devueltos al controlador. En la Arquitectura Limpia, los servicios deberían ser independientes de las interfaces externas como los controladores o bases de datos, permitiendo que la lógica de negocio se mantenga aislada y fácil de probar.
    - **Ejemplo Real**: En la aplicación de vuelos, `app.service.ts` podría tener un método `obtenerVuelosDisponibles()` que consulta la base de datos para obtener todos los vuelos que aún tienen asientos disponibles y cumplen con los criterios de búsqueda del usuario.

- **main.ts**: 
    - **Descripción**: El punto de entrada de la aplicación.
    - **Rol en la Arquitectura Limpia**: **Capa de Configuración**
    - **Propósito**: Aquí es donde se configura y arranca el servidor de la aplicación. En términos de Arquitectura Limpia, `main.ts` configura las capas externas, como la red (HTTP), y orquesta la inicialización de la aplicación sin involucrarse en la lógica de negocio. Es el archivo que conecta todos los componentes antes de que la aplicación comience a procesar solicitudes.
    - **Ejemplo Real**: En el caso de la aplicación de reserva de vuelos, `main.ts` se encargaría de arrancar el servidor que escuche en un puerto específico (por ejemplo, 3000) y de aplicar configuraciones como CORS o middleware de seguridad.

## **test/**: Carpeta destinada a las pruebas unitarias y de extremo a extremo (e2e).
- **app.e2e-spec.ts**: 
    - **Descripción**: Archivo de prueba de extremo a extremo (e2e) para el controlador principal.
    - **Rol en la Arquitectura Limpia**: **Capa de Configuración / Testeo**
    - **Propósito**: Este archivo contiene pruebas que verifican el comportamiento de la aplicación desde una perspectiva de usuario final, evaluando cómo interactúan las capas sin entrar en detalles de implementación. Asegura que las interfaces públicas de la aplicación funcionen correctamente cuando se integran con las capas internas.
    - **Ejemplo Real**: En la aplicación de vuelos, este archivo podría contener una prueba que simula una solicitud `GET /vuelos` y verifica que la respuesta incluya una lista de vuelos con el formato y contenido correctos.

- **jest-e2e.json**: 
    - **Descripción**: Configuración de Jest para pruebas e2e.
    - **Rol en la Arquitectura Limpia**: **Capa de Configuración / Testeo**
    - **Propósito**: Define cómo Jest ejecuta las pruebas e2e. Este archivo pertenece a la capa de configuración, asegurando que las pruebas se ejecuten con los parámetros y el entorno adecuados.
    - **Ejemplo Real**: En el proyecto de vuelos, este archivo configura Jest para que ejecute las pruebas de extremo a extremo, garantizando que todos los componentes (controladores, servicios, bases de datos) funcionen correctamente cuando se integran.

## **node_modules/**: Carpeta donde se instalan las dependencias del proyecto.
- **Descripción**: Contiene todas las dependencias de Node.js necesarias para que la aplicación funcione.
- **Rol en la Arquitectura Limpia**: **Capa de Infraestructura**
- **Propósito**: Las dependencias externas y bibliotecas que soportan la infraestructura de la aplicación se ubican aquí. En la Arquitectura Limpia, estas dependencias deben mantenerse alejadas de la lógica de negocio central para evitar acoplamientos innecesarios.
- **Ejemplo Real**: En la aplicación de vuelos, `node_modules/` contendrá paquetes como Express, TypeORM, o cualquier otra biblioteca que se use para manejar peticiones HTTP, conectar con bases de datos, etc.

## **dist/**: Carpeta donde se genera el código compilado (TypeScript a JavaScript).
- **Descripción**: Contiene el código JavaScript compilado que se ejecuta en el entorno de producción.
- **Rol en la Arquitectura Limpia**: **Capa de Infraestructura / Implementación**
- **Propósito**: Este es el resultado final de la construcción de la aplicación. La lógica de negocio y las configuraciones son compiladas en un formato que puede ser ejecutado por Node.js en un entorno de producción.
- **Ejemplo Real**: Para la aplicación de vuelos, `dist/` contendría el código JavaScript listo para ser desplegado en un servidor de producción, incluyendo todos los módulos y servicios necesarios para que la aplicación funcione.

## **package.json**: Archivo de configuración de npm que incluye scripts, dependencias y metadatos del proyecto.
- **Descripción**: Especifica la configuración y las dependencias del proyecto.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Centraliza la gestión de dependencias, scripts y metadatos del proyecto. Facilita la configuración y el despliegue del entorno de desarrollo y producción.
- **Ejemplo Real**: En la aplicación de vuelos, `package.json` especificará todas las bibliotecas necesarias (por ejemplo, `@nestjs/core`, `@nestjs/common`) y definirá scripts como `start` para iniciar la aplicación o `test` para ejecutar las pruebas.

## **tsconfig.json**: Configuración de TypeScript para el proyecto.
- **Descripción**: Define cómo se debe compilar el código TypeScript a JavaScript.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Controla la compilación de TypeScript, asegurando que el código sea transformado correctamente a JavaScript y que se respeten las reglas y convenciones establecidas para el proyecto.
- **Ejemplo Real**: Para la aplicación de vuelos, `tsconfig.json` podría incluir configuraciones específicas para usar decoradores, módulos ES6, y establecer la carpeta de salida como `dist`.

## **tsconfig.build.json**: Configuración específica de TypeScript para la construcción del proyecto.
- **Descripción**: Configuración específica para la compilación del proyecto en un entorno de producción.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Permite una configuración separada para la compilación final del proyecto, optimizando la salida para el entorno de producción y minimizando errores.
- **Ejemplo Real**: En la aplicación de vuelos, `tsconfig.build.json` puede optimizar la compilación al excluir archivos de prueba y generar código JavaScript minificado para un rendimiento óptimo en producción.

## **.eslintrc.js**: Configuración de ESLint para mantener la calidad del código.
- **Descripción**: Define reglas para analizar y asegurar la calidad del código.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Garantiza que el código sigue las mejores prácticas de estilo y calidad, ayudando a prevenir errores comunes y manteniendo un código consistente.
- **Ejemplo Real**: En la aplicación de vuelos, `.eslintrc.js` puede configurar reglas específicas, como asegurar que todas las variables están tipadas correctamente y que no se dejan console.logs en el código final.

## **.prettierrc**: Configuración de Prettier para formatear el código.
- **Descripción**: Define reglas para formatear automáticamente el código.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Mantiene un estilo de código consistente y legible en todo el proyecto, lo que facilita la colaboración y reduce la fricción en el desarrollo.

## **.gitignore**: Lista de archivos y carpetas que Git debe ignorar.
- **Descripción**: Especifica qué archivos y carpetas no deben ser seguidos por el control de versiones.
- **Rol en la Arquitectura Limpia**: **Capa de Configuración**
- **Propósito**: Evita que archivos innecesarios, como dependencias instaladas o archivos temporales, sean incluidos en el repositorio, manteniendo el control de versiones limpio y manejable.

## **README.md**: Archivo de documentación inicial del proyecto.
- **Descripción**: Contiene la documentación básica sobre el proyecto.
- **Rol en la Arquitectura Limpia**: **Capa de Documentación**
- **Propósito**: Proporciona una visión general del proyecto, instrucciones de configuración, uso, y cualquier otra información relevante. Es el primer punto de referencia para nuevos desarrolladores o colaboradores.