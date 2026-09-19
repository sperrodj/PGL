# Bloque 1. JavaScript moderno

## 1. Introducción

JavaScript es un lenguaje de programación utilizado para crear aplicaciones web, aplicaciones de servidor y aplicaciones móviles. En este módulo será la base sobre la que trabajaremos con **TypeScript**, **React** y **React Native**.

React Native no utiliza HTML ni ejecuta directamente una página web, pero sí emplea JavaScript para definir la lógica de la aplicación, gestionar datos, responder a eventos y construir componentes.

En este bloque repasaremos los elementos de JavaScript que necesitaremos durante el desarrollo de nuestra aplicación móvil. El objetivo no es memorizar toda la sintaxis del lenguaje, sino aprender a:

- Leer y comprender código JavaScript.
- Transformar y organizar datos.
- Dividir un problema en funciones.
- Trabajar con arrays y objetos.
- Consumir información procedente de una API.
- Gestionar operaciones asíncronas.
- Escribir código claro, mantenible y reutilizable.

Los ejemplos se irán acercando progresivamente al proyecto de una **Pokédex**, que más adelante construiremos con React y React Native utilizando la [PokéAPI](https://pokeapi.co/).

---

## 2. Preparación del entorno

Para ejecutar los ejemplos necesitaremos:

- **Visual Studio Code** como editor.
- **Node.js** como entorno de ejecución.
- Una terminal.

Podemos comprobar que Node.js está instalado mediante:

```bash
node --version
```

Para ejecutar un archivo llamado `app.js`:

```bash
node app.js
```

Nuestro primer archivo puede contener:

```javascript
console.log("Comenzamos nuestra Pokédex");
```

`console.log()` muestra información en la consola. Durante el aprendizaje y la depuración será una herramienta muy útil, aunque no debe sustituir a una interfaz adecuada en la aplicación final.

### Prueba guiada en consola: nuestro primer archivo

1. Crea una carpeta llamada `pokedex-js` y ábrela con Visual Studio Code.
2. Crea dentro el archivo `app.js`.
3. Escribe:

```javascript
console.log("=== POKÉDEX DE 2.º DAM ===");
console.log("Entorno preparado correctamente");
```

4. Abre la terminal integrada y ejecuta:

```bash
node app.js
```

El resultado esperado es:

```text
=== POKÉDEX DE 2.º DAM ===
Entorno preparado correctamente
```

5. Cambia el segundo mensaje, guarda el archivo y vuelve a ejecutar el comando. Comprueba que Node ejecuta siempre la última versión guardada.

---

## 3. Variables y constantes

Una variable permite almacenar un valor para utilizarlo posteriormente.

JavaScript moderno utiliza principalmente `const` y `let`:

```javascript
const nombre = "Pikachu";
let nivel = 5;

nivel = 6;

console.log(nombre);
console.log(nivel);
```

### 3.1. `const`

Se utiliza cuando la variable no va a recibir un nuevo valor.

```javascript
const region = "Kanto";
```

La siguiente reasignación produciría un error:

```javascript
const region = "Kanto";
region = "Johto"; // Error
```

### 3.2. `let`

Se utiliza cuando el valor debe cambiar durante la ejecución:

```javascript
let puntosDeVida = 100;
puntosDeVida = 75;
```

### 3.3. ¿Por qué evitamos `var`?

`var` pertenece a versiones anteriores de JavaScript y tiene un comportamiento de ámbito menos predecible. En código moderno utilizaremos:

- `const` por defecto.
- `let` únicamente cuando sea necesario reasignar el valor.

> Utilizar `const` no significa que el contenido de un objeto o array sea inmutable. Significa que la variable no puede apuntar a otro objeto o array diferente.

```javascript
const tipos = ["eléctrico"];
tipos.push("acero"); // Permitido

// tipos = ["agua"]; // No permitido
```

### Prueba guiada en consola: `const` y `let`

Copia y ejecuta:

```javascript
const nombre = "Pikachu";
let nivel = 5;

console.log(nombre, nivel);

nivel = nivel + 1;
console.log(`${nombre} ha subido al nivel ${nivel}`);
```

Resultado esperado:

```text
Pikachu 5
Pikachu ha subido al nivel 6
```

Ahora prueba a añadir `nombre = "Raichu";`. Observa el error, lee su mensaje y elimina esa línea. Después crea `let evolucion = "Pikachu";` y comprueba que esta sí puede reasignarse.

---

## 4. Tipos de datos

JavaScript es un lenguaje de **tipado dinámico**: una variable puede almacenar valores de distintos tipos y el tipo se determina durante la ejecución.

### 4.1. Tipos primitivos

```javascript
const nombre = "Charmander";  // string
const nivel = 5;              // number
const disponible = true;      // boolean
const evolucion = null;       // null
let entrenador;               // undefined
```

Podemos consultar el tipo mediante `typeof`:

```javascript
console.log(typeof nombre);      // string
console.log(typeof nivel);       // number
console.log(typeof disponible);  // boolean
```

Los tipos más habituales son:

| Tipo | Uso | Ejemplo |
|---|---|---|
| `string` | Texto | `"Bulbasaur"` |
| `number` | Números enteros y decimales | `25`, `3.14` |
| `boolean` | Valores lógicos | `true`, `false` |
| `undefined` | Valor todavía no asignado | `let resultado;` |
| `null` | Ausencia intencionada de valor | `const objeto = null;` |

### 4.2. Conversión de tipos

Los datos introducidos por el usuario o recibidos desde determinados orígenes pueden llegar como texto.

```javascript
const numeroComoTexto = "25";
const numero = Number(numeroComoTexto);

console.log(numero + 5); // 30
```

Otras conversiones habituales:

```javascript
String(25);       // "25"
Boolean(1);       // true
parseInt("42");  // 42
parseFloat("3.5"); // 3.5
```

Debemos comprobar las conversiones numéricas cuando los datos pueden ser incorrectos:

```javascript
const valor = Number("Pikachu");
console.log(Number.isNaN(valor)); // true
```

### Prueba guiada en consola: tipos y conversiones

```javascript
const idTexto = "25";
const idNumero = Number(idTexto);
const altura = 0.4;
const capturado = false;
let mote;

console.log(idTexto, typeof idTexto);
console.log(idNumero, typeof idNumero);
console.log(altura, typeof altura);
console.log(capturado, typeof capturado);
console.log(mote, typeof mote);
```

Comprueba que `"25" + 5` produce `"255"`, mientras que `Number("25") + 5` produce `30`. Prueba después a convertir `"veinticinco"` y utiliza `Number.isNaN()` para detectar el problema.

---

## 5. Operadores

### 5.1. Operadores aritméticos

```javascript
const suma = 10 + 5;
const resta = 10 - 5;
const multiplicacion = 10 * 5;
const division = 10 / 5;
const resto = 10 % 3;
const potencia = 2 ** 3;
```

### 5.2. Operadores de comparación

```javascript
5 > 3;    // true
5 < 3;    // false
5 >= 5;   // true
5 === 5;  // true
5 !== 3;  // true
```

En general utilizaremos `===` y `!==`, que comparan tanto el valor como el tipo:

```javascript
5 == "5";   // true: realiza conversión automática
5 === "5";  // false: son de tipos diferentes
```

### 5.3. Operadores lógicos

```javascript
const tieneEnergia = true;
const conoceAtaque = true;

console.log(tieneEnergia && conoceAtaque); // AND
console.log(tieneEnergia || conoceAtaque); // OR
console.log(!tieneEnergia);                // NOT
```

### 5.4. Operador ternario

Permite expresar una decisión sencilla:

```javascript
const nivel = 18;
const mensaje = nivel >= 16 ? "Puede evolucionar" : "Todavía no puede evolucionar";

console.log(mensaje);
```

No conviene encadenar varios ternarios complejos, porque dificultan la lectura.

### 5.5. Encadenamiento opcional y fusión nula

El operador `?.` permite acceder de forma segura a una propiedad que podría no existir:

```javascript
const pokemon = {
  nombre: "Eevee",
  entrenador: null,
};

console.log(pokemon.entrenador?.nombre); // undefined
```

El operador `??` proporciona un valor alternativo únicamente cuando el valor es `null` o `undefined`:

```javascript
const mote = null;
const nombreVisible = mote ?? "Sin mote";
```

### Prueba guiada en consola: operadores

```javascript
const nivel = 16;
const tienePiedra = false;
const puedeEvolucionar = nivel >= 16 && tienePiedra;

console.log("Nivel suficiente:", nivel >= 16);
console.log("Tiene piedra:", tienePiedra);
console.log("Puede evolucionar:", puedeEvolucionar);

const mensaje = puedeEvolucionar
  ? "La evolución está disponible"
  : "Falta algún requisito";

console.log(mensaje);
```

Cambia `tienePiedra` a `true` y predice el resultado antes de volver a ejecutar. Finalmente, compara `25 == "25"` y `25 === "25"` y explica por qué dan resultados diferentes.

---

## 6. Cadenas de texto y plantillas literales

Podemos crear texto con comillas simples, dobles o acentos graves:

```javascript
const nombre = "Squirtle";
const tipo = 'agua';
```

Las **plantillas literales** permiten insertar expresiones dentro de una cadena:

```javascript
const nombre = "Squirtle";
const nivel = 8;

const mensaje = `${nombre} se encuentra en el nivel ${nivel}`;
console.log(mensaje);
```

También admiten varias líneas:

```javascript
const ficha = `
Nombre: Pikachu
Tipo: Eléctrico
Número: 25
`;
```

Algunos métodos útiles de `string` son:

```javascript
const nombre = "  Pikachu  ";

nombre.trim();                 // "Pikachu"
nombre.toLowerCase();          // "  pikachu  "
nombre.toUpperCase();          // "  PIKACHU  "
nombre.includes("Pika");      // true
nombre.replace("Pika", "Ra"); // "  Rachu  "
```

### Prueba guiada en consola: preparar un nombre para la API

```javascript
const entradaUsuario = "  PIKACHU  ";
const nombreNormalizado = entradaUsuario.trim().toLowerCase();
const url = `https://pokeapi.co/api/v2/pokemon/${nombreNormalizado}`;

console.log(nombreNormalizado);
console.log(url);
```

Resultado esperado:

```text
pikachu
https://pokeapi.co/api/v2/pokemon/pikachu
```

Sustituye la entrada por `"  CHARMANDER "` y comprueba que no es necesario cambiar ninguna otra línea.

---

## 7. Estructuras de control

### 7.1. Condicionales

```javascript
const nivel = 20;

if (nivel < 10) {
  console.log("Nivel inicial");
} else if (nivel < 30) {
  console.log("Nivel intermedio");
} else {
  console.log("Nivel avanzado");
}
```

### 7.2. `switch`

Resulta útil cuando se compara un mismo valor con varias posibilidades:

```javascript
const tipo = "fuego";

switch (tipo) {
  case "fuego":
    console.log("Es débil frente al agua");
    break;
  case "agua":
    console.log("Es débil frente a planta");
    break;
  case "planta":
    console.log("Es débil frente al fuego");
    break;
  default:
    console.log("Tipo no contemplado");
}
```

### 7.3. Bucles

Un bucle `for` tradicional permite controlar el índice:

```javascript
const nombres = ["Bulbasaur", "Ivysaur", "Venusaur"];

for (let i = 0; i < nombres.length; i++) {
  console.log(nombres[i]);
}
```

Para recorrer directamente los valores de un iterable podemos utilizar `for...of`:

```javascript
for (const nombre of nombres) {
  console.log(nombre);
}
```

Aunque estos bucles siguen siendo útiles, en React utilizaremos frecuentemente métodos como `map()` y `filter()` para transformar arrays sin modificar los originales.

### Prueba guiada en consola: decisiones y repetición

```javascript
const niveles = [5, 18, 34];

for (const nivel of niveles) {
  if (nivel < 10) {
    console.log(`${nivel}: nivel inicial`);
  } else if (nivel < 30) {
    console.log(`${nivel}: nivel intermedio`);
  } else {
    console.log(`${nivel}: nivel avanzado`);
  }
}
```

Añade los niveles `9`, `10`, `29` y `30`. Antes de ejecutar, decide en qué grupo aparecerá cada uno. Con ello comprobaremos también los valores límite de las condiciones.

---

## 8. Funciones

Una función agrupa instrucciones que realizan una tarea concreta.

### 8.1. Declaración tradicional

```javascript
function saludar(nombre) {
  return `Hola, ${nombre}`;
}

console.log(saludar("Pikachu"));
```

### 8.2. Expresión de función

```javascript
const saludar = function (nombre) {
  return `Hola, ${nombre}`;
};
```

### 8.3. Funciones flecha

Las funciones flecha aparecen constantemente en React:

```javascript
const saludar = (nombre) => {
  return `Hola, ${nombre}`;
};
```

Si la función contiene una única expresión, podemos utilizar un retorno implícito:

```javascript
const duplicar = (numero) => numero * 2;
```

Si recibe un único parámetro, los paréntesis son opcionales, aunque mantenerlos puede mejorar la consistencia:

```javascript
const normalizarNombre = (nombre) => nombre.trim().toLowerCase();
```

### 8.4. Parámetros predeterminados

```javascript
const describirPokemon = (nombre, nivel = 1) => {
  return `${nombre} está en el nivel ${nivel}`;
};

console.log(describirPokemon("Pichu"));
```

### 8.5. Funciones puras

Una función pura:

- Devuelve siempre el mismo resultado para los mismos argumentos.
- No modifica datos externos.
- No produce efectos secundarios inesperados.

```javascript
const aumentarNivel = (nivelActual) => nivelActual + 1;
```

Esta forma de trabajar es especialmente útil en React, donde evitaremos modificar directamente el estado.

### Prueba guiada en consola: construir funciones reutilizables

```javascript
const capitalizar = (texto) => {
  const textoLimpio = texto.trim().toLowerCase();
  return textoLimpio.charAt(0).toUpperCase() + textoLimpio.slice(1);
};

const crearDescripcion = (nombre, tipo, nivel = 1) => {
  return `${capitalizar(nombre)} es de tipo ${tipo} y está en el nivel ${nivel}`;
};

console.log(crearDescripcion("  PIKACHU ", "eléctrico", 25));
console.log(crearDescripcion("eevee", "normal"));
```

Comprueba el valor predeterminado de `nivel` en la segunda llamada. Después crea una función `formatearId(id)` que transforme `25` en `"#025"` utilizando `String(id).padStart(3, "0")`.

---

## 9. Arrays

Un array almacena una colección ordenada de valores:

```javascript
const pokemon = ["Bulbasaur", "Charmander", "Squirtle"];

console.log(pokemon[0]);     // Bulbasaur
console.log(pokemon.length); // 3
```

### 9.1. Operaciones básicas

```javascript
const pokemon = ["Bulbasaur", "Charmander"];

pokemon.push("Squirtle"); // Añade al final
pokemon.pop();             // Elimina el último
pokemon.includes("Charmander"); // true
```

`push()` y `pop()` modifican el array original. En React será habitual crear un array nuevo:

```javascript
const pokemonActualizados = [...pokemon, "Pikachu"];
```

### 9.2. `forEach()`

Ejecuta una función por cada elemento, pero no devuelve un nuevo array:

```javascript
pokemon.forEach((nombre) => {
  console.log(nombre);
});
```

### 9.3. `map()`

Transforma cada elemento y devuelve un nuevo array:

```javascript
const nombres = ["bulbasaur", "charmander", "squirtle"];

const nombresEnMayusculas = nombres.map((nombre) => nombre.toUpperCase());

console.log(nombresEnMayusculas);
```

Ejemplo con objetos:

```javascript
const pokemon = [
  { id: 1, nombre: "Bulbasaur" },
  { id: 4, nombre: "Charmander" },
  { id: 7, nombre: "Squirtle" },
];

const nombres = pokemon.map((elemento) => elemento.nombre);
```

En React, `map()` se utilizará para convertir datos en componentes visuales.

### 9.4. `filter()`

Devuelve un nuevo array con los elementos que cumplen una condición:

```javascript
const pokemon = [
  { nombre: "Bulbasaur", tipo: "planta" },
  { nombre: "Charmander", tipo: "fuego" },
  { nombre: "Squirtle", tipo: "agua" },
  { nombre: "Vulpix", tipo: "fuego" },
];

const pokemonDeFuego = pokemon.filter(
  (elemento) => elemento.tipo === "fuego"
);
```

### 9.5. `find()`

Devuelve el primer elemento que cumple una condición:

```javascript
const resultado = pokemon.find(
  (elemento) => elemento.nombre === "Squirtle"
);
```

Si no encuentra ningún elemento, devuelve `undefined`.

### 9.6. `some()` y `every()`

```javascript
const hayPokemonDeFuego = pokemon.some(
  (elemento) => elemento.tipo === "fuego"
);

const todosTienenNombre = pokemon.every(
  (elemento) => elemento.nombre.length > 0
);
```

### 9.7. `reduce()`

Reduce todos los elementos a un único resultado:

```javascript
const niveles = [5, 10, 15];

const suma = niveles.reduce(
  (acumulador, nivel) => acumulador + nivel,
  0
);

console.log(suma); // 30
```

No debemos utilizar `reduce()` cuando `map()`, `filter()` o una solución más directa hagan el código más comprensible.

### Prueba guiada en consola: una cadena de transformaciones

```javascript
const pokemon = [
  { id: 1, nombre: "Bulbasaur", tipo: "planta", nivel: 12 },
  { id: 4, nombre: "Charmander", tipo: "fuego", nivel: 18 },
  { id: 7, nombre: "Squirtle", tipo: "agua", nivel: 9 },
  { id: 37, nombre: "Vulpix", tipo: "fuego", nivel: 22 },
];

const nombresDeFuego = pokemon
  .filter((elemento) => elemento.tipo === "fuego")
  .map((elemento) => elemento.nombre);

const pokemonBuscado = pokemon.find((elemento) => elemento.id === 7);
const hayNivelAlto = pokemon.some((elemento) => elemento.nivel >= 20);

console.log(nombresDeFuego);
console.log(pokemonBuscado);
console.log(hayNivelAlto);
```

Resultado esperado:

```text
[ 'Charmander', 'Vulpix' ]
{ id: 7, nombre: 'Squirtle', tipo: 'agua', nivel: 9 }
true
```

Modifica el filtro para obtener los Pokémon con nivel igual o superior a 15. Después utiliza `reduce()` para sumar todos los niveles.

---

## 10. Objetos

Un objeto agrupa propiedades relacionadas mediante pares de clave y valor:

```javascript
const pikachu = {
  id: 25,
  nombre: "Pikachu",
  tipo: "eléctrico",
  nivel: 12,
  capturado: true,
};
```

Podemos acceder a sus propiedades de dos maneras:

```javascript
console.log(pikachu.nombre);
console.log(pikachu["tipo"]);
```

La notación con corchetes es útil cuando el nombre de la propiedad está almacenado en una variable:

```javascript
const propiedad = "nivel";
console.log(pikachu[propiedad]);
```

### 10.1. Métodos

Un objeto también puede contener funciones:

```javascript
const pikachu = {
  nombre: "Pikachu",
  nivel: 12,
  presentarse() {
    return `${this.nombre}, nivel ${this.nivel}`;
  },
};
```

En React trabajaremos principalmente con objetos de datos y funciones externas, evitando depender innecesariamente de `this`.

### 10.2. Objetos anidados

Las respuestas de una API suelen contener estructuras anidadas:

```javascript
const pokemon = {
  nombre: "Pikachu",
  estadisticas: {
    puntosDeVida: 35,
    ataque: 55,
    defensa: 40,
  },
};

console.log(pokemon.estadisticas.ataque);
```

### Prueba guiada en consola: leer y actualizar objetos

```javascript
const pokemon = {
  id: 25,
  nombre: "Pikachu",
  tipos: ["eléctrico"],
  estadisticas: {
    vida: 35,
    ataque: 55,
  },
};

console.log(pokemon.nombre);
console.log(pokemon.tipos[0]);
console.log(pokemon.estadisticas.ataque);

const pokemonEntrenado = {
  ...pokemon,
  estadisticas: {
    ...pokemon.estadisticas,
    ataque: 60,
  },
};

console.log("Original:", pokemon.estadisticas.ataque);
console.log("Actualizado:", pokemonEntrenado.estadisticas.ataque);
```

Comprueba que el objeto original conserva el ataque `55`. Después añade la propiedad `capturado: true` únicamente al objeto nuevo.

---

## 11. Desestructuración

La desestructuración permite extraer valores de objetos y arrays de forma concisa.

### 11.1. Objetos

```javascript
const pokemon = {
  nombre: "Pikachu",
  nivel: 12,
  tipo: "eléctrico",
};

const { nombre, nivel } = pokemon;

console.log(nombre);
console.log(nivel);
```

Podemos asignar un nombre diferente o un valor predeterminado:

```javascript
const { nombre: nombrePokemon, mote = "Sin mote" } = pokemon;
```

También podemos desestructurar parámetros:

```javascript
const mostrarResumen = ({ nombre, tipo }) => {
  return `${nombre} es de tipo ${tipo}`;
};
```

Esta sintaxis se utilizará frecuentemente para recibir las `props` de un componente React.

### 11.2. Arrays

```javascript
const tipos = ["eléctrico", "acero"];
const [tipoPrincipal, tipoSecundario] = tipos;
```

### Prueba guiada en consola: desestructurar una respuesta

```javascript
const respuesta = {
  id: 6,
  name: "charizard",
  height: 17,
  weight: 905,
};

const { id, name: nombre, height: altura, weight: peso } = respuesta;

console.log(id);
console.log(nombre);
console.log(altura);
console.log(peso);
```

Observa cómo renombramos las propiedades inglesas al extraerlas. Añade una propiedad opcional mediante `const { imagen = "sin-imagen.png" } = respuesta;` y muestra su valor.

---

## 12. Operadores *spread* y *rest*

Ambos utilizan `...`, pero cumplen funciones diferentes.

### 12.1. *Spread*: expandir

Permite copiar o combinar arrays y objetos:

```javascript
const iniciales = ["Bulbasaur", "Charmander", "Squirtle"];
const ampliados = [...iniciales, "Pikachu"];
```

```javascript
const pokemon = {
  nombre: "Pikachu",
  nivel: 12,
};

const pokemonActualizado = {
  ...pokemon,
  nivel: 13,
};
```

El objeto original no se modifica. Esta idea de **inmutabilidad** será fundamental cuando gestionemos el estado en React.

> La copia con *spread* es superficial. Los objetos anidados continúan compartiendo referencias si no se copian también de forma explícita.

### 12.2. *Rest*: agrupar

Permite recoger varios valores:

```javascript
const sumar = (...numeros) => {
  return numeros.reduce((total, numero) => total + numero, 0);
};
```

También puede separar propiedades:

```javascript
const pokemon = {
  id: 25,
  nombre: "Pikachu",
  tipo: "eléctrico",
};

const { id, ...datosVisibles } = pokemon;
```

### Prueba guiada en consola: actualizar sin modificar

```javascript
const equipo = ["Pikachu", "Eevee"];
const equipoAmpliado = [...equipo, "Snorlax"];

const entrenador = {
  nombre: "Rojo",
  medallas: 7,
};

const entrenadorActualizado = {
  ...entrenador,
  medallas: 8,
};

console.log("Equipo original:", equipo);
console.log("Equipo nuevo:", equipoAmpliado);
console.log("Entrenador original:", entrenador);
console.log("Entrenador nuevo:", entrenadorActualizado);
```

Añade otro Pokémon al array nuevo sin utilizar `push()`. Comprueba que los originales no cambian.

---

## 13. Valores verdaderos y falsos

JavaScript convierte determinados valores en `true` o `false` cuando se evalúan en una condición.

Se consideran valores **falsy**, entre otros:

- `false`
- `0`
- `""`
- `null`
- `undefined`
- `NaN`

El resto de valores son generalmente **truthy**, incluidos `[]` y `{}`.

```javascript
const nombre = "";

if (!nombre) {
  console.log("Debes introducir un nombre");
}
```

Hay que tener cuidado cuando `0` es un valor válido:

```javascript
const nivel = 0;
const nivelVisible = nivel || 1;  // Devuelve 1
const nivelCorrecto = nivel ?? 1; // Devuelve 0
```

### Prueba guiada en consola: `||` frente a `??`

```javascript
const valores = [false, 0, "", null, undefined, [], {}];

for (const valor of valores) {
  console.log(valor, "→", Boolean(valor));
}

const cantidad = 0;
console.log("Con ||:", cantidad || 10);
console.log("Con ??:", cantidad ?? 10);
```

Explica por qué `[]` y `{}` se consideran verdaderos y por qué `??` es más adecuado cuando el número `0` es un valor válido.

---

## 14. Módulos: `export` e `import`

A medida que crece una aplicación, dividimos el código en archivos con responsabilidades concretas.

Archivo `utilidades.js`:

```javascript
export const normalizarNombre = (nombre) => {
  return nombre.trim().toLowerCase();
};

export const formatearId = (id) => {
  return `#${String(id).padStart(3, "0")}`;
};
```

Archivo `app.js`:

```javascript
import { normalizarNombre, formatearId } from "./utilidades.js";

console.log(normalizarNombre("  Pikachu "));
console.log(formatearId(25));
```

También existe la exportación por defecto:

```javascript
const obtenerColorTipo = (tipo) => {
  // ...
};

export default obtenerColorTipo;
```

```javascript
import obtenerColorTipo from "./obtenerColorTipo.js";
```

En un mismo módulo solo puede existir una exportación `default`, pero pueden existir varias exportaciones con nombre.

### Prueba guiada en consola: dividir el programa

1. Crea `utilidades.js`:

```javascript
export const capitalizar = (texto) => {
  const textoLimpio = texto.trim().toLowerCase();
  return textoLimpio.charAt(0).toUpperCase() + textoLimpio.slice(1);
};

export const formatearId = (id) => `#${String(id).padStart(3, "0")}`;
```

2. Crea `app.js`:

```javascript
import { capitalizar, formatearId } from "./utilidades.js";

console.log(`${formatearId(25)} - ${capitalizar("pikachu")}`);
```

3. Crea `package.json` en la misma carpeta:

```json
{
  "type": "module"
}
```

4. Ejecuta `node app.js`. El resultado esperado es `#025 - Pikachu`.

5. Añade a `utilidades.js` la función `normalizarNombre()` e impórtala desde `app.js`.

---

## 15. JSON

JSON es un formato de texto empleado para intercambiar datos. Se parece a la sintaxis de los objetos JavaScript, pero no son exactamente lo mismo.

```json
{
  "id": 25,
  "nombre": "Pikachu",
  "tipos": ["eléctrico"],
  "capturado": true
}
```

Características importantes:

- Las claves utilizan comillas dobles.
- No puede contener funciones.
- No admite comentarios.
- Se utiliza habitualmente en las APIs.

Conversión entre objetos y JSON:

```javascript
const pokemon = {
  id: 25,
  nombre: "Pikachu",
};

const textoJson = JSON.stringify(pokemon);
const objetoRecuperado = JSON.parse(textoJson);
```

### Prueba guiada en consola: guardar y recuperar JSON

```javascript
const equipo = {
  entrenador: "Misty",
  pokemon: ["Staryu", "Psyduck", "Togepi"],
};

const textoJson = JSON.stringify(equipo);
console.log(textoJson);
console.log(typeof textoJson);

const equipoRecuperado = JSON.parse(textoJson);
console.log(equipoRecuperado.pokemon[1]);
console.log(typeof equipoRecuperado);
```

Prueba también `JSON.stringify(equipo, null, 2)` para obtener una representación con sangría y más fácil de leer.

---

## 16. Programación asíncrona

JavaScript puede iniciar una operación que tardará en completarse sin bloquear toda la aplicación. Esto sucede, por ejemplo, cuando:

- Se consulta una API.
- Se lee o guarda información.
- Se solicita la ubicación del dispositivo.
- Se accede a la cámara.

### 16.1. Promesas

Una promesa representa el resultado futuro de una operación. Puede estar:

- Pendiente (`pending`).
- Resuelta (`fulfilled`).
- Rechazada (`rejected`).

Podemos consumirla mediante `.then()` y `.catch()`:

```javascript
fetch("https://pokeapi.co/api/v2/pokemon/pikachu")
  .then((respuesta) => respuesta.json())
  .then((datos) => console.log(datos))
  .catch((error) => console.error(error));
```

### 16.2. `async` y `await`

`async` y `await` permiten escribir el mismo proceso de forma más legible:

```javascript
const obtenerPokemon = async (nombre) => {
  const respuesta = await fetch(
    `https://pokeapi.co/api/v2/pokemon/${nombre}`
  );

  const datos = await respuesta.json();
  return datos;
};
```

Una función marcada con `async` siempre devuelve una promesa.

```javascript
const mostrarPokemon = async () => {
  const pokemon = await obtenerPokemon("pikachu");
  console.log(pokemon.name);
};

