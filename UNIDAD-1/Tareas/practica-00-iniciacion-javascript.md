# Práctica 0. Preparación del repositorio y toma de contacto con JavaScript

## Introducción

Durante el módulo utilizaremos un repositorio de GitHub para guardar las actividades, ejercicios y proyectos realizados.

Este repositorio funcionará como:

- Cuaderno de trabajo del módulo.
- Copia de seguridad.
- Evidencia del trabajo realizado.
- Portfolio personal.
- Espacio para practicar Git, GitHub y Markdown.

En esta primera práctica crearás y organizarás el repositorio del módulo. Después realizarás dos ejercicios muy básicos para tener una primera toma de contacto con JavaScript.

> No es necesario haber utilizado JavaScript anteriormente. Todos los ejercicios están guiados.

## Objetivos

Al finalizar la práctica deberás haber conseguido:

- Crear un repositorio en GitHub.
- Configurar correctamente su visibilidad.
- Clonar y abrir el repositorio con Visual Studio Code.
- Crear la estructura inicial del módulo.
- Ejecutar un archivo JavaScript.
- Reconocer variables, textos, números y operaciones básicas.
- Mostrar información en la consola.
- Guardar y subir los cambios mediante Git.
- Comprobar que la profesora puede acceder al repositorio.

---

## Parte 1. Creación del repositorio

### 1. Crea el repositorio

Crea en tu cuenta de GitHub un repositorio con el siguiente nombre:

```text
pgl-2dam
```

No es necesario añadir tu nombre, ya que el repositorio estará asociado a tu cuenta personal de GitHub.

Al crearlo:

- Añade un archivo `README.md`.
- No añadas ninguna licencia.
- Puedes seleccionar la plantilla de `.gitignore` para Node.
- Decide si el repositorio será público o privado.

### 2. Configura el acceso de la profesora

La profesora debe poder acceder al contenido del repositorio durante todo el curso. Tienes dos opciones:

#### Opción A. Repositorio público

Selecciona la visibilidad `Public`. La profesora podrá acceder directamente mediante el enlace del repositorio.

#### Opción B. Repositorio privado

Si prefieres mantenerlo privado, deberás añadir a la profesora como colaboradora:

1. Accede al repositorio.
2. Entra en `Settings`.
3. Selecciona `Collaborators`.
4. Pulsa `Add people`.
5. Introduce el usuario de GitHub indicado por la profesora.
6. Envía la invitación.

Antes de entregar, comprueba que el repositorio es público o que la invitación se ha enviado correctamente.

---

## Parte 2. Descarga y organización

### 3. Clona el repositorio

Pulsa el botón `Code` de GitHub y copia la dirección HTTPS. Abre una terminal en la carpeta donde guardarás tus proyectos y ejecuta:

```bash
git clone URL-DEL-REPOSITORIO
```

Por ejemplo:

```bash
git clone https://github.com/usuario/pgl-2dam.git
```

Entra en la carpeta y abre el proyecto:

```bash
cd pgl-2dam
code .
```

Si el último comando no funciona, abre Visual Studio Code y utiliza `Archivo → Abrir carpeta`.

### 4. Crea la estructura inicial

```text
pgl-2dam/
├── README.md
├── tema-0-introduccion/
│   └── practica-00-iniciacion-javascript/
│       ├── README.md
│       ├── ejercicio-01.js
│       └── ejercicio-02.js
├── ut1-fundamentos/
├── ut2-react-native/
├── ut3-multimedia/
├── ut4-motores-videojuegos/
└── ut5-videojuego-unity/
```

Git no guarda carpetas vacías. Para que las carpetas de las unidades aparezcan en GitHub, crea dentro de cada una un archivo vacío llamado `.gitkeep`.

### 5. Completa el README principal

Abre el archivo `README.md` situado en la raíz y añade:

```markdown
# Programación multimedia y dispositivos móviles

Repositorio de actividades y proyectos del módulo de Programación multimedia y dispositivos móviles.

## Datos

- Alumno/a:
- Curso: 2.º DAM
- Curso académico: 2026/2027

## Contenidos

### Tema 0. Introducción

- [Práctica 00. Iniciación a JavaScript](tema-0-introduccion/practica-00-iniciacion-javascript/README.md)

### UT1. Fundamentos y tecnologías para el desarrollo multiplataforma

Próximamente.

### UT2. Desarrollo de aplicaciones móviles con React Native

Próximamente.

### UT3. Multimedia y animaciones en aplicaciones móviles

Próximamente.

### UT4. Introducción a los motores de videojuegos

Próximamente.

### UT5. Desarrollo de un videojuego con Unity

Próximamente.
```

### 6. Realiza el primer commit

```bash
git status
git add .
git commit -m "Crea la estructura inicial del repositorio"
git push
```

Entra en GitHub y comprueba que los archivos aparecen correctamente.

---

## Parte 3. Primera toma de contacto con JavaScript

### ¿Qué es JavaScript?

JavaScript es un lenguaje de programación que permite escribir instrucciones que el ordenador ejecutará en el orden indicado.

Durante el módulo utilizaremos JavaScript y TypeScript como base para desarrollar aplicaciones con React y React Native. Los archivos de JavaScript utilizan la extensión `.js`.

En esta práctica trabajaremos únicamente con la consola. Todavía no crearemos interfaces ni páginas web.

