# JavaScript

## Sumario

- [Introducción](#introducción)
- [Tipos de datos](#tipos-de-datos)
  - [Null y undefined](#null-y-undefined)
  - [Manejando strings](#manejando-strings)
  - [Trabajando con fechas](#trabajando-con-fechas)
  - [Arrays](#arrays)
    - [Array de arrays](#array-de-arrays)
    - [Mapas](#mapas)
  - [Conjuntos (Set)](#conjuntos-set)
  - [Diferencia entre `==` y `===`](#diferencia-entre--y-)
- [El problema de los `for`](#el-problema-de-los-for)
- [Funciones](#funciones)
- [Acceso al DOM](#acceso-al-dom)
- [Clases y objetos](#clases-y-objetos)
  - [El ámbito de `this`](#el-ámbito-de-this)
  - [Clases con ES6](#clases-con-es6)
    - [Herencia](#herencia)
    - [`instanceof`](#instanceof)

---

## Introducción

JavaScript es un lenguaje que se usa especialmente desde el navegador web, que es quien lo interpreta. Su uso es principalmente para la parte de desarrollo del lado del cliente, aunque también se puede usar en el servidor mediante Node.js.

JavaScript y el DOM permiten que existan programadores que hagan un uso inapropiado para introducir scripts que ejecuten código con contenido malicioso sin el consentimiento del usuario y que pueda así comprometer su seguridad.

Los desarrolladores de los navegadores tienen en cuenta este riesgo utilizando dos restricciones. En primer lugar, los scripts se ejecutan en un *sandbox* en el que solo se pueden llevar a cabo acciones relacionadas con la web, no con tareas de programación de propósito general, como la creación de archivos. En segundo lugar, está limitada por la política del mismo origen: los scripts de un sitio web no tienen acceso a la información enviada a otro sitio web —de otro dominio—, como pudieran ser nombres de usuario, contraseñas o *cookies*. La mayoría de los fallos de seguridad de JavaScript están relacionados con violaciones de cualquiera de estas dos restricciones.

Para mostrar mensajes en la consola usamos:

```javascript
console.log("mensaje");
```

Si queremos que se muestre una ventana con el mensaje:

```javascript
alert("mensaje");
```

JavaScript es un lenguaje funcional. Si bien soporta objetos, su origen y uso principal es funcional.

Así, por ejemplo, cuando queremos declarar una variable y escribimos:

```javascript
for (var i = 0; i < 10; i++) {
  console.log(i);
}

console.log(i);
```

Observamos que la variable tiene un ámbito mayor que las llaves. De hecho, su ámbito es el de la función en la que está contenida. Si no se declara dentro de una función, entonces es global.

Hoy en día eso se puede solucionar mediante el uso de `let` en lugar de `var`, pero ya nos da una idea de que este lenguaje tiene una clara orientación funcional.

Continuando con el ámbito, podemos probar en la consola:

```javascript
var a = 10;
```

Luego miramos lo que contiene `a`:

```javascript
a;
```

Veremos que mostrará `10`, lo que coincide con:

```javascript
window.a;
```

Esto se debe a que todo cuelga de nuestra ventana de trabajo.

---

## Tipos de datos

JavaScript no está pensado inicialmente para tipado estático —como hacemos con Java, donde definimos el tipo de la variable antes de usarla—, sino que lo deduce de la asignación inicial.

Así:

```javascript
let num = "hola";
```

hará que `num` sea una variable de texto.

Si queremos estar seguros de que está tomando un número, podemos hacer uso de la función `Number()`:

```javascript
let num = Number(5);
```

También soporta `parseInt()`:

```javascript
parseInt("20€");
```

Devuelve:

```text
20
```

### Null y undefined

Pensemos un momento en las variables locales de un método en Java:

```java
String mivar;
System.out.println(mivar);
```

El IDE protestará porque la variable no ha sido inicializada.

Si en lugar de lo anterior ponemos:

```java
String mivar = null;
System.out.println(mivar);
```

veremos que muestra `null` en pantalla.

Así pues, hay una diferencia entre `null` y una variable sin inicializar en Java. De forma parecida, en JavaScript existen `undefined` y `null`.

Si escribimos:

```javascript
let mivar;
console.log(mivar);
```

mostrará:

```text
undefined
```

Si escribimos:

```javascript
let mivar2 = null;
console.log(mivar2);
```

mostrará:

```text
null
```

Para realizar operaciones con números tenemos `Math`:

```javascript
Math.round();
Math.random();
Math.abs();
Math.pow();
Math.trunc();
```

También, en lugar de usar `round`, podemos redondear un número directamente mediante:

```javascript
num.toFixed();
```

### Manejando strings

Tenemos bastantes elementos parecidos a Java:

```javascript
"Hola".length;     // Longitud de la cadena
"Hola".charAt(1); // Carácter de la posición 1
```

Pero también tenemos otras opciones:

```javascript
"Hola".charCodeAt(1); // Devuelve el código ASCII
```

También funciona:

```javascript
"Hola"[1];
```

Más operaciones con cadenas:

```javascript
"Animor".indexOf("i"); // Devuelve la posición del carácter

"Hola amigo".search("am"); // Busca la expresión regular y devuelve la posición

"En un lugar de la aancha".replace("aancha", "Mancha");
```

Podemos crear cadenas de texto para formatear variables:

```javascript
`Hola, el valor de a es: ${a}`;
```

#### Expresiones regulares

```javascript
var re = new RegExp(/d.n/);
re.test("Cadena");
```

Devuelve:

```text
true
```

#### Reemplazo en cadenas

```javascript
str.replace(/o/g, "i"); // Reemplaza todas las "o" por "i"
```

### Trabajando con fechas

Para crear una fecha:

```javascript
let fecha = new Date(); // Guarda la fecha actual: milisegundos desde 1-1-1970
```

Podemos consultar sus componentes:

```javascript
fecha.getDate();     // Día del mes
fecha.getDay();      // Día de la semana
fecha.getMonth();    // Mes: empieza en 0, no en 1
fecha.getFullYear(); // Año
```

### Arrays

Podemos crear arrays de diferentes maneras:

```javascript
var a = new Array();

var a = new Array(4, 3, 1);

var a = [];
var a = [6, 3, 2];
```

Para crear una cantidad específica de posiciones:

```javascript
var a = new Array(5);
```

Tenemos soporte para una pila:

```javascript
b = a.pop(); // Toma el último elemento y lo elimina del array
a.push(4);   // Añade el número al final y devuelve la nueva longitud
```

También para una cola:

```javascript
a.shift();
a.unshift(4);
```

#### Array de arrays

```javascript
var a = [
  [2, 3],
  [4, 5, 2],
];
```

Para buscar un elemento y obtener su índice:

```javascript
a.indexOf();
```

#### Mapas

Hay soporte para mapas:

```javascript
var m = new Map();
```

Podemos crearlos directamente con datos:

```javascript
var m = new Map([
  ["uno", 1],
  ["dos", 2],
]);
```

Obtenemos un valor por su clave:

```javascript
m.get("uno");
```

Podemos recorrer las claves y obtener los valores:

```javascript
for (const k of m.keys()) {
  console.log(k, m.get(k));
}
```

### Conjuntos (Set)

Tenemos el equivalente a un `HashSet`:

```javascript
const set = new Set();

set.add(2);
set.add(2); // No lo añade de nuevo
```

### Diferencia entre `==` y `===`

Observemos lo siguiente:

```javascript
var a = 50;
var b = "50";

a == b; // Devuelve true: realiza una transformación interna y compara el valor
```

Si queremos la igualdad estricta:

```javascript
a === b; // Devuelve false
```

---

## El problema de los `for`

```javascript
let array = [5, 4, 3];

for (const i in array) {
  console.log(i); // Muestra los índices: 0, 1 y 2
}
```

Para obtener los valores mediante `for...in` tendríamos que utilizar:

```javascript
for (const i in array) {
  console.log(array[i]);
}
```

Sin embargo, podemos utilizar `for...of`:

```javascript
for (const i of array) {
  console.log(i); // Muestra los valores del array
}
```

---

## Funciones

Para crear una función:

```javascript
function func(a, b) {
  console.log("hola...");
  console.log("a: " + a + " b: " + b);
  return 3;
}
```

También podemos utilizar funciones anónimas:

```javascript
let f = function () {
  console.log("holahola");
}; // La función queda almacenada en la variable
```

Se puede ejecutar mediante:

```javascript
f();
```

---

## Acceso al DOM

Para acceder a los objetos de nuestra página web podemos usar la sintaxis de CSS.

En este ejemplo obtenemos el nodo con la clase `decocat`:

```javascript
document.querySelector(".decocat"); // Devuelve únicamente el primer elemento
```

Si queremos obtener todos:

```javascript
document.querySelectorAll(".decocat");
```

Para obtener las imágenes que están dentro del elemento con identificador `pagina`:

```javascript
var elemento = document.querySelectorAll("#pagina img");
```

Una buena práctica es tener un objeto que defina los puntos del DOM que vamos a usar desde JavaScript:

```javascript
const DOM = {
  imgs: document.querySelectorAll("img"),
};
```

De esta forma, después podemos usar:

```javascript
DOM.imgs[0];
```

y estaremos accediendo a la primera imagen del documento.

### Modificando el HTML

```javascript
var element = document.querySelector("#pagina div");
```

Con `innerHTML` obtenemos el interior del `div`, sin las etiquetas `div`:

```javascript
element.innerHTML;
```

Con `outerHTML` obtenemos también las etiquetas `div`:

```javascript
element.outerHTML;
```

Podemos modificar el HTML del `div`:

```javascript
element.innerHTML = "<h1>jajaja</h1>";
```

También podemos crear elementos y agregarlos al final del documento:

```javascript
var div = document.createElement("div");
div.textContent = "Esto es un texto agregado al div";
div.id = "pieDePagina";
document.body.appendChild(div);
```

Es fácil observar que `innerHTML` es destructivo. Una forma de evitarlo es mediante `insertAdjacentHTML()`:

```javascript
var h1 = document.querySelector("h1");
h1.insertAdjacentHTML("afterbegin", "<span>after begin</span>");
```

Para borrar un elemento:

```javascript
document.querySelector("span").remove();
```

### Modificando los estilos

Podemos actuar directamente sobre el atributo `style` de HTML:

```javascript
var sw = document.querySelector("#switch");
sw.style.backgroundColor = "green";
```

Esto se recomienda únicamente para cambios muy pequeños o atómicos. Al modificar el atributo `style`, este tiene más prioridad que otras opciones. Es preferible hacer uso de clases CSS:

```javascript
sw.classList;                 // Lista de las clases del elemento
sw.classList.length;          // Número de clases cargadas
sw.classList.add("button");   // Añade una clase
sw.classList.contains("on"); // Comprueba si contiene la clase "on"
sw.classList.remove("on");   // Elimina la clase
sw.classList.toggle("on");   // La añade si no existe y la elimina si ya existe
```

---

## Clases y objetos

Podemos crear un objeto mediante el constructor `Object`:

```javascript
var obj = new Object();
```

También podemos crear un objeto asociando llaves:

```javascript
var obj = {};
var obj = { id: 4, name: "pepo" };
```

Estos objetos no se crean de forma fija. Se pueden agregar propiedades posteriormente.

Podemos añadir una función al objeto:

```javascript
obj.saludar = function () {
  console.log("eooo. Soy " + this.name);
};
```

Es posible reducir la declaración de las funciones en un objeto:

```javascript
var per = {
  nombre: "luis",
  apellidos: "martín",
  nombre_completo: function () {
    console.log(this.nombre + " " + this.apellidos);
  },
};
```

Equivale a:

```javascript
var per = {
  nombre: "luis",
  apellidos: "martín",
  nombre_completo() {
    console.log(this.nombre + " " + this.apellidos);
  },
};
```

Al igual que en Java, cuando igualamos dos variables se copia únicamente la referencia:

```javascript
var per2 = per;
```

Para copiar un objeto en lugar de la referencia —funciona únicamente de forma superficial—:

```javascript
var per2 = Object.assign({}, per);
```

### El ámbito de `this`

```javascript
function f() {
  return this;
}
```

En un script clásico ejecutado en el navegador puede devolver `window`.

Si lo hacemos dentro de un objeto:

```javascript
var o = {
  get() {
    return this;
  },
};
```

dentro del método, `this` hace referencia al objeto sobre el que se ha invocado.

### Clases con ES6

ES6 introduce una sintaxis específica para trabajar con clases. Esta sintaxis es azúcar sintáctico sobre el sistema de prototipos de JavaScript.

```javascript
class Monster {
  // ES6 únicamente permite un constructor por clase
  constructor() {
    this.life = 20;
    this.type = "generic monster";
    this.power = 5;
  }

  // Métodos
  talk() {
    return "Grrrr!";
  }

  stats() {
    return `Tipo: ${this.type}\nVida: ${this.life}`;
  }
}

const m = new Monster();
m.talk();
```

#### Herencia

Podemos crear una clase que herede de `Monster`:

```javascript
const Ghost = class extends Monster {
  constructor() {
    super();
    this.life = 40;
    this.type = "ghost";
  }

  talk() {
    return "uuuuuuhhhh!";
  }
};

const g = new Ghost();
```

#### `instanceof`

Podemos comprobar si un objeto es una instancia de una clase:

```javascript
g instanceof Ghost;   // true
g instanceof Monster; // true
```
