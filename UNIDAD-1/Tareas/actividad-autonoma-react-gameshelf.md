# Actividad autónoma: mi primera app con React

## GameShelf — catálogo de videojuegos

**Duración:** tres sesiones de 55 minutos (165 minutos en total).  
**Entrega:** aplicación React con TypeScript en GitHub y documentación en el archivo README.md del proyecto.

Ve respondiendo las preguntas del README y guardando las capturas a medida que avances; no dejes toda la documentación para la última sesión.

> Si te surge alguna duda durante la práctica, puedes consultar los apuntes que tienes disponibles en el aula virtual. Después, vuelve al paso en el que estabas e intenta continuar aplicando lo que has repasado.

### El reto

Crearás una página con un catálogo de al menos diez videojuegos: conservarás los tres de ejemplo y añadirás al menos siete juegos que te gusten. Para que haya bastantes tarjetas, intenta llegar a 12–15 videojuegos. Incluirás un botón para ver solo los disponibles y, como reto final, otro para filtrar los que cuestan menos de 15 €. Documenta tus decisiones en README.md.

| Sesión | Plan |
|---|---|
| 1 | Crear el proyecto con Vite, investigar las herramientas y archivos, y empezar el README. |
| 2 | Crear los componentes, ampliar el catálogo con juegos propios y diseñar el CSS. |
| 3 | Añadir los filtros, completar el reto, terminar el README y publicar el proyecto en GitHub. |

---

# Sesión 1 — Preparación e interfaz

## 1. Comprueba Node.js y npm

Abre Visual Studio Code y elige **Terminal → Nueva terminal**. Escribe:

```
node --version
npm --version
```

Node.js permite ejecutar JavaScript y herramientas de desarrollo. npm instala y ejecuta paquetes que necesita el proyecto. Si alguno de los comandos no funciona, avisa antes de seguir.

## 2. Crea el proyecto con Vite

En la terminal, sitúate en la carpeta donde guardarás tus trabajos. Ejecuta:

```
npm create vite@latest gameshelf -- --template react-ts
```

- `npm create` ejecuta una herramienta de creación de proyectos.
- `vite@latest` indica que se utilizará la versión más reciente de Vite.
- `gameshelf` será el nombre de la carpeta del proyecto.
- `--template react-ts` pide la plantilla de React con TypeScript.

Si pregunta si quieres instalar create-vite, responde `y` y pulsa Enter. Si aparece un menú, elige React y después TypeScript.

Entra en la carpeta e instala las dependencias:

```
cd gameshelf
npm install
```

Las dependencias son paquetes de código que la aplicación necesita. npm lee package.json y las descarga en node_modules. Esa carpeta se puede volver a generar y no se sube a GitHub.

Abre gameshelf en VS Code. Puedes ejecutar `code .` desde esa carpeta. Si no funciona, usa **Archivo → Abrir carpeta…** y elige gameshelf.

## 3. Ejecuta la aplicación inicial

En la terminal ejecuta:

```
npm run dev
```

Abre en el navegador la dirección que aparezca, normalmente `http://localhost:5173/`. Mantén la terminal abierta: Vite sirve el proyecto y actualiza el navegador cuando guardas cambios. Para pararlo, pulsa Ctrl + C en la terminal.

**Qué hace cada parte:** npm create ejecuta create-vite, que genera la estructura inicial usando la plantilla elegida. Vite es la herramienta que ofrece el servidor local de desarrollo y prepara la compilación para publicar. React es la biblioteca con la que escribiremos los componentes y el comportamiento de la interfaz.

### Investiga qué es cada herramienta

La explicación inicial es solo una pista. Busca en la documentación oficial y escribe en README.md una respuesta propia para cada pregunta:

- ¿Qué es React? ¿Es un lenguaje, una biblioteca o una herramienta para crear proyectos?
- ¿Qué diferencia hay entre create-vite y Vite? ¿Qué hace Vite mientras ejecutamos npm run dev? ¿Qué crees que hará npm run build?
- ¿Qué son Node.js y npm? ¿Para qué usamos npm create, npm install y npm run dev?
- ¿Qué son las dependencias? ¿Por qué aparece node_modules?
- ¿Qué añade TypeScript a JavaScript?
- ¿Qué significan JSX y TSX?