mostrarPokemon();
```

> `await` espera a que se resuelva la promesa dentro de la función asíncrona, pero no bloquea por completo la aplicación.

### Prueba guiada en consola: observar una promesa

```javascript
const esperar = (milisegundos) => {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Espera terminada"), milisegundos);
  });
};

const ejecutar = async () => {
  console.log("1. Inicio");
  const resultado = await esperar(2000);
  console.log("2.", resultado);
  console.log("3. Fin");
};

ejecutar();
console.log("4. El programa puede continuar");
```

Antes de ejecutarlo, predice el orden de los mensajes. Observa que `"4. El programa puede continuar"` aparece antes de que finalice la espera. Esto demuestra que la operación asíncrona no bloquea todo el programa.

---

## 17. Consumo de APIs con `fetch()`

Una API permite que una aplicación solicite o envíe información a otro sistema. `fetch()` realiza peticiones HTTP y devuelve una promesa.

### 17.1. Petición básica

```javascript
const obtenerPokemon = async (nombre) => {
  const url = `https://pokeapi.co/api/v2/pokemon/${nombre.toLowerCase()}`;
  const respuesta = await fetch(url);
  const datos = await respuesta.json();

  return datos;
};
```

### 17.2. Comprobar la respuesta

`fetch()` no lanza automáticamente un error por recibir un estado HTTP como `404`. Debemos comprobar `response.ok`:

```javascript
const obtenerPokemon = async (nombre) => {
  const url = `https://pokeapi.co/api/v2/pokemon/${nombre.toLowerCase()}`;
  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error(`No se pudo obtener el Pokémon: ${respuesta.status}`);
  }

  return respuesta.json();
};
```

### 17.3. Gestión de errores

```javascript
const mostrarPokemon = async (nombre) => {
  try {
    const pokemon = await obtenerPokemon(nombre);
    console.log(pokemon.name);
  } catch (error) {
    console.error("Se produjo un error:", error.message);
  }
};
```

En una aplicación visual no mostraremos el error únicamente en consola. Mantendremos estados que permitan informar al usuario.

### 17.4. Seleccionar solo los datos necesarios

Las APIs suelen devolver mucha más información de la que necesitamos. Podemos transformar la respuesta:

```javascript
const obtenerPokemon = async (nombre) => {
  const respuesta = await fetch(
    `https://pokeapi.co/api/v2/pokemon/${nombre.toLowerCase()}`
  );

  if (!respuesta.ok) {
    throw new Error("Pokémon no encontrado");
  }

  const datos = await respuesta.json();

  return {
    id: datos.id,
    nombre: datos.name,
    altura: datos.height,
    peso: datos.weight,
    imagen: datos.sprites.front_default,
    tipos: datos.types.map((elemento) => elemento.type.name),
  };
};
```

Este paso crea un objeto más pequeño y adaptado a las necesidades de nuestra aplicación.

### Prueba guiada en consola: primera consulta real

1. Asegúrate de tener Node.js actualizado y copia:

```javascript
const obtenerPokemon = async (nombre) => {
  const nombreNormalizado = nombre.trim().toLowerCase();
  const url = `https://pokeapi.co/api/v2/pokemon/${nombreNormalizado}`;

  console.log("Solicitando:", url);

  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error(`Error HTTP ${respuesta.status}`);
  }

  const datos = await respuesta.json();

  return {
    id: datos.id,
    nombre: datos.name,
    imagen: datos.sprites.front_default,
    tipos: datos.types.map(({ type }) => type.name),
  };
};

