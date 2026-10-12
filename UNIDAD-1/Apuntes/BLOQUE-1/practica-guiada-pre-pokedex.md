# Práctica guiada: crea una mini-Pokédex

## Introducción

En esta práctica construirás paso a paso una pequeña aplicación web capaz de consultar información de un Pokémon mediante [PokéAPI](https://pokeapi.co/) y mostrarla en una tarjeta.

La aplicación permitirá:

- Introducir el nombre o el número de un Pokémon.
- Consultar sus datos en PokéAPI.
- Mostrar su imagen, número, nombre, altura, peso y tipos.
- Informar mientras se cargan los datos.
- Mostrar un mensaje si el Pokémon no existe o se produce un error.

Esta práctica sirve como preparación para la tarea evaluable de la Pokédex. Aquí construiremos únicamente la búsqueda y visualización de **un Pokémon cada vez**.

---

## 1. ¿Qué vamos a practicar?

Durante la actividad utilizarás:

- HTML para crear la estructura de la aplicación.
- CSS para aplicar un diseño básico.
- JavaScript para programar su comportamiento.
- Selección de elementos del DOM con `querySelector()`.
- Eventos mediante `addEventListener()`.
- Funciones tradicionales y funciones flecha.
- Funciones asíncronas con `async` y `await`.
- Peticiones a una API mediante `fetch()`.
- Conversión de respuestas JSON a objetos JavaScript.
- Arrays y `map()`.
- Plantillas literales.
- Control de errores con `try`, `catch` y `throw`.

El recorrido que seguirá la información será:

```text
Texto introducido por el usuario
              ↓
Normalización de la búsqueda
              ↓
Petición a PokéAPI con fetch()
              ↓
Conversión de la respuesta a JSON
              ↓
Selección y transformación de los datos
              ↓
Creación de una tarjeta en el DOM
```

---

## 2. Requisitos previos

Necesitarás:

- Visual Studio Code.
- Un navegador web actualizado.
- Conexión a Internet.
- La extensión **Live Server** de Visual Studio Code, recomendada para abrir el proyecto.

No necesitas instalar ninguna librería adicional.

> **Importante:** no copies todo el documento de una vez. Realiza cada paso, guarda los archivos y comprueba el resultado antes de continuar.

---

## 3. Crea la estructura del proyecto

Dentro del repositorio de la asignatura, crea una carpeta llamada:

```text
mini-pokedex
```

Dentro de ella, crea esta estructura:

```text
mini-pokedex/
├── index.html
├── css/
│   └── style.css
└── js/
    └── app.js
```

Comprueba que:

- `index.html` está directamente dentro de `mini-pokedex`.
- `style.css` está dentro de la carpeta `css`.
- `app.js` está dentro de la carpeta `js`.

### Punto de control 1

Tu explorador de Visual Studio Code debe mostrar los tres archivos anteriores. Si has creado `css` o `js` dentro de otra carpeta por error, corrígelo antes de continuar.

---

## 4. Construye la estructura HTML

Abre `index.html` y escribe:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini-Pokédex</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <main class="contenedor">
    <h1>Mini-Pokédex</h1>

    <p class="introduccion">
      Introduce el nombre o el número de un Pokémon.
    </p>

    <form id="formulario-busqueda" class="buscador">
      <label for="busqueda">Nombre o número</label>

      <div class="buscador__controles">
        <input
          id="busqueda"
          name="busqueda"
          type="text"
          placeholder="Ejemplo: pikachu o 25"
          autocomplete="off"
        >

        <button type="submit">Buscar</button>
      </div>
    </form>

    <p id="mensaje" class="mensaje" aria-live="polite"></p>

    <section id="resultado" class="resultado"></section>
  </main>

  <script src="js/app.js"></script>
</body>
</html>
```

### ¿Qué hemos creado?

#### El formulario

```html
<form id="formulario-busqueda">
```

Agrupa el campo de texto y el botón. Utilizaremos su evento `submit`, que se produce tanto al pulsar el botón como al presionar Enter.

#### El campo de búsqueda

```html
<input id="busqueda" type="text">
```

Permitirá introducir:

- Un nombre: `pikachu`, `eevee` o `charizard`.
- Un identificador: `25`, `133` o `6`.

Su identificador nos permitirá seleccionarlo desde JavaScript.

#### El mensaje de estado

```html
<p id="mensaje" aria-live="polite"></p>
```

Se utilizará para mostrar mensajes como:

- `Cargando...`
- `Pokémon no encontrado.`
- `Introduce un nombre o número.`

El atributo `aria-live="polite"` permite que las tecnologías de asistencia anuncien los cambios realizados en este contenido.

#### La sección de resultados

```html
<section id="resultado"></section>
```

Inicialmente estará vacía. JavaScript insertará aquí la tarjeta del Pokémon.

#### La conexión con JavaScript

```html
<script src="js/app.js"></script>
```

Esta etiqueta carga el archivo JavaScript. Está colocada al final del `body` para que el contenido HTML ya exista cuando se ejecute el programa.

### Punto de control 2

Abre `index.html` con Live Server. Deberías ver:

- El título de la aplicación.
- Un texto introductorio.
- Un campo de búsqueda.
- Un botón.

Todavía no ocurrirá nada al realizar una búsqueda porque `app.js` está vacío.

---

## 5. Añade un diseño básico

Abre `css/style.css` y escribe:

```css
* {
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  margin: 0;
  padding: 2rem 1rem;
  font-family: Arial, sans-serif;
  color: #1f2937;
  background: #f3f4f6;
}

.contenedor {
  width: min(100%, 650px);
  margin: 0 auto;
}

h1 {
  margin-bottom: 0.5rem;
  text-align: center;
  color: #dc2626;
}

.introduccion {
  margin-bottom: 2rem;
  text-align: center;
}

.buscador {
  padding: 1.5rem;
  background: white;
  border-radius: 1rem;
  box-shadow: 0 8px 25px rgb(0 0 0 / 10%);
}

.buscador label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}