No copies definiciones sin entenderlas. Añade el enlace que consultaste.

## 4. Investiga los archivos

En VS Code, abre los archivos y completa esta tabla en tu README.md. Anota qué observas y para qué crees que sirve cada uno. No borres archivos de configuración. Si necesitas ayuda, consulta los enlaces oficiales del final.

| Archivo/carpeta | Pregunta para investigar | Función (con tus palabras) |
|---|---|---|
| index.html | ¿Qué elemento tiene id="root"? | |
| src/main.tsx | ¿Qué componente importa? ¿Dónde aparece createRoot? | |
| src/App.tsx | ¿Qué interfaz se muestra? | |
| src/App.css | ¿Qué reglas visuales aparecen? | |
| src/index.css | ¿En qué se diferencia de App.css? | |
| package.json | ¿Qué comandos aparecen en scripts? | |
| tsconfig.json | ¿Qué indica “ts”? ¿Necesitas editarlo hoy? | |
| vite.config.ts | ¿Qué nombre de herramienta aparece? | |
| public/ y src/assets/ | ¿Qué recursos encuentras? | |
| node_modules/ | ¿Qué contiene? ¿Por qué no se sube? | |
| .gitignore | ¿Qué archivos o carpetas ignora Git? | |

Responde también: ¿qué archivo contiene el punto de entrada HTML?, ¿qué archivo carga App?, ¿cuál es el componente principal?, ¿qué hace el script dev?, ¿qué significa la extensión .tsx?

### Conceptos que vas a encontrar

| Concepto | Definición inicial |
|---|---|
| JavaScript | Lenguaje de programación usado en la web. |
| TypeScript | Añade comprobación de tipos a JavaScript. |
| JSX | Sintaxis parecida a HTML que se escribe dentro de JavaScript. |
| TSX | Archivo TypeScript que también contiene JSX. |
| Componente | Parte reutilizable de una interfaz que devuelve elementos visuales. |
| Props | Datos que un componente recibe desde otro componente. |
| Función | Bloque de instrucciones que se puede ejecutar al llamarlo. |
| Parámetro | Nombre que recibe un valor dentro de una función. |
| Evento | Acción, por ejemplo, pulsar un botón. |
| Manejador | Función que responde a un evento. |
| Estado | Dato que React conserva para actualizar la interfaz cuando cambia. |
| Callback | Función que se entrega a otra función para que esta la ejecute. |

Estas definiciones son pistas. Investiga cada término que no entiendas y añade una definición propia y el enlace consultado en el glosario del README.

## 5. Crea tu README.md y decide cómo organizarlo

El proyecto ya incluye un archivo README.md en la carpeta principal. Ábrelo, elimina el texto automático de Vite y escribe tu propia documentación en Markdown. **No hay una plantilla que debas copiar**: organiza el documento con títulos y apartados que tengan sentido para ti.

Tu README debe responder las preguntas de investigación de esta actividad y explicar el trabajo realizado. Incluye, como mínimo:

- título del proyecto, autoría y una descripción de GameShelf;
- herramientas utilizadas y cómo ejecutar el proyecto con npm install y npm run dev;
- respuestas sobre React, Vite, create-vite, Node.js, npm, dependencias, JavaScript, TypeScript, JSX y TSX;
- función de los archivos y carpetas que investigaste;
- explicaciones de componentes, props, funciones, parámetros, funciones flecha, eventos, manejadores, estado, callbacks, map(), filter() y key;
- qué reglas CSS creaste, en qué archivo y cómo las relacionaste con className;
- cómo ampliaste el catálogo con tus juegos;
- cómo funciona cada filtro y cómo probaste el reto;
- dificultades que aparecieron y cómo las resolviste;
- enlaces a las fuentes consultadas;
- enlace al repositorio de GitHub.

### Capturas que debe incluir el README

Toma capturas durante el proceso, no solo al final. Guarda las imágenes en una carpeta llamada capturas dentro de gameshelf e insértalas en el README con rutas relativas. En Markdown, una imagen se incluye con esta sintaxis:

```
![Descripción de la imagen](capturas/nombre-de-la-captura.png)
```

Incluye capturas que permitan ver:

- la estructura del proyecto en VS Code;
- la primera pantalla de GameShelf;
- una parte del código y el catálogo ampliado;
- el filtro de disponibilidad activo;
- la aplicación final con el filtro de precio;
- el repositorio publicado en GitHub.

Puedes elegir los nombres de archivo y el orden de los apartados. Comprueba al abrir README.md en GitHub que las imágenes se ven. No incluyas contraseñas, tokens ni datos personales que no quieras publicar.

# Sesión 2 — Componentes, catálogo y estilos

## 6. Cambia la pantalla inicial

Abre src/App.tsx, borra el ejemplo de Vite y escribe:

```
import "./App.css";

function App() {
  return (
    <main className="app">
      <h1>GameShelf</h1>
      <p>Mi catálogo de videojuegos</p>
    </main>
  );
}

export default App;
```

Guarda con Ctrl + S. Debes ver el nuevo título en el navegador.

- `function App()` declara el componente App.
- `return` indica qué interfaz devuelve.
- Las etiquetas main, h1 y p se parecen a HTML y están escritas en JSX.
- En JSX usamos `className` para enlazar una clase CSS; en HTML se usa habitualmente `class`.
- El import carga App.css.
- El export permite que main.tsx pueda utilizar App.

Apunta en el README qué significa “componente” y qué muestra ahora la página.

## 7. Crea la tarjeta de videojuego

Dentro de src, crea la carpeta components. Dentro de ella crea GameCard.tsx. Escribe:

```
type GameCardProps = {
  title: string;
  genre: string;
  price: number;
  available: boolean;
};

export default function GameCard({
  title,
  genre,
  price,
  available,
}: GameCardProps) {
  return (
    <article className="card">
      <h2>{title}</h2>
      <p>Género: {genre}</p>
      <p>Precio: {price} €</p>
      <p>{available ? "Disponible" : "No disponible"}</p>
    </article>
  );
}
```

GameCard recibe props: title, genre, price y available. GameCardProps describe sus nombres y tipos. JavaScript también puede pasar props, pero TypeScript permite comprobar si el componente recibe el tipo de valor esperado.

Dentro de JSX, las llaves permiten insertar expresiones JavaScript. `available ? "Disponible" : "No disponible"` es un ternario: si available es verdadero, aparece “Disponible”; si es falso, aparece “No disponible”.

Si aparece un error, revisa los nombres, dos puntos, llaves, paréntesis y etiquetas.

## 8. Añade datos y muestra tres tarjetas

Reemplaza src/App.tsx con:

```
import "./App.css";
import GameCard from "./components/GameCard";

type Game = {
  id: number;
  title: string;
  genre: string;
  price: number;
  available: boolean;
};

const games: Game[] = [
  { id: 1, title: "Hollow Knight", genre: "Aventura", price: 14.99, available: true },
  { id: 2, title: "Celeste", genre: "Plataformas", price: 19.99, available: true },
  { id: 3, title: "Stardew Valley", genre: "Simulación", price: 13.99, available: false },
];

function App() {
  return (
    <main className="app">
      <h1>GameShelf</h1>
      <p>Mi catálogo de videojuegos</p>
      <section className="catalog">
        {games.map((game) => (
          <GameCard
            key={game.id}
            title={game.title}
            genre={game.genre}
            price={game.price}
            available={game.available}
          />
        ))}
      </section>
    </main>
  );
}

export default App;
```

- `type Game` describe la estructura de cada videojuego.
- string significa texto; number, número; boolean, true o false.
- `Game[]` significa array de elementos Game.
- TypeScript comprueba que los objetos tengan esas propiedades y tipos. JavaScript podría ejecutar el array sin declarar el tipo Game.

map() es un método de arrays de JavaScript. Recorre games y transforma cada juego en un componente GameCard. La parte (game) => (...) es una función flecha que map ejecuta una vez por cada elemento. game es su parámetro: en cada vuelta contiene un videojuego distinto. La función devuelve el componente que se añadirá a la lista.