const iniciar = async () => {
  console.log("Cargando...");

  try {
    const pokemon = await obtenerPokemon("pikachu");
    console.log("Datos recibidos:", pokemon);
  } catch (error) {
    console.error("No se pudo completar la consulta:", error.message);
  }
};

iniciar();
```

2. Ejecuta el archivo y localiza en el resultado el identificador, el nombre, la imagen y los tipos.
3. Cambia `"pikachu"` por `"charizard"`.
4. Escribe deliberadamente un nombre inexistente para comprobar el `catch`.
5. Añade al objeto devuelto las propiedades `altura` y `peso`.

---

## 18. Ejemplo completo: ficha de un Pokémon

```javascript
const formatearId = (id) => `#${String(id).padStart(3, "0")}`;

const capitalizar = (texto) => {
  return texto.charAt(0).toUpperCase() + texto.slice(1);
};

const obtenerPokemon = async (nombre) => {
  const nombreNormalizado = nombre.trim().toLowerCase();
  const url = `https://pokeapi.co/api/v2/pokemon/${nombreNormalizado}`;
  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error(`No se encontró el Pokémon "${nombre}"`);
  }

  const datos = await respuesta.json();

  return {
    id: datos.id,
    nombre: capitalizar(datos.name),
    imagen: datos.sprites.front_default,
    tipos: datos.types.map(({ type }) => type.name),
    estadisticas: datos.stats.map(({ base_stat, stat }) => ({
      nombre: stat.name,
      valor: base_stat,
    })),
  };
};