.buscador__controles {
  display: flex;
  gap: 0.75rem;
}

.buscador input {
  flex: 1;
  min-width: 0;
  padding: 0.75rem;
  border: 2px solid #d1d5db;
  border-radius: 0.5rem;
  font: inherit;
}

.buscador input:focus {
  border-color: #dc2626;
  outline: 3px solid rgb(220 38 38 / 20%);
}

.buscador button {
  padding: 0.75rem 1.25rem;
  border: 0;
  border-radius: 0.5rem;
  color: white;
  font: inherit;
  font-weight: bold;
  background: #dc2626;
  cursor: pointer;
}

.buscador button:hover {
  background: #b91c1c;
}

.mensaje {
  min-height: 1.5rem;
  margin: 1.5rem 0;
  text-align: center;
  font-weight: bold;
}

.resultado {
  display: flex;
  justify-content: center;
}

.pokemon {
  width: min(100%, 360px);
  padding: 1.5rem;
  text-align: center;
  background: white;
  border-radius: 1rem;
  box-shadow: 0 8px 25px rgb(0 0 0 / 10%);
}

.pokemon__imagen {
  width: 180px;
  height: 180px;
  image-rendering: pixelated;
}

.pokemon__nombre {
  margin: 0.5rem 0;
  text-transform: capitalize;
}

.pokemon__numero {
  color: #6b7280;
  font-weight: bold;
}

.pokemon__datos {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
  margin: 1.5rem 0;
}

.pokemon__datos p {
  margin: 0;
  padding: 0.75rem;
  background: #f3f4f6;
  border-radius: 0.5rem;
}

.pokemon__tipos {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.5rem;
}

.tipo {
  padding: 0.4rem 0.8rem;
  color: white;
  text-transform: capitalize;
  background: #4b5563;
  border-radius: 999px;
}