La propiedad key da a React una identificación estable para reconocer cada tarjeta cuando cambia la lista. key la utiliza React; no es una prop normal de GameCard.

Investiga y responde en README: ¿qué es una función?, ¿qué diferencia hay entre definirla y llamarla?, ¿qué recibe el parámetro game en cada vuelta?, ¿qué devuelve la función flecha dentro de map()? Recuerda indicar qué fuente consultaste.

**Comprueba:** deben aparecer tres tarjetas. Añade en README qué hace map() y por qué se usa key.

### Amplía el catálogo con tus videojuegos favoritos

Antes de seguir, amplía la colección: conserva Hollow Knight, Celeste y Stardew Valley, y añade **al menos siete juegos que te gusten**, para tener diez tarjetas o más. Diez es el mínimo; si puedes, llega a 12–15 tarjetas.

Añade los objetos nuevos dentro del array games, separados por comas. Cada juego necesita un id numérico único, título, género, precio numérico y disponibilidad true o false. Utiliza este patrón y rellena valores reales:

```
{
  id: 4,
  title: "Escribe aquí un videojuego que te guste",
  genre: "Género",
  price: 24.99,
  available: true,
},
```

Cambia el id, el título, el género, el precio y la disponibilidad. Repite con otros seis juegos como mínimo. Comprueba que los id no se repiten y que los precios son números sin comillas. TypeScript mostrará un error si falta una propiedad o tiene un tipo incorrecto.

En README explica cómo elegiste los datos y añade una captura del catálogo ampliado. Prueba los filtros más adelante también con tus juegos: los resultados ya dependerán de los precios y disponibilidades que hayas puesto.

---

## 9. Investiga y crea los estilos CSS

No se proporciona el diseño terminado: vas a investigar CSS y a crear tus propias reglas.

1. Abre src/App.css. Ese archivo contiene CSS normal, no TypeScript ni JSX.
2. App.tsx ya carga ese archivo mediante el import de estilos. Por eso los selectores CSS que escribas en App.css pueden aplicarse a los elementos de la interfaz.
3. En los elementos TSX ya aparecen className="app", className="catalog" y className="card". Investiga cómo se selecciona una clase en CSS y por qué en JSX se escribe className mientras que en el archivo CSS se escribe un selector de clase.
4. Diseña por tu cuenta la presentación del título, el catálogo, las tarjetas y los botones. Puedes consultar documentación o ejemplos de CSS, pero escribe reglas que entiendas.
5. Prueba los cambios en el navegador y guarda una captura de tu diseño.

En README explica: qué es CSS, qué es un selector, qué es una propiedad CSS, dónde escribiste los estilos y cómo conectaste una clase del TSX con su regla CSS. No copies CSS sin poder describir qué hace.

# Sesión 3 — Interactividad, documentación y publicación

## 10. Añade el filtro de disponibilidad

Una variable normal puede cambiar, pero React no actualiza la pantalla automáticamente por ese cambio. Para los datos que afectan a la interfaz utilizamos **estado**.

Antes de escribir el botón, identifica estas ideas:

- Una **función** agrupa instrucciones que podemos ejecutar. En JavaScript podemos declararla, por ejemplo, con function saludar() { ... }.
- **Llamar** a una función significa pedir que se ejecute. En JavaScript normalmente se escribe su nombre seguido de paréntesis.
- Una **función flecha** es otra forma de escribir una función: (valor) => expresión. La flecha separa los parámetros del código que se ejecuta.
- Un **evento** es una acción que ocurre, como un clic.
- El **manejador** es la función que responde a ese evento.
- En JSX, onClick es la prop de evento que React utiliza para escuchar un clic. El nombre lleva C mayúscula.

Mira un ejemplo sencillo de una función y de cómo se conecta a un evento:

```
function saludar() {
  alert("Hola");
}

<button onClick={saludar}>Saludar</button>
```

alert es una función del navegador que muestra un aviso emergente. function saludar() define la función, pero no la ejecuta en ese momento. En JSX, onClick={saludar} entrega esa función para que se ejecute al pulsar. En cambio, onClick={saludar()} la llamaría mientras React prepara la pantalla. La forma onClick={() => saludar()} también espera al clic: entrega una función flecha que, al ejecutarse, llama a saludar.