const mostrarFicha = ({ id, nombre, tipos, estadisticas }) => {
  console.log(`${formatearId(id)} - ${nombre}`);
  console.log(`Tipos: ${tipos.join(", ")}`);

  estadisticas.forEach(({ nombre: nombreEstadistica, valor }) => {
    console.log(`${nombreEstadistica}: ${valor}`);
  });
};

const iniciar = async () => {
  try {
    console.log("Cargando Pokémon...");
    const pokemon = await obtenerPokemon("pikachu");
    mostrarFicha(pokemon);
  } catch (error) {
    console.error(error.message);
  }
};

iniciar();
```

Este ejemplo reúne varios conceptos del bloque:

- Funciones flecha.
- Plantillas literales.
- Normalización de texto.
- Funciones asíncronas.
- `fetch()`.
- Gestión de errores.
- Transformación de arrays con `map()`.
- Desestructuración de objetos.
- Recorrido mediante `forEach()`.
- Separación del código por responsabilidades.

---

## 19. Buenas prácticas

### 19.1. Utilizar nombres descriptivos

```javascript
// Evitar
const p = [];
const f = (x) => x.name;

// Preferible
const listaPokemon = [];
const obtenerNombre = (pokemon) => pokemon.name;
```

### 19.2. Mantener funciones pequeñas

Cada función debería tener una responsabilidad clara. Por ejemplo:

- `obtenerPokemon()` solicita y transforma los datos.
- `formatearId()` da formato al identificador.
- `mostrarFicha()` presenta la información.

### 19.3. Evitar modificar datos innecesariamente

```javascript
// Modifica el array original
pokemon.push(nuevoPokemon);

