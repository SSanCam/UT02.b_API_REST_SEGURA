# Refugio Kimba

### Desarrollo web en Entorno Servidor
### UT02-b Spring Boot | Proyecto – API REST Segura
### __Sara Sánchez Camilleri.__

<hr>
<br>
<br>

# 1. Idea del proyecto: API REST para una protectora de animales

Con vistas al Trabajo de Fin de Curso (TFC), este proyecto será una práctica que puede evolucionar para convertirse en una herramienta funcional o incluso en una parte del proyecto final.

La idea es desarrollar una **aplicación web para una protectora de animales**, cuyo objetivo principal será facilitar la gestión de las instalaciones y los animales en busca de adopción, además de promover la participación de voluntarios.

## Características principales

### Web App
Diseñada para que los administradores y trabajadores gestionen la información de manera centralizada. Desde aquí, podrán:
- Actualizar el estado de los animales (en adopción, apadrinados, bajo tratamiento, etc.).
- Gestionar usuarios y voluntarios.
- Supervisar las donaciones y el interés de posibles adoptantes.

### App Móvil
Una herramienta sencilla enfocada principalmente a los **trabajadores del refugio** (usuarios admin) para:
- Subir información de nuevos animales de forma ágil y directa.
- Actualizar el estado de los registros existentes.

## Arquitectura
El proyecto utilizará una **base de datos propia** para almacenar toda la información necesaria. Los datos serán accesibles a través de una **API REST**, lo que asegurará:
- La interoperabilidad entre la aplicación web y la app móvil.
- Actualizaciones en tiempo real, facilitando la sincronización de datos entre las plataformas.

## Propósito
El propósito final es optimizar la comunicación y el flujo de trabajo en la protectora, creando una herramienta que permita:
- Gestionar de manera eficiente los recursos del refugio.
- Maximizar el alcance de la misión del refugio para captar más adoptantes y voluntarios.

<hr>
<br>
<br>

# 2. Planteamiento de las entidades.

## Entidades básicas
Para garantizar un mínimo viable funcional, el proyecto trabajará inicialmente con las siguientes entidades fundamentales:

- **Usuarios**: Representarán a las personas que interactúan con el sistema. Los usuarios estarán clasificados en distintos roles según sus responsabilidades y actividades: administrador, voluntario, padrino, usuario regular, entre otros. Como roles básicos tendremos `administrador` y un `usuario
 que será genérico, conforme se vayan ampliando las funcionalidades de la propia app web, se irán incrementando los roles necesarios. 


- **Animales**: Contendrá la información detallada de todos los animales registrados por la protectora, incluyendo datos básicos, estado de salud, y disponibilidad para adopción o apadrinamiento.


- **Adopciones**: Gestionará y registrará la información relacionada con las adopciones realizadas, permitiendo un seguimiento detallado de los animales que han salido del refugio hacia un hogar permanente.

## Ampliación en entidades: roles.
Conforme avance el desarrollo del proyecto, se integrarán nuevas entidades para ampliar las funcionalidades y proporcionar herramientas adicionales tanto para los usuarios como para el personal del centro. Estas futuras incorporaciones están diseñadas para mejorar la experiencia de usuario y optimizar los procesos internos del refugio. Las entidades adicionales son las siguientes:

- **Apadrinamiento**: Permitirá registrar la información de los animales que han sido apadrinados, asociándolos con los usuarios responsables. Esta funcionalidad facilitará la comunicación con los padrinos mediante el envío de notificaciones personalizadas sobre el estado y las necesidades del animal.


- **Cita Veterinaria**: Llevará un registro detallado de las citas veterinarias asignadas a cada animal mientras permanezcan bajo la tutela del refugio. Esta funcionalidad incluirá datos como fecha, motivo, veterinario asignado y resultados de la visita.


- **Donaciones**: Administrará el registro de las donaciones realizadas al refugio, ya sea por usuarios registrados o donantes anónimos. Este sistema facilitará el seguimiento de los aportes económicos y permitirá generar informes para la gestión de recursos.


- **Visitas al Centro**: Gestionará las actividades organizadas por el refugio, como jornadas de adopción, eventos educativos en colaboración con escuelas u otras instituciones, y visitas generales. Esta funcionalidad permitirá al centro organizar y registrar la participación en estas actividades de manera eficiente.

<hr>
<br>
<br>

## 3. Descripción de las entidades

### _Usuarios_
- **Campos principales**:
    - `id_usuario`: **INT** (PK, AUTO_INCREMENT).
    - `nombre`: **VARCHAR**(100) (NOT NULL).
    - `email`: **VARCHAR**(255) (NOT NULL, UNIQUE).
    - `telefono`: **VARCHAR**(9) NOT NULL CHECK (telefono ~ '^[0-9]{9}$').
    - `contraseña`: **VARCHAR**(255) (NOT NULL, almacenada como hash). 
    - `rol` ENUM('administrador', 'voluntario', 'padrino', 'adoptante', 'genérico') NOT NULL

- **Roles**:
    - Relación N:M con una tabla `Roles` para asignar uno o más roles a un usuario.
    - Roles disponibles:
        - Administrador: Serán capaces de `crear`, `leer`, `modificar` y `eliminar` registros de la base de datos, tanto en la tabla `Animales` como la de `Adopciones` .
        - Genérico: Sólo serán capaces de `leer` parte de los registros, como información genérica, sobre los animales.

### _Animales_
- **Campos principales**:
    - `id_animal`: **INT** (PK, AUTO_INCREMENT).
    - `nombre`: **VARCHAR(100)** (NOT NULL).
    - `tipo_animal`: **ENUM**(`perro`, `gato`) (NOT NULL).
    - `estado`: **ENUM**(`en adopción`, `apadrinado`, `en tratamiento`, `adoptado`) (NOT NULL).


### _Adopciones_
**Descripción**: La entidad `Adopciones` gestiona y registra la información relacionada con los procesos de adopción realizados por los usuarios. Cada adopción vincula un usuario adoptante con un animal específico, permitiendo llevar un seguimiento detallado.

**Campos principales**:
- `id_adopcion`: **INT** (PK, AUTO_INCREMENT)  
- `id_usuario`: **INT** (FK, NOT NULL, referencia a `Usuario.id_usuario`)  
- `id_animal`: **INT** (FK, referencia a `Animal.id_animal`)
- `fecha_adopcion`: **DATE**  (NOT, NULL)
- `observaciones`: **TEXT**  
  **Restricción**: Una vez se formaliza el proceso de adopción, el registro del animal debe ser acualizado para que se cambie el campo de `estado`.

**Relaciones**:
- **Usuario y Adopciones**: Relación 1:N (Un usuario puede adoptar múltiples animales).
- **Animal y Adopciones**: Relación 1:1 (Cada animal sólo puede tener un único adoptante en el registro veterinario).

<hr>
<br>
<br>

# 4. Diagrama Entidad/Relación

![Diagrama Entidad/Relación](./src/main/resources/images/UT02_API_REST_SEGURA.jpg)