En el botón de esta actividad pasaremos una función a onClick para que React la ejecute al pulsar. No queremos ejecutar el cambio durante el renderizado.

### 10.1 Importa y utiliza useState

Arriba de src/App.tsx añade este import junto al import de App.css y GameCard:

```
import { useState } from "react";
```

Dentro de function App(), antes de return, añade:

```
const [onlyAvailable, setOnlyAvailable] = useState(false);
```

useState es un **hook** de React. false es el valor inicial. onlyAvailable representa el valor actual y setOnlyAvailable es la función para cambiarlo. Los corchetes usan desestructuración, sintaxis de JavaScript. TypeScript deduce que el estado es booleano porque empieza con false.

### 10.2 Calcula qué juegos mostrar

Debajo del estado y antes del return, escribe:

```
const visibleGames = onlyAvailable
  ? games.filter((game) => game.available)
  : games;
```

filter() es un método de array de JavaScript que crea una lista con los elementos que cumplen una condición. La parte (game) => game.available es otra función flecha: filter llama a esa función para cada juego y conserva los que devuelven true.

El ternario significa: si onlyAvailable es verdadero, usa la lista filtrada; si no, muestra games completo. Tanto filter como las funciones flecha son JavaScript; React representa el resultado en la interfaz.

### 10.3 Añade el botón

Dentro del return, debajo del párrafo, añade:

```
<button onClick={() => setOnlyAvailable(!onlyAvailable)}>
  {onlyAvailable ? "Mostrar todos" : "Mostrar disponibles"}
</button>
```

En el map(), reemplaza `games.map(...)` por `visibleGames.map(...)`.

onClick es la prop que escucha el evento de clic en JSX. El código () => setOnlyAvailable(!onlyAvailable) es una función flecha sin parámetros: no se ejecuta al preparar la pantalla, sino cuando ocurre el clic. Al ejecutarse, llama a setOnlyAvailable. El signo ! invierte el valor: false cambia a true y true a false.

Compara estas dos ideas: pasar una función para que se ejecute después y llamar la función inmediatamente. En un evento queremos la primera. Escribe esta diferencia en README y explica qué hace cada parte de () => setOnlyAvailable(!onlyAvailable).

**Comprueba:** al principio aparecen tres tarjetas; al activar “Mostrar disponibles”, quedan dos; al pulsar de nuevo, vuelven las tres.

En README, explica qué diferencia hay entre estado y variable normal, y qué hace onClick.

## 11. Mini reto: juegos por menos de 15 €

Añade un segundo botón que muestre solo juegos que cuesten menos de 15 €. Al desactivarlo, debe volver a la lista que resulta del filtro de disponibilidad. Los dos filtros deben combinarse.

Intenta resolverlo primero sin copiar una solución. Usa estas pistas:

1. Declara un segundo estado booleano que empiece en false; por ejemplo, onlyUnder15 y setOnlyUnder15.
2. El cálculo visibleGames ya parte del filtro de disponibilidad. Aplica el precio a esa lista; si vuelves a empezar por games, los filtros no se combinarán.
3. Usa filter() y la condición game.price < 15.
4. Añade el botón con onClick y cambia su texto con un ternario.
5. Haz que map() use la lista resultante de ambos filtros.
6. En README explica que useState devuelve el valor actual y una función actualizadora, y que React vuelve a renderizar el componente al actualizar el estado.

Hazte estas preguntas: ¿qué significa game.price < 15?, ¿qué lista debe pasar por el segundo filter()?, ¿cómo inviertes el estado?, ¿qué texto verá la persona en cada estado?

### Resultados para comprobar

| Filtros activados | Resultado |
|---|---|
| Ninguno | Hollow Knight, Celeste y Stardew Valley |
| Menos de 15 € | Hollow Knight y Stardew Valley |
| Disponibles y menos de 15 € | Hollow Knight |