// Crea un array nuevo
const listaActualizada = [...pokemon, nuevoPokemon];
```

### 19.4. Gestionar errores

Una petición puede fallar por problemas de conexión, datos incorrectos o errores del servidor. No debemos asumir que siempre tendrá éxito.

### 19.5. Evitar valores repetidos sin significado

```javascript
const LIMITE_POKEMON = 20;
const url = `https://pokeapi.co/api/v2/pokemon?limit=${LIMITE_POKEMON}`;
```

### 19.6. Separar lógica y presentación

La obtención y transformación de datos no debería mezclarse con la forma de mostrarlos. Esta separación facilitará después el paso a React.

---

## 20. Errores frecuentes

### Confundir `=` con `===`

```javascript
if (tipo = "fuego") { // Asigna un valor
  // ...
}

if (tipo === "fuego") { // Compara
  // ...
}
```

### Olvidar devolver un valor

```javascript
const duplicar = (numero) => {
  numero * 2; // No devuelve el resultado
};
```

La versión correcta:

```javascript
const duplicar = (numero) => numero * 2;
```

### Confundir `map()` con `forEach()`

- `map()` devuelve un array nuevo.
- `forEach()` recorre los elementos, pero devuelve `undefined`.

### No comprobar `response.ok`

Una respuesta `404` también resuelve la promesa de `fetch()`. Por eso debemos verificar si la respuesta HTTP es correcta.

### Olvidar `await`

```javascript
const datos = respuesta.json(); // Promesa pendiente
const datosCorrectos = await respuesta.json();
```

### Modificar directamente objetos o arrays compartidos

En React puede provocar que la interfaz no se actualice correctamente. Trabajaremos creando nuevas versiones mediante *spread*, `map()` o `filter()`.

---

## 21. Resumen

En este bloque hemos trabajado los fundamentos de JavaScript moderno necesarios para continuar con el módulo:

- `const` y `let` para declarar variables.
- Tipos de datos, operadores y estructuras de control.
- Funciones tradicionales y funciones flecha.
- Arrays, objetos y desestructuración.
- `map()`, `filter()`, `find()`, `some()` y `reduce()`.
- Copia y actualización de datos con *spread*.
- Organización del código mediante módulos.
- JSON como formato de intercambio.
- Promesas, `async` y `await`.
- Consumo de APIs con `fetch()`.
- Gestión de errores con `try...catch`.
- Inmutabilidad, reutilización y separación de responsabilidades.

Estos conocimientos serán la base para incorporar TypeScript y trabajar posteriormente con componentes y estados en React.

---