### Antes de empezar: comprobar Node.js

Abre la terminal de Visual Studio Code y escribe:

```bash
node --version
```

Si Node.js está instalado, aparecerá un número de versión. Si aparece un mensaje de error, avisa a la profesora.

Para ejecutar un archivo JavaScript utilizaremos:

```bash
node nombre-del-archivo.js
```

---

## Ejercicio 1. Mi primer programa

Abre `ejercicio-01.js`.

### Paso 1. Mostrar un mensaje

```javascript
console.log("Hola, JavaScript");
```

`console.log()` es una instrucción que permite mostrar información en la consola. Guarda el archivo y ejecútalo desde la raíz del repositorio:

```bash
node tema-0-introduccion/practica-00-iniciacion-javascript/ejercicio-01.js
```

Deberá aparecer:

```text
Hola, JavaScript
```

### Paso 2. Mostrar varios mensajes

```javascript
console.log("Estoy preparando mi primera Pokédex");
console.log("Durante el curso utilizaremos JavaScript y TypeScript");
```

Las instrucciones se ejecutan de arriba abajo.

### Paso 3. Crear variables

Una variable permite guardar un dato para utilizarlo posteriormente:

```javascript
const nombre = "Pikachu";
const tipo = "eléctrico";
const nivel = 5;
```

- `"Pikachu"` y `"eléctrico"` son textos.
- `5` es un número.
- `const` permite crear una variable cuyo valor no vamos a reasignar.
- Cada instrucción termina con `;`.

### Paso 4. Mostrar las variables

```javascript
console.log(nombre);
console.log(tipo);
console.log(nivel);
```

También puedes combinar texto y variables mediante una plantilla de texto:

```javascript
console.log(`${nombre} es de tipo ${tipo} y tiene nivel ${nivel}.`);
```

Las plantillas utilizan acentos graves: `` ` ``.

### Paso 5. Cambia los datos

Cambia los valores anteriores por los de otro Pokémon. Por ejemplo:

```javascript
const nombre = "Charmander";
const tipo = "fuego";
const nivel = 8;
```

Ejecuta nuevamente el archivo y observa el resultado.

---

## Ejercicio 2. Operaciones y decisiones básicas

Abre `ejercicio-02.js`.

### Paso 1. Declara los datos

```javascript
const nombre = "Bulbasaur";
const nivel = 10;
const experienciaActual = 70;
const experienciaGanada = 30;
```

### Paso 2. Realiza una operación

```javascript
const experienciaTotal = experienciaActual + experienciaGanada;

console.log(`${nombre} tenía ${experienciaActual} puntos de experiencia.`);
console.log(`${nombre} ha ganado ${experienciaGanada} puntos.`);
console.log(`Ahora tiene ${experienciaTotal} puntos de experiencia.`);
```

### Paso 3. Comprueba una condición

Una condición permite que el programa tome una decisión:

```javascript
if (experienciaTotal >= 100) {
    console.log(`${nombre} puede subir de nivel.`);
} else {
    console.log(`${nombre} todavía no puede subir de nivel.`);
}
```

La condición `experienciaTotal >= 100` pregunta si la experiencia total es mayor o igual que `100`.

- Si se cumple, se ejecuta el bloque de `if`.
- Si no se cumple, se ejecuta el bloque de `else`.

### Paso 4. Experimenta

Modifica `experienciaActual` y `experienciaGanada`. Prueba distintas cantidades hasta conseguir que se ejecuten los dos posibles mensajes. Deja finalmente los valores que prefieras.

---

## Parte 4. Documentación

Completa `tema-0-introduccion/practica-00-iniciacion-javascript/README.md` con esta plantilla:

```markdown
# Práctica 00. Iniciación a JavaScript

## Objetivos

En esta práctica he preparado el repositorio del módulo y he tenido una primera toma de contacto con JavaScript.

## Ejercicio 1. Mi primer programa

Explica brevemente qué has hecho.

## Ejercicio 2. Operaciones básicas

Explica qué operación y qué condición utiliza el programa.

## Conceptos utilizados

- `console.log()`:
- Variable:
- Texto:
- Número:
- Condición:

## Dificultades encontradas

Indica si has encontrado algún problema y cómo lo has solucionado.

## Conclusión

Explica brevemente qué has aprendido.
```

Las explicaciones deben estar escritas con tus propias palabras. No es necesario que sean extensas.

---

## Parte 5. Entrega

Guarda los cambios y ejecuta:

```bash
git status
git add .
git commit -m "Completa la práctica inicial de JavaScript"
git push
```

Entrega en Moodle el enlace al repositorio:

```text
https://github.com/usuario/pgl-2dam
```

## Lista de comprobación

- [ ] El repositorio se llama `pgl-2dam`.
- [ ] El repositorio es público o la profesora ha sido añadida como colaboradora.
- [ ] El `README.md` principal está completado.
- [ ] La estructura de carpetas es correcta.
- [ ] `ejercicio-01.js` funciona sin errores.
- [ ] `ejercicio-02.js` funciona sin errores.
- [ ] El `README.md` de la práctica está completado.
- [ ] Se han realizado al menos dos commits descriptivos.
- [ ] Todos los cambios aparecen en GitHub.
- [ ] Se ha entregado el enlace del repositorio en Moodle.