@media (max-width: 480px) {
  .buscador__controles {
    flex-direction: column;
  }
}
```

No es necesario memorizar estas propiedades. El objetivo principal de esta práctica es JavaScript. El CSS ya contiene las clases que utilizaremos al crear la tarjeta.

### Punto de control 3

Guarda el archivo y comprueba que el formulario aparece centrado y con fondo blanco. Si no cambia su apariencia, revisa esta línea de `index.html`:

```html
<link rel="stylesheet" href="css/style.css">
```

---

## 6. Selecciona los elementos del DOM

Abre `js/app.js` y escribe:

```javascript
const formulario = document.querySelector("#formulario-busqueda");
const inputBusqueda = document.querySelector("#busqueda");
const mensaje = document.querySelector("#mensaje");
const resultado = document.querySelector("#resultado");
```

### ¿Qué hace `querySelector()`?

`document.querySelector()` busca el primer elemento que coincide con un selector CSS.

Por ejemplo:

```javascript
document.querySelector("#busqueda");
```

busca el elemento cuyo atributo `id` es `busqueda`.

Guardamos cada elemento en una constante para poder utilizarlo posteriormente sin volver a buscarlo.

### Comprueba las selecciones

Añade temporalmente:

```javascript
console.log(formulario);
console.log(inputBusqueda);
console.log(mensaje);
console.log(resultado);
```

Abre las herramientas de desarrollo del navegador con `F12` y entra en la pestaña **Consola**.

Debes ver cuatro elementos HTML. Si alguno muestra `null`:

- Comprueba que el identificador existe en `index.html`.
- Revisa que no hayas escrito un identificador diferente.
- Comprueba que `app.js` está correctamente enlazado.

Cuando funcione, puedes eliminar los cuatro `console.log()`.

### Punto de control 4

Los cuatro elementos deben aparecer correctamente en la consola y no debe mostrarse ningún error en rojo.

---

## 7. Escucha el envío del formulario

Añade después de las constantes:

```javascript
formulario.addEventListener("submit", (evento) => {
  evento.preventDefault();

  console.log("Formulario enviado");
});
```

### ¿Qué hace `addEventListener()`?

Permite indicar qué código debe ejecutarse cuando ocurre un evento.

En este caso escuchamos el evento:

```javascript
"submit"
```

Este evento se produce cuando se envía el formulario:

- Al pulsar el botón Buscar.
- Al presionar Enter dentro del campo.

### ¿Qué es `evento`?

Es un objeto que contiene información sobre lo que acaba de suceder.

### ¿Por qué usamos `preventDefault()`?

El comportamiento predeterminado de un formulario es enviar sus datos y recargar la página. Nosotros queremos controlar el proceso con JavaScript, así que impedimos ese comportamiento:

```javascript
evento.preventDefault();
```

### Comprueba el evento

1. Guarda el archivo.
2. Escribe cualquier texto en el campo.
3. Pulsa Buscar.
4. Comprueba que aparece en la consola:

```text
Formulario enviado
```

La página no debe recargarse.

### Punto de control 5

Prueba también a pulsar Enter. El mensaje debe aparecer igualmente en la consola.

---

## 8. Recoge y normaliza la búsqueda

Sustituye el `console.log()` anterior por:

```javascript
const busqueda = inputBusqueda.value.trim().toLowerCase();

console.log(busqueda);
```

El manejador completo debe quedar así:

```javascript
formulario.addEventListener("submit", (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  console.log(busqueda);
});
```

### ¿Qué representa `inputBusqueda.value`?

Contiene el texto introducido por el usuario.

### ¿Qué hace `trim()`?

Elimina los espacios que aparecen al principio y al final:

```text
"   pikachu   " → "pikachu"
```

### ¿Qué hace `toLowerCase()`?

Convierte el texto a minúsculas:

```text
"PIKACHU" → "pikachu"
```

Esto permite normalizar búsquedas escritas de formas diferentes.

### Comprueba la normalización

Introduce:

```text
   PIKACHU   