Con solo los tres juegos iniciales, el filtro de precio muestra Hollow Knight y Stardew Valley, y al combinarlo con disponibilidad queda Hollow Knight. Después de añadir tus juegos, el resultado variará según tus datos. Comprueba que cada filtro conserva exactamente los juegos que cumplen su condición. No cambies el array original: filter() calcula listas nuevas para representar.

**Extra si terminas:** cambia el límite a 18 €, mejora los botones o añade un contador con la propiedad length de la lista visible.

## 12. Comprueba y completa el README

Marca cada punto:

- [ ] Se muestran el título y las tres tarjetas.
- [ ] Las tarjetas muestran título, género, precio y disponibilidad.
- [ ] El filtro de disponibilidad funciona.
- [ ] El filtro de precio funciona.
- [ ] Ambos filtros funcionan combinados.
- [ ] Cada elemento de la lista tiene una key estable.
- [ ] El navegador no muestra errores.
- [ ] README.md explica los archivos, conceptos, reto, errores resueltos y fuentes.
- [ ] No se han borrado package.json, .gitignore ni otros archivos de configuración.

Anota en README un error que hayas tenido y cómo lo resolviste.

## 13. Sube el proyecto a GitHub

Git guarda versiones del proyecto. GitHub aloja el repositorio en internet.

### 13.1 Crea un repositorio vacío

1. En GitHub selecciona **New repository**.
2. Nombre: `react-gameshelf`.
3. Elige la visibilidad que indique la profesora.
4. Créalo **sin** README, licencia ni .gitignore: esos archivos ya existen en tu proyecto local.
5. Pulsa **Create repository** y copia la URL HTTPS. Será similar a `https://github.com/USUARIO/react-gameshelf.git`. Cambia USUARIO por tu nombre de GitHub.

### 13.2 Comprueba si Git ya está iniciado

En la terminal de VS Code, confirma que estás dentro de gameshelf y ejecuta:

```
git status
```

Si muestra el estado del proyecto, continúa. Si dice que no es un repositorio Git, ejecuta:

```
git init -b main
```

Si tu versión no acepta `-b main`, ejecuta `git init` y después `git branch -M main`.

### 13.3 Guarda la primera versión y conéctala

Desde la carpeta gameshelf ejecuta los comandos. En la URL, sustituye USUARIO por tu usuario:

```
git add .
git commit -m "Crear primera versión de GameShelf"
git branch -M main
git remote add origin https://github.com/USUARIO/react-gameshelf.git
git push -u origin main
```

| Comando | Función |
|---|---|
| `git add .` | Prepara los archivos para guardar una versión. |
| `git commit -m "..."` | Guarda una versión con un mensaje. |
| `git branch -M main` | Nombra main a la rama actual. |
| `git remote add origin URL` | Conecta el proyecto local con GitHub. |
| `git push -u origin main` | Sube la versión al repositorio remoto. |

Recarga la página del repositorio. Debes ver el código y README.md. Si haces cambios después, súbelos así:

```
git add .
git commit -m "Añadir filtro de precio"
git push
```

No subas contraseñas, tokens ni claves. node_modules debe quedar excluida mediante .gitignore. Si Git muestra un error, copia el mensaje y pide ayuda en vez de repetir comandos a ciegas.

## 14. Entrega y reflexión

Entrega en el aula virtual la dirección de tu repositorio y comprueba que la última versión está subida. Completa en README, con tus palabras:

1. ¿Qué archivo conecta el HTML con App?
2. ¿Qué diferencias hay entre React, TypeScript y Vite?
3. ¿Qué son las props de GameCard?
4. ¿Por qué usamos setOnlyAvailable para cambiar el estado?
5. ¿Qué hacen map() y filter()?
6. ¿Para qué sirve key?
7. ¿Qué error te costó más resolver y cómo lo solucionaste?

## Fuentes oficiales

- [Guía de inicio de Vite](https://vite.dev/guide/)
- [Aprender React](https://react.dev/learn)
- [Primer componente de React](https://react.dev/learn/your-first-component)
- [Estado de un componente](https://react.dev/learn/state-a-components-memory)
- [Mostrar listas en React](https://react.dev/learn/rendering-lists)
- [Añadir código local a GitHub](https://docs.github.com/es/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github)

