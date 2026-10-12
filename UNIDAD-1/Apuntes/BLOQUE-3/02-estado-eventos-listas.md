# React con TypeScript II

## Estado, eventos, listas y renderizado condicional

## Sumario

- [Datos que cambian](#datos-que-cambian)
- [`useState`](#usestate)
- [Eventos](#eventos)
- [Inputs controlados](#inputs-controlados)
- [Interfaces y colecciones](#interfaces-y-colecciones)
- [Renderizar listas](#renderizar-listas)
- [Filtrar sin modificar el estado](#filtrar-sin-modificar-el-estado)
- [Renderizado condicional](#renderizado-condicional)
- [Actualizar arrays](#actualizar-arrays)
- [Práctica guiada](#práctica-guiada)
- [Errores frecuentes](#errores-frecuentes)

---

## Datos que cambian

Una variable normal puede cambiar, pero React no vuelve a representar la interfaz automáticamente por ese cambio.

Para guardar datos que afectan a la pantalla utilizamos el **estado**.

Cuando actualizamos un estado:

1. React guarda el nuevo valor.
2. React vuelve a ejecutar el componente.
3. React actualiza en el navegador únicamente lo necesario.

---

## `useState`

Importamos el hook:

```tsx
import { useState } from "react";
```

Ejemplo de contador:

```tsx
import { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <section>
      <p>Valor: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>
        Incrementar
      </button>
    </section>
  );
}

export default Contador;
```

La expresión:

```tsx
const [contador, setContador] = useState(0);
```

crea:

- `contador`: valor actual.
- `setContador`: función para actualizarlo.
- `0`: valor inicial.

TypeScript infiere que el estado es numérico.

También podríamos escribirlo explícitamente:

```tsx
const [contador, setContador] = useState<number>(0);
```

### Actualizaciones basadas en el valor anterior

Cuando el nuevo valor depende del anterior, es recomendable utilizar una función:

```tsx
setContador((valorAnterior) => valorAnterior + 1);
```

---

## Eventos

Los nombres de eventos se escriben en *camelCase*:

```tsx
<button onClick={mostrarMensaje}>Saludar</button>
```

La función se pasa sin paréntesis:

```tsx
function mostrarMensaje(): void {
  console.log("Hola");
}
```

Esto es incorrecto porque ejecuta la función durante el renderizado:

```tsx
<button onClick={mostrarMensaje()}>Saludar</button>
```

Si necesitamos enviar un argumento, utilizamos una función flecha:

```tsx
<button onClick={() => mostrarJuego("Celeste")}>Mostrar</button>
```

### Tipar un evento

Cuando escribimos el manejador directamente, TypeScript suele inferir el tipo:

```tsx
<input onChange={(evento) => console.log(evento.currentTarget.value)} />
```

Si extraemos la función, podemos tiparla:

```tsx
function manejarCambio(evento: React.ChangeEvent<HTMLInputElement>): void {
  console.log(evento.currentTarget.value);
}
```

---

## Inputs controlados

Un input controlado obtiene su valor desde el estado y lo actualiza mediante `onChange`:

```tsx
import { useState } from "react";

function Buscador() {
  const [texto, setTexto] = useState("");

  function manejarCambio(
    evento: React.ChangeEvent<HTMLInputElement>
  ): void {
    setTexto(evento.currentTarget.value);
  }

  return (
    <section>
      <label htmlFor="busqueda">Buscar</label>
      <input
        id="busqueda"
        type="search"
        value={texto}
        onChange={manejarCambio}
      />
      <p>Estás buscando: {texto}</p>
    </section>
  );
}

export default Buscador;
```

El atributo se escribe `htmlFor` porque `for` es una palabra reservada de JavaScript.

---

## Interfaces y colecciones

Crea `src/types/Videojuego.ts`:

```typescript
export interface Videojuego {
  id: number;
  titulo: string;
  genero: string;
  precio: number;
  disponible: boolean;
  plataforma?: string;
}
```

Crea `src/data/videojuegos.ts`:

```typescript
import type { Videojuego } from "../types/Videojuego";

export const videojuegosIniciales: Videojuego[] = [
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

`import type` indica que solo importamos una definición de tipo.

---

## Renderizar listas

En React utilizamos `map()` para convertir cada dato en un componente.

Actualiza las props de `TarjetaJuego` para recibir un objeto:

```tsx
import type { Videojuego } from "../types/Videojuego";

type TarjetaJuegoProps = {
  juego: Videojuego;
};

function TarjetaJuego({ juego }: TarjetaJuegoProps) {
  return (
    <article className="tarjeta">
      <h2>{juego.titulo}</h2>
      <p>Género: {juego.genero}</p>
      <p>Precio: {juego.precio.toFixed(2)} €</p>
      <p>{juego.disponible ? "Disponible" : "No disponible"}</p>
    </article>
  );
}

export default TarjetaJuego;
```

Desde `App.tsx`:

```tsx
import { videojuegosIniciales } from "./data/videojuegos";
import TarjetaJuego from "./components/TarjetaJuego";

function App() {
  return (
    <main>
      <section className="catalogo">
        {videojuegosIniciales.map((juego) => (
          <TarjetaJuego key={juego.id} juego={juego} />
        ))}
      </section>
    </main>
  );
}
```

### La prop especial `key`

Cada elemento creado mediante `map()` necesita una clave estable y única:

```tsx
key={juego.id}
```

React la utiliza para identificar los elementos cuando la lista cambia.

No utilizaremos el índice del array si contamos con un identificador real.

---

## Filtrar sin modificar el estado

Guarda la búsqueda en `App.tsx`:

```tsx
const [busqueda, setBusqueda] = useState("");
```

Calcula los resultados visibles:

```tsx
const juegosVisibles = videojuegosIniciales.filter((juego) =>
  juego.titulo.toLowerCase().includes(busqueda.toLowerCase())
);
```

No es necesario guardar `juegosVisibles` en otro estado. Es un valor que podemos calcular a partir de los datos originales y la búsqueda.

Renderizado completo:

```tsx
import { useState } from "react";
import TarjetaJuego from "./components/TarjetaJuego";
import { videojuegosIniciales } from "./data/videojuegos";

function App() {
  const [busqueda, setBusqueda] = useState("");

  const juegosVisibles = videojuegosIniciales.filter((juego) =>
    juego.titulo.toLowerCase().includes(busqueda.toLowerCase())
  );

  return (
    <main>
      <h1>GameShelf</h1>

      <label htmlFor="busqueda">Buscar videojuego</label>
      <input
        id="busqueda"
        type="search"
        value={busqueda}
        onChange={(evento) => setBusqueda(evento.currentTarget.value)}
      />

      <p>{juegosVisibles.length} resultados</p>

      <section className="catalogo">
        {juegosVisibles.map((juego) => (
          <TarjetaJuego key={juego.id} juego={juego} />
        ))}
      </section>
    </main>
  );
}

export default App;
```

---

## Renderizado condicional

Podemos decidir qué mostrar mediante un operador ternario:

```tsx
<p>{juego.disponible ? "Disponible" : "No disponible"}</p>
```

Para mostrar un elemento únicamente si se cumple una condición:

```tsx
{juego.disponible && <span className="estado">En stock</span>}
```

Para mostrar un mensaje cuando no existen resultados:

```tsx
{juegosVisibles.length === 0 ? (
  <p>No se han encontrado videojuegos.</p>
) : (
  <section className="catalogo">
    {juegosVisibles.map((juego) => (
      <TarjetaJuego key={juego.id} juego={juego} />
    ))}
  </section>
)}
```

---

## Actualizar arrays

Si la colección puede cambiar, la guardamos en un estado:

```tsx
const [videojuegos, setVideojuegos] =
  useState<Videojuego[]>(videojuegosIniciales);
```

No debemos modificar directamente el array:

```tsx
videojuegos.push(nuevoJuego);
```

Creamos un array nuevo:

```tsx
setVideojuegos((anteriores) => [...anteriores, nuevoJuego]);
```

Para eliminar:

```tsx
function eliminarJuego(id: number): void {
  setVideojuegos((anteriores) =>
    anteriores.filter((juego) => juego.id !== id)
  );
}
```

Para cambiar una propiedad:

```tsx
function alternarDisponibilidad(id: number): void {
  setVideojuegos((anteriores) =>
    anteriores.map((juego) =>
      juego.id === id
        ? { ...juego, disponible: !juego.disponible }
        : juego
    )
  );
}
```

---

## Práctica guiada

Continúa el proyecto anterior:

1. Crea la interfaz `Videojuego` en `src/types`.
2. Crea el array inicial en `src/data`.
3. Modifica `TarjetaJuego` para recibir un objeto `juego`.
4. Renderiza todas las tarjetas mediante `map()`.
5. Añade una `key` basada en `id`.
6. Crea un buscador controlado con `useState`.
7. Filtra por título sin modificar el array original.
8. Muestra el número de resultados.
9. Muestra un mensaje cuando no haya coincidencias.
10. Añade un botón para limpiar la búsqueda.

Botón de limpieza:

```tsx
<button type="button" onClick={() => setBusqueda("")}>
  Limpiar
</button>
```

### Ampliación

Añade un botón a cada tarjeta para cambiar su disponibilidad. Para ello tendrás que:

- Guardar el array en un estado.
- Enviar una función mediante props.
- Crear un array nuevo mediante `map()`.

---

## Errores frecuentes

### Llamar directamente al actualizador

Incorrecto:

```tsx
<button onClick={setContador(contador + 1)}>Sumar</button>
```

Correcto:

```tsx
<button onClick={() => setContador((anterior) => anterior + 1)}>
  Sumar
</button>
```

### Olvidar `key`

```tsx
{videojuegos.map((juego) => (
  <TarjetaJuego key={juego.id} juego={juego} />
))}
```

### Modificar un estado directamente

Debemos utilizar siempre su función actualizadora y crear arrays u objetos nuevos.

### Guardar estados innecesarios

Si un dato puede calcularse a partir de otros estados, normalmente no necesita un estado propio.

### Confundir props y estado

- Las props llegan desde el padre.
- El estado pertenece al componente que lo declara.
- Las props no deben modificarse directamente.

