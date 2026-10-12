# React con TypeScript III

## Efectos y consumo de una API

## Sumario

- [Qué es un efecto](#qué-es-un-efecto)
- [`useEffect`](#useeffect)
- [El array de dependencias](#el-array-de-dependencias)
- [Consultar una API](#consultar-una-api)
- [Tipar la respuesta](#tipar-la-respuesta)
- [Carga y errores](#carga-y-errores)
- [Separar componentes](#separar-componentes)
- [Consulta dependiente de un dato](#consulta-dependiente-de-un-dato)
- [Práctica guiada](#práctica-guiada)
- [Errores frecuentes](#errores-frecuentes)

---

## Qué es un efecto

Durante el renderizado, un componente calcula qué debe mostrar.

Algunas acciones necesitan comunicarse con sistemas externos a React:

- Consultar una API.
- Modificar el título del documento.
- Crear un temporizador.
- Suscribirse a un evento externo.

Estas acciones se denominan **efectos**.

No utilizaremos `useEffect` para cualquier cálculo. Filtrar un array o construir un texto puede hacerse directamente durante el renderizado.

---

## `useEffect`

Importamos el hook:

```tsx
import { useEffect } from "react";
```

Ejemplo:

```tsx
import { useEffect } from "react";

function App() {
  useEffect(() => {
    console.log("El componente se ha mostrado");
  }, []);

  return <h1>GameShelf</h1>;
}
```

El primer argumento es la función que ejecutará el efecto.

El segundo argumento es el array de dependencias.

---

## El array de dependencias

### Array vacío

```tsx
useEffect(() => {
  console.log("Se ejecuta al montar el componente");
}, []);
```

Se utiliza para iniciar una acción cuando el componente aparece.

### Con una dependencia

```tsx
useEffect(() => {
  document.title = `Búsqueda: ${busqueda}`;
}, [busqueda]);
```

Se ejecuta al montar el componente y cada vez que cambia `busqueda`.

### Sin array

```tsx
useEffect(() => {
  console.log("Se ejecuta después de cada renderizado");
});
```

Este comportamiento puede producir ejecuciones innecesarias y, si actualizamos un estado dentro del efecto, bucles infinitos.

### Limpieza

Algunos efectos deben deshacerse:

```tsx
useEffect(() => {
  const id = window.setInterval(() => {
    console.log("tic");
  }, 1000);

  return () => {
    window.clearInterval(id);
  };
}, []);
```

La función devuelta se ejecuta cuando React necesita limpiar el efecto.

---

## Consultar una API

Para el ejemplo utilizaremos la API pública de Studio Ghibli:

```text
https://ghibliapi.vercel.app/films
```

La consulta se realizará mediante `fetch()`.

`fetch()` devuelve una promesa. Por ello utilizaremos una función asíncrona.

```typescript
async function obtenerPeliculas() {
  const respuesta = await fetch("https://ghibliapi.vercel.app/films");
  const datos = await respuesta.json();
  console.log(datos);
}
```

También debemos comprobar el estado HTTP:

```typescript
if (!respuesta.ok) {
  throw new Error(`Error HTTP: ${respuesta.status}`);
}
```

---

## Tipar la respuesta

Crea `src/types/Pelicula.ts`:

```typescript
export interface Pelicula {
  id: string;
  title: string;
  original_title: string;
  description: string;
  director: string;
  release_date: string;
  image: string;
}
```

Solo necesitamos describir los campos que utilizará nuestra aplicación.

> TypeScript comprueba cómo utilizamos los datos en el código, pero una interfaz no valida por sí sola que el servidor haya enviado realmente esa estructura.

---

## Carga y errores

Una consulta tarda un tiempo y puede fallar. Necesitamos representar tres estados:

- Cargando.
- Datos recibidos.
- Error.

Ejemplo completo:

```tsx
import { useEffect, useState } from "react";
import type { Pelicula } from "./types/Pelicula";

function App() {
  const [peliculas, setPeliculas] = useState<Pelicula[]>([]);
  const [cargando, setCargando] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    async function cargarPeliculas(): Promise<void> {
      try {
        setCargando(true);
        setError(null);

        const respuesta = await fetch(
          "https://ghibliapi.vercel.app/films"
        );

        if (!respuesta.ok) {
          throw new Error(`Error HTTP: ${respuesta.status}`);
        }

        const datos: Pelicula[] = await respuesta.json();
        setPeliculas(datos);
      } catch (errorCapturado) {
        const mensaje =
          errorCapturado instanceof Error
            ? errorCapturado.message
            : "Se ha producido un error desconocido";

        setError(mensaje);
      } finally {
        setCargando(false);
      }
    }

    cargarPeliculas();
  }, []);

  if (cargando) {
    return <p>Cargando películas...</p>;
  }

  if (error) {
    return <p role="alert">Error: {error}</p>;
  }

  return (
    <main>
      <h1>Películas de Studio Ghibli</h1>

      <section className="catalogo">
        {peliculas.map((pelicula) => (
          <article className="tarjeta" key={pelicula.id}>
            <img src={pelicula.image} alt={`Cartel de ${pelicula.title}`} />
            <h2>{pelicula.title}</h2>
            <p>{pelicula.original_title}</p>
            <p>Dirección: {pelicula.director}</p>
            <p>Año: {pelicula.release_date}</p>
          </article>
        ))}
      </section>
    </main>
  );
}

export default App;
```

No hacemos que la función pasada directamente a `useEffect` sea `async`. Creamos una función asíncrona dentro del efecto y después la ejecutamos.

---

## Separar componentes

Crea `src/components/TarjetaPelicula.tsx`:

```tsx
import type { Pelicula } from "../types/Pelicula";

type TarjetaPeliculaProps = {
  pelicula: Pelicula;
};

function TarjetaPelicula({ pelicula }: TarjetaPeliculaProps) {
  return (
    <article className="tarjeta">
      <img src={pelicula.image} alt={`Cartel de ${pelicula.title}`} />
      <h2>{pelicula.title}</h2>
      <p>{pelicula.original_title}</p>
      <p>Dirección: {pelicula.director}</p>
      <p>Año: {pelicula.release_date}</p>
    </article>
  );
}

export default TarjetaPelicula;
```

En `App.tsx`:

```tsx
<section className="catalogo">
  {peliculas.map((pelicula) => (
    <TarjetaPelicula key={pelicula.id} pelicula={pelicula} />
  ))}
</section>
```

---

## Consulta dependiente de un dato

Si un efecto depende de una variable, la incluimos en el array:

```tsx
useEffect(() => {
  async function cargarPelicula(): Promise<void> {
    const respuesta = await fetch(
      `https://ghibliapi.vercel.app/films/${idSeleccionado}`
    );

    const datos: Pelicula = await respuesta.json();
    setPelicula(datos);
  }

  cargarPelicula();
}, [idSeleccionado]);
```

El efecto se repetirá cuando cambie `idSeleccionado`.

No incluimos dependencias arbitrarias: incluimos los valores externos utilizados por el efecto y cuya modificación debe volver a ejecutar la acción.

---

## Práctica guiada

1. Crea la interfaz `Pelicula`.
2. Crea tres estados:
   - Array de películas.
   - Indicador de carga.
   - Mensaje de error.
3. Realiza la consulta desde un `useEffect` con array vacío.
4. Comprueba `respuesta.ok`.
5. Guarda los datos recibidos.
6. Muestra un mensaje durante la carga.
7. Muestra el error si la consulta falla.
8. Extrae la tarjeta a un componente con props tipadas.
9. Renderiza la colección con `map()` y una `key`.
10. Añade una búsqueda por título utilizando `useState` y `filter()`.

### CSS básico para las imágenes

```css
.tarjeta img {
  display: block;
  width: 100%;
  aspect-ratio: 2 / 3;
  object-fit: cover;
  border-radius: 0.5rem;
}
```

### Ampliaciones

- Botón para reintentar la consulta si falla.
- Filtro por director.
- Ordenación por año.
- Mostrar u ocultar la descripción.
- Contador de resultados.

---

## Errores frecuentes

### Convertir directamente el callback en asíncrono

Evita:

```tsx
useEffect(async () => {
  // ...
}, []);
```

Crea la función asíncrona dentro del efecto.

### Olvidar el array de dependencias

Sin `[]`, el efecto se ejecutará después de cada renderizado. Si dentro actualiza un estado, puede repetir la consulta continuamente.

### No comprobar `respuesta.ok`

`fetch()` no rechaza automáticamente todas las respuestas HTTP con error. Debemos comprobarlas.

### No representar la carga

Una colección vacía durante la petición no significa necesariamente que la API no tenga resultados.

### Usar `any` para la respuesta

Define una interfaz con los campos que realmente utilizará la aplicación.

### Guardar los resultados filtrados en otro estado

La búsqueda visible puede calcularse a partir de `peliculas` y `busqueda` durante el renderizado.

---

## Resumen del bloque

```tsx
const [datos, setDatos] = useState<Pelicula[]>([]);
const [cargando, setCargando] = useState(true);
const [error, setError] = useState<string | null>(null);

useEffect(() => {
  async function cargar(): Promise<void> {
    try {
      const respuesta = await fetch(URL);

      if (!respuesta.ok) {
        throw new Error(`Error HTTP: ${respuesta.status}`);
      }

      const resultado: Pelicula[] = await respuesta.json();
      setDatos(resultado);
    } catch (errorCapturado) {
      setError(
        errorCapturado instanceof Error
          ? errorCapturado.message
          : "Error desconocido"
      );
    } finally {
      setCargando(false);
    }
  }

  cargar();
}, []);
```

Con estos conocimientos ya podemos construir una aplicación React sencilla con TypeScript. En el siguiente bloque trasladaremos los conceptos de componentes, props, estado y efectos a React Native mediante Expo.

---

## Referencias

- [Documentación oficial de React](https://react.dev/learn)
- [React con TypeScript](https://react.dev/learn/typescript)
- [Documentación oficial de Vite](https://vite.dev/guide/)
- [Documentación oficial de TypeScript](https://www.typescriptlang.org/docs/)
