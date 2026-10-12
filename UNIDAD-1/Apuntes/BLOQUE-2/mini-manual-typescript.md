# TypeScript aplicado

## Sumario

- [Introducción](#introducción)
- [Cómo ejecutar los ejemplos](#cómo-ejecutar-los-ejemplos)
- [Tipos básicos](#tipos-básicos)
  - [Inferencia de tipos](#inferencia-de-tipos)
  - [El tipo `any`](#el-tipo-any)
- [Arrays tipados](#arrays-tipados)
- [Uniones de tipos](#uniones-de-tipos)
- [Funciones tipadas](#funciones-tipadas)
  - [Parámetros opcionales](#parámetros-opcionales)
- [Tipos personalizados](#tipos-personalizados)
- [Interfaces](#interfaces)
  - [Propiedades opcionales](#propiedades-opcionales)
- [Arrays de objetos](#arrays-de-objetos)
  - [`map`](#map)
  - [`filter`](#filter)
  - [`find`](#find)
- [Ejercicio final guiado](#ejercicio-final-guiado)
- [Errores frecuentes](#errores-frecuentes)
- [Resumen](#resumen)

---

## Introducción

TypeScript es un lenguaje basado en JavaScript que añade la posibilidad de indicar los tipos de los datos.

El navegador no ejecuta TypeScript directamente. El código TypeScript se transforma en JavaScript antes de ejecutarse.

JavaScript permite hacer lo siguiente:

```javascript
let precio = 20;
precio = "veinte";
```

La variable empieza conteniendo un número y después pasa a contener texto. Esto puede producir errores difíciles de detectar.

En TypeScript podemos indicar que `precio` debe ser siempre un número:

```typescript
let precio: number = 20;
precio = "veinte";
```

La segunda asignación produce un error antes de ejecutar el programa:

```text
Type 'string' is not assignable to type 'number'.
```

TypeScript no evita todos los errores, pero permite encontrar muchos de ellos mientras escribimos el código.

### Primera prueba

```typescript
const modulo: string = "Programación multimedia";
const horas: number = 6;
const terminado: boolean = false;

console.log(modulo);
console.log(horas);
console.log(terminado);
```

Prueba a cambiar `horas` por un texto:

```typescript
const horas: number = "seis";
```

Observa el error que muestra TypeScript.

---

## Cómo ejecutar los ejemplos

Para esta introducción utilizaremos **TypeScript Playground**:

1. Accede a [https://www.typescriptlang.org/play](https://www.typescriptlang.org/play).
2. Borra el código que aparezca en el editor.
3. Copia uno de los ejemplos del manual.
4. Observa si aparecen errores subrayados en rojo.
5. Pulsa **Run** para ejecutarlo.
6. Consulta el resultado en la pestaña **Logs**.

En la parte derecha se puede observar el JavaScript generado a partir del código TypeScript.

> TypeScript Playground se utilizará para aprender el lenguaje sin dedicar la sesión a instalar o configurar herramientas. En los proyectos de React trabajaremos con archivos `.ts` y `.tsx` desde Visual Studio Code.

---

## Tipos básicos

Los tipos básicos que utilizaremos con más frecuencia son:

| Tipo | Contenido | Ejemplo |
|---|---|---|
| `string` | Texto | `"Hollow Knight"` |
| `number` | Números enteros o decimales | `19.99` |
| `boolean` | Verdadero o falso | `true` |
| `null` | Ausencia intencionada de valor | `null` |
| `undefined` | Valor todavía no definido | `undefined` |

Ejemplo:

```typescript
let titulo: string = "Hollow Knight";
let precio: number = 14.99;
let disponible: boolean = true;

console.log(`${titulo} cuesta ${precio} €`);
console.log(`¿Está disponible? ${disponible}`);
```

Los nombres de los tipos básicos se escriben en minúscula: `string`, `number` y `boolean`.

### Prueba guiada

Declara las siguientes variables:

```typescript
const nombreJugador: string = "Marta";
const nivel: number = 12;
const conectado: boolean = true;
```

Muestra este mensaje utilizando una plantilla de texto:

```typescript
console.log(`${nombreJugador} está en el nivel ${nivel}`);
```

Resultado esperado:

```text
Marta está en el nivel 12
```

### Inferencia de tipos

No siempre es necesario escribir el tipo. TypeScript puede deducirlo a partir del valor inicial.

```typescript
let puntos = 100;
```

TypeScript infiere que `puntos` es de tipo `number`. Por tanto, esto producirá un error:

```typescript
puntos = "cien";
```

Ambas declaraciones representan una variable numérica:

```typescript
let vidas: number = 3;
let energia = 100;
```

Escribiremos el tipo cuando ayude a comprender el código o cuando TypeScript no pueda deducirlo correctamente.

### El tipo `any`

El tipo `any` permite almacenar cualquier valor:

```typescript
let dato: any = 10;
dato = "texto";
dato = true;
```

Aunque funciona, estamos desactivando gran parte de la ayuda que ofrece TypeScript.

```typescript
let precio: any = "gratis";
console.log(precio.toFixed(2));
```

TypeScript no avisa del problema, pero el programa fallará al ejecutar `toFixed()` sobre un texto.

> Debemos evitar `any` salvo que exista una razón concreta para utilizarlo.

---

## Arrays tipados

Un array puede limitarse a un tipo de dato:

```typescript
const puntuaciones: number[] = [100, 80, 95];
const plataformas: string[] = ["PC", "Switch", "PlayStation"];
```

También puede utilizarse la sintaxis `Array<tipo>`:

```typescript
const puntuaciones: Array<number> = [100, 80, 95];
```

Las dos formas son equivalentes.

Si intentamos añadir un dato incorrecto, TypeScript muestra un error:

```typescript
plataformas.push(25);
```

Podemos recorrer el array:

```typescript
for (const plataforma of plataformas) {
  console.log(plataforma);
}
```

### Prueba guiada

```typescript
const generos: string[] = ["Aventura", "Rol", "Plataformas"];

generos.push("Estrategia");

for (const genero of generos) {
  console.log(genero);
}
```

Prueba después a añadir un número al array y observa el error.

---

## Uniones de tipos

En ocasiones una variable puede admitir más de un tipo. Para indicarlo utilizamos `|`:

```typescript
let identificador: number | string;

identificador = 25;
identificador = "juego-25";
```

Un tipo unión también puede limitar los valores permitidos:

```typescript
type Estado = "pendiente" | "instalado" | "completado";

let estadoJuego: Estado = "instalado";
```

Esto produciría un error:

```typescript
estadoJuego = "jugando";
```

`"jugando"` es un texto, pero no forma parte de los tres valores admitidos por `Estado`.

### Prueba guiada

```typescript
type Dificultad = "fácil" | "normal" | "difícil";

const dificultadActual: Dificultad = "normal";
console.log(dificultadActual);
```

Prueba a asignar un valor diferente de los tres permitidos.

---

## Funciones tipadas

Podemos indicar el tipo de los parámetros y el tipo del valor que devuelve una función:

```typescript
function calcularDescuento(precio: number, porcentaje: number): number {
  return precio - precio * (porcentaje / 100);
}

const precioFinal = calcularDescuento(50, 20);
console.log(precioFinal);
```

La función recibe dos números y devuelve otro número.

Esto produciría un error:

```typescript
calcularDescuento("cincuenta", 20);
```

Si una función no devuelve ningún valor, podemos indicar `void`:

```typescript
function mostrarMensaje(mensaje: string): void {
  console.log(mensaje);
}
```

También podemos tipar funciones flecha:

```typescript
const calcularIva = (precio: number): number => {
  return precio * 0.07;
};
```

### Prueba guiada

Completa y ejecuta esta función:

```typescript
function calcularPuntuacion(
  victorias: number,
  derrotas: number
): number {
  return victorias * 3 - derrotas;
}

console.log(calcularPuntuacion(5, 2));
```

Resultado esperado:

```text
13
```

### Parámetros opcionales

Un parámetro opcional se marca con `?`:

```typescript
function saludarJugador(nombre: string, apodo?: string): string {
  if (apodo) {
    return `Hola, ${nombre}, también conocido como ${apodo}`;
  }

  return `Hola, ${nombre}`;
}

console.log(saludarJugador("Daniel"));
console.log(saludarJugador("Daniel", "Shadow"));
```

---

## Tipos personalizados

La palabra `type` permite poner nombre a un tipo propio:

```typescript
type Plataforma = "PC" | "Switch" | "PlayStation" | "Xbox";
type Identificador = number | string;
```

También permite describir la forma de un objeto:

```typescript
type Jugador = {
  nombre: string;
  nivel: number;
  conectado: boolean;
};

const jugador: Jugador = {
  nombre: "Lucía",
  nivel: 8,
  conectado: true,
};
```

Si falta una propiedad obligatoria o se utiliza un tipo incorrecto, TypeScript muestra un error.

---

## Interfaces

Una interfaz describe la estructura que debe tener un objeto.

```typescript
interface Videojuego {
  id: number;
  titulo: string;
  genero: string;
  precio: number;
  disponible: boolean;
}
```

Ahora podemos crear objetos que cumplan ese contrato:

```typescript
const juego: Videojuego = {
  id: 1,
  titulo: "Hollow Knight",
  genero: "Metroidvania",
  precio: 14.99,
  disponible: true,
};
```

Si escribimos un precio como texto, TypeScript avisará:

```typescript
const juegoIncorrecto: Videojuego = {
  id: 2,
  titulo: "Celeste",
  genero: "Plataformas",
  precio: "veinte",
  disponible: true,
};
```

Las interfaces no crean objetos por sí mismas. Únicamente describen qué propiedades y tipos esperamos encontrar.

### Propiedades opcionales

Podemos indicar que una propiedad no es obligatoria utilizando `?`:

```typescript
interface Videojuego {
  id: number;
  titulo: string;
  genero: string;
  precio: number;
  disponible: boolean;
  plataforma?: string;
}
```

Los dos objetos siguientes son válidos:

```typescript
const juego1: Videojuego = {
  id: 1,
  titulo: "Hollow Knight",
  genero: "Metroidvania",
  precio: 14.99,
  disponible: true,
  plataforma: "PC",
};

const juego2: Videojuego = {
  id: 2,
  titulo: "Celeste",
  genero: "Plataformas",
  precio: 19.99,
  disponible: true,
};
```

---

## Arrays de objetos

Podemos combinar interfaces y arrays:

```typescript
interface Videojuego {
  id: number;
  titulo: string;
  genero: string;
  precio: number;
  disponible: boolean;
  plataforma?: string;
}

const videojuegos: Videojuego[] = [
  {
    id: 1,
    titulo: "Hollow Knight",
    genero: "Metroidvania",
    precio: 14.99,
    disponible: true,
    plataforma: "PC",
  },
  {
    id: 2,
    titulo: "Celeste",
    genero: "Plataformas",
    precio: 19.99,
    disponible: true,
  },
  {
    id: 3,
    titulo: "The Legend of Zelda",
    genero: "Aventura",
    precio: 59.99,
    disponible: false,
    plataforma: "Switch",
  },
];
```

### `map`

`map()` transforma cada elemento de un array y devuelve un array nuevo.

```typescript
const titulos: string[] = videojuegos.map(
  (videojuego: Videojuego): string => videojuego.titulo
);

console.log(titulos);
```

Resultado:

```text
["Hollow Knight", "Celeste", "The Legend of Zelda"]
```

TypeScript puede inferir el tipo del parámetro, por lo que normalmente bastaría con:

```typescript
const titulos = videojuegos.map((videojuego) => videojuego.titulo);
```

### `filter`

`filter()` devuelve un array con los elementos que cumplen una condición:

```typescript
const disponibles: Videojuego[] = videojuegos.filter(
  (videojuego) => videojuego.disponible
);

console.log(disponibles);
```

Podemos filtrar por precio:

```typescript
const economicos = videojuegos.filter(
  (videojuego) => videojuego.precio < 20
);
```

### `find`

`find()` devuelve el primer elemento que cumple una condición:

```typescript
const encontrado = videojuegos.find(
  (videojuego) => videojuego.id === 2
);
```

Si no encuentra nada, devuelve `undefined`. Por eso el tipo inferido es:

```typescript
Videojuego | undefined
```

Debemos comprobar el resultado antes de utilizarlo:

```typescript
if (encontrado) {
  console.log(encontrado.titulo);
} else {
  console.log("No se ha encontrado el videojuego");
}
```

También podemos utilizar encadenamiento opcional:

```typescript
console.log(encontrado?.titulo);
```

---

## Ejercicio final guiado

Vamos a construir un pequeño catálogo tipado.

### Paso 1. Crear los tipos

```typescript
type Plataforma = "PC" | "Switch" | "PlayStation" | "Xbox";

interface Videojuego {
  id: number;
  titulo: string;
  genero: string;
  precio: number;
  disponible: boolean;
  plataforma?: Plataforma;
}
```

### Paso 2. Crear el catálogo

```typescript
const catalogo: Videojuego[] = [
  {
    id: 1,
    titulo: "Hades",
    genero: "Roguelike",
    precio: 24.99,
    disponible: true,
    plataforma: "PC",
  },
  {
    id: 2,
    titulo: "Animal Crossing",
    genero: "Simulación",
    precio: 49.99,
    disponible: false,
    plataforma: "Switch",
  },
  {
    id: 3,
    titulo: "Ori and the Blind Forest",
    genero: "Plataformas",
    precio: 19.99,
    disponible: true,
    plataforma: "Xbox",
  },
];
```

### Paso 3. Mostrar un videojuego

```typescript
function mostrarVideojuego(videojuego: Videojuego): string {
  const estado = videojuego.disponible ? "Disponible" : "No disponible";
  const plataforma = videojuego.plataforma ?? "Sin especificar";

  return `${videojuego.titulo} | ${plataforma} | ${videojuego.precio} € | ${estado}`;
}

console.log(mostrarVideojuego(catalogo[0]));
```

El operador `??` permite utilizar un valor alternativo cuando el dato de la izquierda es `null` o `undefined`.

### Paso 4. Mostrar todo el catálogo

```typescript
for (const videojuego of catalogo) {
  console.log(mostrarVideojuego(videojuego));
}
```

### Paso 5. Filtrar los disponibles

```typescript
function obtenerDisponibles(lista: Videojuego[]): Videojuego[] {
  return lista.filter((videojuego) => videojuego.disponible);
}

const disponibles = obtenerDisponibles(catalogo);
console.log(disponibles);
```

### Paso 6. Buscar por identificador

```typescript
function buscarPorId(
  lista: Videojuego[],
  id: number
): Videojuego | undefined {
  return lista.find((videojuego) => videojuego.id === id);
}

const resultado = buscarPorId(catalogo, 2);

if (resultado) {
  console.log(mostrarVideojuego(resultado));
} else {
  console.log("No se ha encontrado el videojuego");
}
```

### Reto final

Añade al programa:

1. Dos videojuegos nuevos.
2. Una función `buscarPorTitulo()` que reciba un texto.
3. Una función `obtenerEconomicos()` que devuelva los juegos con un precio inferior al indicado.
4. Una función que calcule el precio medio del catálogo.

Utiliza tipos en todos los parámetros y valores de retorno.

---

## Errores frecuentes

### Utilizar mayúsculas en los tipos básicos

Debemos escribir:

```typescript
let nombre: string;
let edad: number;
let activo: boolean;
```

No utilizaremos `String`, `Number` o `Boolean` para estas declaraciones.

### Escribir `any` para evitar los errores

```typescript
let dato: any;
```

Esto silencia al compilador, pero también elimina su ayuda. Es preferible definir el tipo real.

### Olvidar que `find()` puede devolver `undefined`

```typescript
const resultado = catalogo.find((juego) => juego.id === 20);
console.log(resultado.titulo);
```

El acceso a `titulo` no es seguro porque quizá no exista ningún resultado.

```typescript
if (resultado) {
  console.log(resultado.titulo);
}
```

### Confundir una propiedad opcional con una propiedad obligatoria

```typescript
interface Usuario {
  nombre: string;
  apodo?: string;
}
```

`nombre` es obligatorio y `apodo` es opcional.

### Modificar el tipo de una variable inferida

```typescript
let puntuacion = 100;
puntuacion = "máxima";
```

Aunque no hayamos escrito `: number`, TypeScript lo ha inferido a partir del valor inicial.

---

## Resumen

```typescript
// Tipos básicos
const nombre: string = "Hades";
const precio: number = 24.99;
const disponible: boolean = true;

// Array tipado
const titulos: string[] = ["Hades", "Celeste"];

// Unión
let id: number | string = 1;

// Tipo personalizado
type Estado = "pendiente" | "completado";

// Interfaz
interface Videojuego {
  id: number;
  titulo: string;
  precio: number;
  plataforma?: string;
}

// Función tipada
function aplicarDescuento(precio: number): number {
  return precio * 0.9;
}

// Array de objetos
const catalogo: Videojuego[] = [];

// Resultado que puede no existir
const resultado: Videojuego | undefined = catalogo.find(
  (videojuego) => videojuego.id === 1
);
```

La finalidad de TypeScript no es escribir tipos por todas partes, sino describir correctamente los datos para detectar errores antes de ejecutar la aplicación.

En el siguiente bloque utilizaremos estos conocimientos para tipar componentes, propiedades y estados de React.

---

## Referencias

- [Documentación oficial de TypeScript](https://www.typescriptlang.org/docs/)
- [TypeScript Playground](https://www.typescriptlang.org/play)
- [Tipos habituales en TypeScript](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
