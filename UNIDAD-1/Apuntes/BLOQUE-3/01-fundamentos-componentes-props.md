# React con TypeScript I

## Fundamentos, TSX, componentes y props

## Sumario

- [Qué es React](#qué-es-react)
- [Crear el proyecto](#crear-el-proyecto)
- [Estructura básica](#estructura-básica)
- [Primer componente](#primer-componente)
- [TSX](#tsx)
- [Componentes](#componentes)
- [Props tipadas](#props-tipadas)
- [Composición de componentes](#composición-de-componentes)
- [Estilos CSS](#estilos-css)
- [Práctica guiada](#práctica-guiada)
- [Errores frecuentes](#errores-frecuentes)

---

## Qué es React

React es una biblioteca de JavaScript utilizada para construir interfaces de usuario.

Una aplicación React se divide en piezas reutilizables llamadas **componentes**. Una tarjeta, una cabecera, un buscador o un botón pueden convertirse en componentes independientes.

Cada componente puede combinar:

- Estructura visual mediante TSX.
- Datos recibidos de otros componentes.
- Comportamiento programado con TypeScript.
- Estilos CSS.

En este bloque utilizaremos **componentes funcionales**. No trabajaremos con componentes basados en clases.

---

## Crear el proyecto

Abre una terminal en la carpeta donde quieras guardar el proyecto y ejecuta:

```bash
npm create vite@latest gameshelf -- --template react-ts
```

Entra en la carpeta:

```bash
cd gameshelf
```

Instala las dependencias:

```bash
npm install
```

Inicia el servidor de desarrollo:

```bash
npm run dev
```

La terminal mostrará una dirección parecida a:

```text
http://localhost:5173/
```

Ábrela en el navegador.

> No cierres la terminal mientras utilizas la aplicación. Vite necesita mantener el servidor de desarrollo en ejecución.

### Recarga automática

Cuando guardamos un archivo, Vite actualiza la aplicación automáticamente. Esta característica se denomina **Hot Module Replacement**.

---

## Estructura básica

Los elementos principales del proyecto son:

```text
gameshelf/
├── public/
├── src/
│   ├── assets/
│   ├── App.css
│   ├── App.tsx
│   ├── index.css
│   └── main.tsx
├── index.html
├── package.json
└── tsconfig.json
```

### `index.html`

Contiene el punto de entrada HTML:

```html
<div id="root"></div>
```

React construirá la interfaz dentro de este elemento.

### `main.tsx`

Conecta React con el elemento `root` y renderiza el componente principal:

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import "./index.css";
import App from "./App";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

El símbolo `!` indica a TypeScript que confiamos en que el elemento `root` existe.

### `App.tsx`

Es el componente principal de la aplicación. Desde él construiremos o llamaremos al resto de componentes.

---

## Primer componente

Sustituye el contenido de `App.tsx` por:

```tsx
function App() {
  return (
    <main>
      <h1>GameShelf</h1>
      <p>Mi catálogo de videojuegos</p>
    </main>
  );
}

export default App;
```

Una función se convierte en componente cuando:

- Su nombre comienza con mayúscula.
- Devuelve interfaz mediante TSX.
- Se utiliza como una etiqueta: `<App />`.

---

## TSX

TSX permite escribir una estructura parecida a HTML dentro de TypeScript.

Los archivos que contengan TSX deben utilizar la extensión `.tsx`.

### Insertar expresiones

Dentro del TSX utilizamos llaves para insertar expresiones de TypeScript:

```tsx
function App() {
  const nombre = "GameShelf";
  const total = 3;

  return (
    <main>
      <h1>{nombre}</h1>
      <p>Hay {total} videojuegos.</p>
      <p>El doble es {total * 2}.</p>
    </main>
  );
}
```

Dentro de las llaves podemos utilizar:

- Variables.
- Operaciones.
- Llamadas a funciones.
- Operadores condicionales.
- Métodos de arrays.

No podemos escribir directamente una sentencia `if`, un `for` tradicional o varias instrucciones separadas dentro de las llaves.

### Un único elemento raíz

Un componente debe devolver un único elemento raíz.

Esto es incorrecto:

```tsx
return (
  <h1>GameShelf</h1>
  <p>Catálogo</p>
);
```

Podemos utilizar un contenedor:

```tsx
return (
  <main>
    <h1>GameShelf</h1>
    <p>Catálogo</p>
  </main>
);
```

También podemos usar un fragmento:

```tsx
return (
  <>
    <h1>GameShelf</h1>
    <p>Catálogo</p>
  </>
);
```

### Diferencias con HTML

En TSX utilizamos `className` en lugar de `class`:

```tsx
<section className="catalogo">Contenido</section>
```

Las etiquetas deben cerrarse:

```tsx
<img src="/imagen.png" alt="Portada" />
<input type="text" />
```

---

## Componentes

Crearemos una carpeta para organizarlos:

```text
src/
├── components/
│   └── Cabecera.tsx
├── App.tsx
└── main.tsx
```

Crea `src/components/Cabecera.tsx`:

```tsx
function Cabecera() {
  return (
    <header>
      <h1>GameShelf</h1>
      <p>Catálogo de videojuegos</p>
    </header>
  );
}

export default Cabecera;
```

Importa y utiliza el componente desde `App.tsx`:

```tsx
import Cabecera from "./components/Cabecera";

function App() {
  return (
    <main>
      <Cabecera />
    </main>
  );
}

export default App;
```

El componente puede reutilizarse tantas veces como sea necesario.

---

## Props tipadas

Las **props** permiten enviar información desde un componente padre a un componente hijo.

Crea `src/components/TarjetaJuego.tsx`:

```tsx
type TarjetaJuegoProps = {
  titulo: string;
  genero: string;
  precio: number;
};

function TarjetaJuego({ titulo, genero, precio }: TarjetaJuegoProps) {
  return (
    <article className="tarjeta">
      <h2>{titulo}</h2>
      <p>Género: {genero}</p>
      <p>Precio: {precio.toFixed(2)} €</p>
    </article>
  );
}

export default TarjetaJuego;
```

Desde `App.tsx` enviamos las props como atributos:

```tsx
import Cabecera from "./components/Cabecera";
import TarjetaJuego from "./components/TarjetaJuego";

function App() {
  return (
    <main>
      <Cabecera />

      <TarjetaJuego
        titulo="Hollow Knight"
        genero="Metroidvania"
        precio={14.99}
      />
    </main>
  );
}

export default App;
```

Los textos se envían entre comillas. Las expresiones, números y booleanos se envían entre llaves.

Si intentamos enviar un precio como texto, TypeScript mostrará un error:

```tsx
<TarjetaJuego
  titulo="Hollow Knight"
  genero="Metroidvania"
  precio="14.99"
/>
```

### Props opcionales

Podemos declarar una prop opcional con `?`:

```tsx
type TarjetaJuegoProps = {
  titulo: string;
  genero: string;
  precio: number;
  plataforma?: string;
};
```

Y asignar un valor por defecto al desestructurar:

```tsx
function TarjetaJuego({
  titulo,
  genero,
  precio,
  plataforma = "Sin especificar",
}: TarjetaJuegoProps) {
  return (
    <article>
      <h2>{titulo}</h2>
      <p>Género: {genero}</p>
      <p>Plataforma: {plataforma}</p>
      <p>Precio: {precio.toFixed(2)} €</p>
    </article>
  );
}
```

> Las props son de solo lectura. Un componente hijo no debe modificar directamente la información recibida.

---

## Composición de componentes

Un componente puede utilizar otros componentes:

```tsx
function App() {
  return (
    <main>
      <Cabecera />

      <section className="catalogo">
        <TarjetaJuego
          titulo="Hollow Knight"
          genero="Metroidvania"
          precio={14.99}
          plataforma="PC"
        />

        <TarjetaJuego
          titulo="Celeste"
          genero="Plataformas"
          precio={19.99}
        />
      </section>
    </main>
  );
}
```

La aplicación forma un árbol:

```text
App
├── Cabecera
└── TarjetaJuego
    └── Información visual
```

---

## Estilos CSS

Podemos importar un archivo CSS desde el componente:

```tsx
import "./App.css";
```

Ejemplo para `App.css`:

```css
:root {
  font-family: system-ui, sans-serif;
  color: #1f2937;
  background: #f3f4f6;
}

body {
  margin: 0;
}

main {
  width: min(100% - 2rem, 1100px);
  margin: 0 auto;
}

header {
  padding: 2rem 0;
  text-align: center;
}

.catalogo {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}

.tarjeta {
  padding: 1rem;
  border-radius: 0.75rem;
  background: white;
  box-shadow: 0 4px 12px rgb(0 0 0 / 10%);
}
```

---

## Práctica guiada

1. Crea el proyecto `gameshelf` con Vite.
2. Limpia el contenido inicial de `App.tsx` y `App.css`.
3. Crea el componente `Cabecera`.
4. Crea `TarjetaJuego` con las props:
   - `titulo`.
   - `genero`.
   - `precio`.
   - `plataforma`, opcional.
5. Muestra tres tarjetas diferentes desde `App`.
6. Aplica los estilos anteriores.
7. Añade una prop booleana `disponible` y muestra su valor en la tarjeta.

### Comprobación

- La aplicación se abre sin errores.
- Los componentes están en archivos separados.
- Las props tienen tipos concretos.
- El precio se envía como número.
- La plataforma puede omitirse.
- Las tres tarjetas utilizan el mismo componente.

---

## Errores frecuentes

### Escribir el nombre del componente en minúscula

```tsx
function tarjetaJuego() {
  return <article>...</article>;
}
```

Los componentes deben comenzar con mayúscula: `TarjetaJuego`.

### Olvidar exportar o importar

```tsx
export default TarjetaJuego;
```

```tsx
import TarjetaJuego from "./components/TarjetaJuego";
```

### Utilizar `class`

En TSX utilizamos:

```tsx
<article className="tarjeta">...</article>
```

### No cerrar una etiqueta

```tsx
<img src="/portada.png" alt="Portada" />
```

### Enviar un número como texto

```tsx
precio={19.99}
```

### Modificar una prop

Las props pertenecen al componente padre. Para manejar datos que cambian utilizaremos estado.