```

La consola debe mostrar:

```text
pikachu
```

---

## 9. Valida que la búsqueda no esté vacía

Después de crear `busqueda`, añade:

```javascript
if (!busqueda) {
  mensaje.textContent = "Introduce un nombre o número.";
  resultado.innerHTML = "";
  return;
}
```

El código debe quedar así:

```javascript
formulario.addEventListener("submit", (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  if (!busqueda) {
    mensaje.textContent = "Introduce un nombre o número.";
    resultado.innerHTML = "";
    return;
  }

  console.log(busqueda);
});
```

### ¿Qué significa `!busqueda`?

Una cadena vacía se considera un valor falso en JavaScript. Después de aplicar `trim()`, una entrada compuesta únicamente por espacios se convierte en `""`.

Por tanto:

```javascript
if (!busqueda)
```

significa: «si no hay una búsqueda válida».

### ¿Por qué utilizamos `return`?

Finaliza la ejecución del manejador. Si el campo está vacío, no queremos continuar ni consultar la API.

### Comprueba la validación

Envía el formulario:

- Sin escribir nada.
- Escribiendo únicamente espacios.

En ambos casos debe aparecer:

```text
Introduce un nombre o número.
```

---

## 10. Crea la función que consulta PokéAPI

Antes del `addEventListener()`, crea esta función:

```javascript
const obtenerPokemon = async (busqueda) => {
  const url = `https://pokeapi.co/api/v2/pokemon/${busqueda}`;
  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error("Pokémon no encontrado.");
  }

  const datos = await respuesta.json();

  return datos;
};
```

### Explicación paso a paso

#### 1. Declaramos una función asíncrona

```javascript
const obtenerPokemon = async (busqueda) => {
```

`async` indica que la función realizará operaciones asíncronas y devolverá una promesa.

La función recibe `busqueda`, que puede ser un nombre o un identificador.

#### 2. Construimos la URL

```javascript
const url = `https://pokeapi.co/api/v2/pokemon/${busqueda}`;
```

Utilizamos una plantilla literal para insertar la búsqueda al final de la dirección.

Si `busqueda` contiene `pikachu`, la URL resultante será:

```text
https://pokeapi.co/api/v2/pokemon/pikachu
```

#### 3. Realizamos la petición

```javascript
const respuesta = await fetch(url);
```

`fetch()` realiza una petición HTTP y devuelve una promesa.

`await` espera a que la petición termine antes de continuar dentro de esta función.

#### 4. Comprobamos la respuesta

```javascript
if (!respuesta.ok) {
  throw new Error("Pokémon no encontrado.");
}
```

`respuesta.ok` vale `true` cuando el código HTTP está entre 200 y 299.

Si la API devuelve un error, por ejemplo `404`, creamos una excepción mediante `throw`.

> `fetch()` no entra automáticamente en un `catch` cuando recibe un error HTTP 404. Por eso debemos comprobar `respuesta.ok`.

#### 5. Convertimos la respuesta

```javascript
const datos = await respuesta.json();
```

La respuesta llega en formato JSON. El método `json()` la convierte en un objeto JavaScript.

#### 6. Devolvemos los datos

```javascript
return datos;
```

La primera versión devolverá todo el objeto recibido. En el siguiente paso conservaremos únicamente los datos necesarios.

---

## 11. Prueba la función desde el formulario

Para utilizar `await` dentro del manejador, conviértelo también en asíncrono:

```javascript
formulario.addEventListener("submit", async (evento) => {
```

Después de la validación, añade:

```javascript
const pokemon = await obtenerPokemon(busqueda);
console.log(pokemon);
```

El manejador completo debe quedar así:

```javascript
formulario.addEventListener("submit", async (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  if (!busqueda) {
    mensaje.textContent = "Introduce un nombre o número.";
    resultado.innerHTML = "";
    return;
  }

  const pokemon = await obtenerPokemon(busqueda);
  console.log(pokemon);
});
```

### Comprueba la respuesta

Busca:

```text
pikachu
```

En la consola aparecerá un objeto grande. Despliégalo y localiza:

- `id`
- `name`
- `height`
- `weight`
- `sprites`
- `types`

Prueba también con:

```text
25
```

Debe devolver el mismo Pokémon.

> No pruebes todavía un nombre inexistente. Aún no hemos añadido el bloque que captura el error.

### Punto de control 6

La consola debe mostrar el objeto completo de Pikachu y no debe haber errores.

---

## 12. Selecciona únicamente los datos necesarios

La API devuelve mucha más información de la que necesita nuestra tarjeta. Modifica la parte final de `obtenerPokemon()`.

Sustituye:

```javascript
return datos;
```

por:

```javascript
return {
  id: datos.id,
  nombre: datos.name,
  imagen: datos.sprites.front_default,
  altura: datos.height,
  peso: datos.weight,
  tipos: datos.types.map(({ type }) => type.name),
};
```

La función completa será:

```javascript
const obtenerPokemon = async (busqueda) => {
  const url = `https://pokeapi.co/api/v2/pokemon/${busqueda}`;
  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error("Pokémon no encontrado.");
  }

  const datos = await respuesta.json();

  return {
    id: datos.id,
    nombre: datos.name,
    imagen: datos.sprites.front_default,
    altura: datos.height,
    peso: datos.weight,
    tipos: datos.types.map(({ type }) => type.name),
  };
};
```

### ¿Qué estamos haciendo?

Creamos y devolvemos un objeto nuevo adaptado a nuestra aplicación.

De esta manera:

- Reducimos la cantidad de información con la que trabajamos.
- Utilizamos nombres en español dentro de nuestro programa.
- Evitamos que la interfaz dependa directamente de toda la estructura de la API.

### ¿Cómo obtenemos los tipos?

PokéAPI devuelve `types` como un array de objetos. Aplicamos:

```javascript
datos.types.map(({ type }) => type.name)
```

`map()` recorre el array y devuelve otro array con los nombres de los tipos.

Para Pikachu obtendremos:

```javascript
["electric"]
```

Para Charizard:

```javascript
["fire", "flying"]
```

### Comprueba el nuevo objeto

Vuelve a buscar `pikachu`. Ahora la consola debe mostrar un objeto mucho más pequeño:

```javascript
{
  id: 25,
  nombre: "pikachu",
  imagen: "...",
  altura: 4,
  peso: 60,
  tipos: ["electric"]
}
```

---

## 13. Crea la función que muestra la tarjeta

Después de `obtenerPokemon()` y antes del evento, crea:

```javascript
const mostrarPokemon = (pokemon) => {
  const tiposHTML = pokemon.tipos
    .map((tipo) => `<span class="tipo">${tipo}</span>`)
    .join("");

  resultado.innerHTML = `
    <article class="pokemon">
      <p class="pokemon__numero">N.º ${pokemon.id}</p>

      <img
        class="pokemon__imagen"
        src="${pokemon.imagen}"
        alt="Imagen de ${pokemon.nombre}"
      >

      <h2 class="pokemon__nombre">${pokemon.nombre}</h2>

      <div class="pokemon__datos">
        <p><strong>Altura</strong><br>${pokemon.altura / 10} m</p>
        <p><strong>Peso</strong><br>${pokemon.peso / 10} kg</p>
      </div>

      <div class="pokemon__tipos">
        ${tiposHTML}
      </div>
    </article>
  `;
};
```

### Explicación paso a paso

#### 1. Convertimos los tipos en HTML

```javascript
const tiposHTML = pokemon.tipos
  .map((tipo) => `<span class="tipo">${tipo}</span>`)
  .join("");
```

Si tenemos:

```javascript
["fire", "flying"]
```

`map()` lo transforma en:

```javascript
[
  '<span class="tipo">fire</span>',
  '<span class="tipo">flying</span>'
]
```

Después, `join("")` une los elementos sin añadir comas:

```html
<span class="tipo">fire</span><span class="tipo">flying</span>
```

#### 2. Insertamos la tarjeta

```javascript
resultado.innerHTML = `...`;
```

`innerHTML` sustituye el contenido de la sección `resultado` por el HTML generado.

#### 3. Insertamos valores con plantillas literales

```javascript
${pokemon.nombre}
```

Esta sintaxis permite insertar valores dentro de una cadena delimitada por acentos graves.

#### 4. Convertimos las unidades

PokéAPI devuelve:

- La altura en decímetros. Dividimos entre 10 para obtener metros.
- El peso en hectogramos. Dividimos entre 10 para obtener kilogramos.

---

## 14. Muestra el Pokémon encontrado

Dentro del manejador del formulario, sustituye:

```javascript
console.log(pokemon);
```

por:

```javascript
mostrarPokemon(pokemon);
```

El manejador será:

```javascript
formulario.addEventListener("submit", async (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  if (!busqueda) {
    mensaje.textContent = "Introduce un nombre o número.";
    resultado.innerHTML = "";
    return;
  }

  const pokemon = await obtenerPokemon(busqueda);
  mostrarPokemon(pokemon);
});
```

### Comprueba la tarjeta

Busca:

- `pikachu`
- `25`
- `charizard`
- `eevee`

La tarjeta debe actualizarse con cada búsqueda.

### Punto de control 7

Comprueba que Charizard muestra dos tipos y que no aparecen separados por comas.

---

## 15. Añade el estado de carga y la gestión de errores

Ahora controlaremos los tres estados principales de una petición:

```text
Cargando → Éxito
         ↘ Error
```

Sustituye el manejador del formulario por esta versión:

```javascript
formulario.addEventListener("submit", async (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  if (!busqueda) {
    mensaje.textContent = "Introduce un nombre o número.";
    resultado.innerHTML = "";
    return;
  }

  mensaje.textContent = "Cargando...";
  resultado.innerHTML = "";

  try {
    const pokemon = await obtenerPokemon(busqueda);

    mostrarPokemon(pokemon);
    mensaje.textContent = "";
  } catch (error) {
    mensaje.textContent = error.message;
  }
});
```

### ¿Qué ocurre antes de la petición?

```javascript
mensaje.textContent = "Cargando...";
resultado.innerHTML = "";
```

Informamos de que la consulta está en curso y eliminamos la tarjeta anterior.

### ¿Qué hace `try`?

```javascript
try {
  // Código que puede producir un error
}
```

Intenta ejecutar la consulta y mostrar el resultado.

### ¿Qué hace `catch`?

```javascript
catch (error) {
  mensaje.textContent = error.message;
}
```

Si se lanza una excepción, `catch` la recibe y muestra su mensaje.

El error creado anteriormente era:

```javascript
throw new Error("Pokémon no encontrado.");
```

Por tanto, `error.message` contendrá:

```text
Pokémon no encontrado.
```

### ¿Qué ocurre cuando todo funciona?

```javascript
mostrarPokemon(pokemon);
mensaje.textContent = "";
```

Mostramos la tarjeta y eliminamos el mensaje `Cargando...`.

### Comprueba los tres estados

1. Busca `pikachu`: debe aparecer la tarjeta.
2. Busca `pokemon-inventado`: debe aparecer el mensaje de error.
3. Envía el formulario vacío: debe aparecer el mensaje de validación.

---

## 16. Mejora el número de la Pokédex

Queremos mostrar:

```text
N.º 025
```

en lugar de:

```text
N.º 25
```

Crea esta función antes de `mostrarPokemon()`:

```javascript
const formatearId = (id) => {
  return String(id).padStart(3, "0");
};
```

### ¿Qué hace `String(id)`?

Convierte el número en una cadena, porque `padStart()` es un método de cadenas.

### ¿Qué hace `padStart()`?

```javascript
padStart(3, "0")
```

completa el principio de la cadena con ceros hasta alcanzar tres caracteres:

```text
1   → 001
25  → 025
133 → 133
```

En `mostrarPokemon()`, sustituye:

```javascript
<p class="pokemon__numero">N.º ${pokemon.id}</p>
```

por:

```javascript
<p class="pokemon__numero">N.º ${formatearId(pokemon.id)}</p>
```

---

## 17. Código final de `app.js`

Utiliza este apartado únicamente para comprobar tu trabajo. Tu archivo debe quedar así:

```javascript
const formulario = document.querySelector("#formulario-busqueda");
const inputBusqueda = document.querySelector("#busqueda");
const mensaje = document.querySelector("#mensaje");
const resultado = document.querySelector("#resultado");

const obtenerPokemon = async (busqueda) => {
  const url = `https://pokeapi.co/api/v2/pokemon/${busqueda}`;
  const respuesta = await fetch(url);

  if (!respuesta.ok) {
    throw new Error("Pokémon no encontrado.");
  }

  const datos = await respuesta.json();

  return {
    id: datos.id,
    nombre: datos.name,
    imagen: datos.sprites.front_default,
    altura: datos.height,
    peso: datos.weight,
    tipos: datos.types.map(({ type }) => type.name),
  };
};

const formatearId = (id) => {
  return String(id).padStart(3, "0");
};

const mostrarPokemon = (pokemon) => {
  const tiposHTML = pokemon.tipos
    .map((tipo) => `<span class="tipo">${tipo}</span>`)
    .join("");

  resultado.innerHTML = `
    <article class="pokemon">
      <p class="pokemon__numero">N.º ${formatearId(pokemon.id)}</p>

      <img
        class="pokemon__imagen"
        src="${pokemon.imagen}"
        alt="Imagen de ${pokemon.nombre}"
      >

      <h2 class="pokemon__nombre">${pokemon.nombre}</h2>

      <div class="pokemon__datos">
        <p><strong>Altura</strong><br>${pokemon.altura / 10} m</p>
        <p><strong>Peso</strong><br>${pokemon.peso / 10} kg</p>
      </div>

      <div class="pokemon__tipos">
        ${tiposHTML}
      </div>
    </article>
  `;
};

formulario.addEventListener("submit", async (evento) => {
  evento.preventDefault();

  const busqueda = inputBusqueda.value.trim().toLowerCase();

  if (!busqueda) {
    mensaje.textContent = "Introduce un nombre o número.";
    resultado.innerHTML = "";
    return;
  }

  mensaje.textContent = "Cargando...";
  resultado.innerHTML = "";

  try {
    const pokemon = await obtenerPokemon(busqueda);

    mostrarPokemon(pokemon);
    mensaje.textContent = "";
  } catch (error) {
    mensaje.textContent = error.message;
  }
});
```

---

## 18. Pruebas obligatorias

Prueba todos estos casos y comprueba el resultado:

| Entrada | Resultado esperado |
|---|---|
| `pikachu` | Muestra a Pikachu |
| `PIKACHU` | Muestra a Pikachu |
| `   pikachu   ` | Muestra a Pikachu |
| `25` | Muestra a Pikachu |
| `charizard` | Muestra dos tipos |
| `mr-mime` | Muestra a Mr. Mime |
| Campo vacío | Mensaje de validación |
| Solo espacios | Mensaje de validación |
| `pokemon-inventado` | Mensaje de Pokémon no encontrado |

Si alguno falla, revisa el código antes de continuar.

---

## 19. Preguntas de comprobación

Responde en tu cuaderno o en un archivo Markdown:

1. ¿Por qué escuchamos el evento `submit` del formulario?
2. ¿Qué ocurriría si eliminamos `evento.preventDefault()`?
3. ¿Para qué utilizamos `trim()` y `toLowerCase()`?
4. ¿Por qué `obtenerPokemon()` está declarada con `async`?
5. ¿Qué devuelve `fetch()`?
6. ¿Para qué se utiliza `await`?
7. ¿Por qué debemos comprobar `respuesta.ok`?
8. ¿Qué hace `respuesta.json()`?
9. ¿Por qué no devolvemos directamente todos los datos recibidos?
10. ¿Qué resultado produce `map()` al transformar los tipos?
11. ¿Por qué utilizamos `join("")` después de `map()`?
12. ¿Qué diferencia existe entre `try` y `catch`?
13. ¿Por qué hemos separado `obtenerPokemon()` y `mostrarPokemon()`?
14. ¿Qué función cumple `formatearId()`?
15. ¿Qué habría que modificar para mostrar varios Pokémon simultáneamente?

---

## 20. Pequeñas ampliaciones

Realiza estas mejoras una vez que la versión principal funcione.

### Ampliación 1. Limpia el campo después de buscar

Después de una búsqueda correcta, añade:

```javascript
inputBusqueda.value = "";
```

Decide en qué parte del `try` debe colocarse.

### Ampliación 2. Devuelve el foco al campo

Después de limpiar el campo:

```javascript
inputBusqueda.focus();
```

### Ampliación 3. Desactiva el botón durante la carga

Para hacerlo necesitarás seleccionar el botón y modificar su propiedad `disabled`.

```javascript
const botonBuscar = formulario.querySelector("button");
```

Antes de la petición:

```javascript
botonBuscar.disabled = true;
```

Para garantizar que vuelva a activarse tanto si la petición funciona como si falla, investiga el bloque:

```javascript
finally {
  botonBuscar.disabled = false;
}
```

### Ampliación 4. Usa otra imagen

Investiga dentro de `datos.sprites` otras imágenes disponibles y sustituye:

```javascript
datos.sprites.front_default
```

por otra propiedad que contenga una imagen oficial.

---

## 21. Errores frecuentes

### `formulario` contiene `null`

Revisa que el HTML contenga:

```html
id="formulario-busqueda"
```

y que el selector sea:

```javascript
document.querySelector("#formulario-busqueda")
```

### La página se recarga al buscar

Probablemente falta:

```javascript
evento.preventDefault();
```

### Aparece `await is only valid in async functions`

La función que contiene `await` debe estar declarada con `async`:

```javascript
async (evento) => {
```

### Aparece `Failed to fetch`

Comprueba:

- La conexión a Internet.
- Que la URL está correctamente escrita.
- Que PokéAPI está disponible.

### La tarjeta muestra `undefined`

Revisa que los nombres de las propiedades coincidan:

```javascript
nombre
imagen
altura
peso
tipos
```

### Los tipos aparecen separados por comas

Comprueba que has añadido:

```javascript
.join("")
```

después de `map()`.

### No se aplica el CSS

Revisa la ruta:

```html
<link rel="stylesheet" href="css/style.css">
```

---

## 22. Entrega y organización del repositorio

Guarda la actividad dentro del repositorio del módulo con una estructura clara. Por ejemplo:

```text
PGL/
└── unidad-1/
    └── mini-pokedex/
        ├── index.html
        ├── css/
        │   └── style.css
        └── js/
            └── app.js
```

Antes de finalizar:

- Comprueba que todos los archivos están guardados.
- Prueba todos los casos de la tabla.
- Revisa la consola y corrige los errores.
- Realiza un `commit` descriptivo.
- Sube los cambios al repositorio remoto.

Ejemplo de mensaje:

```bash
git add .
git commit -m "Añadir práctica guiada mini Pokédex"
git push
```

---

## 23. Lista de comprobación final

Marca cada elemento cuando esté terminado:

- [ ] He creado correctamente la estructura de carpetas.
- [ ] El HTML se abre mediante Live Server.
- [ ] El CSS se carga correctamente.
- [ ] Puedo enviar el formulario pulsando el botón.
- [ ] Puedo enviar el formulario pulsando Enter.
- [ ] La página no se recarga al buscar.
- [ ] La búsqueda elimina espacios y diferencia de mayúsculas.
- [ ] El campo vacío muestra un mensaje.
- [ ] Puedo consultar un Pokémon por nombre.
- [ ] Puedo consultar un Pokémon por número.
- [ ] La tarjeta muestra imagen, número, nombre, altura, peso y tipos.
- [ ] Los Pokémon con dos tipos muestran ambos.
- [ ] Una búsqueda inexistente muestra un error comprensible.
- [ ] No aparecen errores en rojo en la consola.
- [ ] He probado todos los casos indicados.
- [ ] He subido el resultado a mi repositorio.

---

## 24. Conclusión

Has construido una aplicación que conecta una interfaz web con una API externa. Aunque se trata de una mini-Pokédex, ya contiene el flujo fundamental de muchas aplicaciones reales:

1. Recoger información del usuario.
2. Validar y preparar los datos.
3. Consultar un servicio externo.
4. Transformar la respuesta.
5. Actualizar la interfaz.
6. Gestionar los estados de carga y error.

En la práctica evaluable ampliarás este mismo proceso para trabajar con una colección de Pokémon y nuevas funcionalidades.
