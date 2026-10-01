# Actividad final: desarrollo de una Pokédex con JavaScript

## ⚠️ Lee esto antes de empezar

La entrega de esta actividad será **el enlace al repositorio público de GitHub en el que hayas desarrollado y alojado el proyecto**.

El repositorio deberá contener obligatoriamente:

1. El código completo y funcional de la Pokédex.
2. Un archivo `README.md` que documente **paso a paso todo el proceso de desarrollo**.
3. Capturas de pantalla que demuestren el funcionamiento de cada fase.
4. Un historial de commits que permita comprobar la evolución real del proyecto.

> **No dejes el `README.md` para el final.** Debes actualizarlo a medida que avanzas en la actividad. Cada vez que termines una fase, documenta qué has hecho, qué código has modificado, cómo lo has comprobado y qué resultado has obtenido.

### Primer paso obligatorio: mostrar el punto de partida

Antes de añadir ninguna funcionalidad nueva, deberás subir al repositorio el código resultante de la práctica guiada anterior y comprobar que funciona correctamente.

El primer apartado del `README.md` se titulará:

```markdown
## 1. Punto de partida
```

En este apartado deberás incluir:

- Una explicación breve de la mini-Pokédex inicial.
- La estructura inicial de carpetas y archivos.
- Una captura de la aplicación funcionando.
- Una captura de una búsqueda correcta.
- Una captura de la gestión de un Pokémon inexistente o de otro error controlado.
- Una explicación de las funcionalidades que ya estaban implementadas.
- El identificador o enlace al commit que contiene este punto de partida.

No continúes con las ampliaciones hasta haber comprobado que:

- La página carga correctamente.
- La búsqueda funciona por nombre o número.
- La información del Pokémon aparece en pantalla.
- Los errores se muestran mediante mensajes comprensibles.
- No aparecen errores en la consola durante el funcionamiento normal.

Después de documentar y confirmar el punto de partida, podrás comenzar las siguientes fases de desarrollo. **Todas las fases deberán quedar explicadas y acompañadas de evidencias en el `README.md`.**

---

## 1. Contexto de la actividad

En esta actividad ampliarás la **mini-Pokédex desarrollada en la guía práctica anterior** hasta convertirla en una Pokédex más completa. La aplicación consultará información real de Pokémon mediante [PokéAPI](https://pokeapi.co/) y generará su contenido dinámicamente con JavaScript.

> **No debes comenzar un proyecto nuevo desde cero.** Debes partir expresamente del código creado durante la práctica guiada `Práctica guiada: crea una mini-Pokédex`. Copia o reutiliza sus archivos `index.html`, `css/style.css` y `js/app.js` y realiza sobre ellos las modificaciones necesarias.

El código anterior ya permite buscar un Pokémon por su nombre o número y mostrar una tarjeta básica. Tu trabajo consistirá en **conservar las funcionalidades que ya operan correctamente, reorganizarlas cuando sea necesario y ampliarlas** para cumplir todos los requisitos de esta actividad.

Antes de empezar, realiza una copia o crea una nueva rama para conservar intacta la versión completada durante la guía.

> La actividad debe realizarse con **HTML, CSS y JavaScript puro**. No es necesario utilizar Node.js ni otras bibliotecas externas.

---

## 2. Objetivos

Con esta actividad practicarás:

- La consulta de una API mediante `fetch()`.
- El uso de funciones asíncronas con `async` y `await`.
- La lectura y transformación de información en formato JSON.
- El trabajo con arrays y objetos.
- El uso de métodos como `map()`, `filter()` y `join()`.
- La selección y modificación de elementos del DOM.
- La gestión de eventos.
- La creación dinámica de elementos o fragmentos HTML.
- La validación de datos introducidos por el usuario.
- La gestión de estados de carga y errores.
- La organización de un proyecto web en diferentes archivos.
- El uso de Git y GitHub para documentar el proceso de desarrollo.

---

## 3. Resultado esperado

Partiendo de la mini-Pokédex anterior, la aplicación resultante mostrará una colección con los **151 Pokémon de la primera generación**.

Cada Pokémon aparecerá en una tarjeta. Inicialmente, la tarjeta mostrará su sprite de espaldas. Cuando el usuario coloque el cursor sobre la imagen o la tarjeta, el sprite deberá cambiar y mostrar al Pokémon de frente. Al retirar el cursor, deberá volver a verse de espaldas.

Además, la aplicación permitirá buscar Pokémon y consultar información adicional proporcionada por PokéAPI.

---

## 4. Requisitos funcionales obligatorios

### 4.1. Carga inicial

Al abrir la aplicación deberá mostrarse:

- Un encabezado con el título o logotipo de la Pokédex.
- Una breve explicación sobre su funcionamiento.
- Una barra de búsqueda.
- Un botón para iniciar la carga o consultar los Pokémon.
- Una zona destinada a mensajes de estado.
- Un contenedor para las tarjetas.

Cuando comience la consulta, la aplicación deberá informar al usuario mediante un mensaje similar a:

```text
Cargando Pokémon...
```

El mensaje deberá desaparecer o cambiar cuando termine la carga.

### 4.2. Consulta de PokéAPI

La información se obtendrá desde el recurso `pokemon` de PokéAPI:

```text
https://pokeapi.co/api/v2/pokemon/{nombre-o-id}
```

La aplicación deberá obtener los datos de los Pokémon con identificadores comprendidos entre el `1` y el `151`.

No se permite escribir manualmente en el código los nombres, tipos, medidas o estadísticas de los Pokémon.

### 4.3. Tarjetas de Pokémon

Cada tarjeta deberá mostrar, como mínimo:

- Número de la Pokédex.
- Nombre.
- Sprite del Pokémon.
- Uno o dos tipos.
- Altura expresada en metros.
- Peso expresado en kilogramos.

Los nombres y tipos deberán mostrarse de forma legible, aunque la API los devuelva en minúsculas.

La altura y el peso deberán convertirse a las unidades solicitadas. Consulta qué unidades utiliza PokéAPI antes de realizar la conversión.

### 4.4. Cambio de sprite

Cada tarjeta deberá utilizar:

- `sprites.back_default` como imagen inicial.
- `sprites.front_default` al colocar el cursor sobre la tarjeta o sobre la imagen.

Al retirar el cursor deberá volver a aparecer el sprite trasero.

El intercambio debe producirse sin realizar una nueva petición a la API.

> No deben verse los dos sprites simultáneamente.

Puedes resolverlo mediante CSS, eventos de JavaScript o una combinación de ambos. La solución debe funcionar en todas las tarjetas.

### 4.5. Barra de búsqueda

La aplicación deberá incluir una barra que permita buscar por:

- Nombre completo del Pokémon.
- Número de la Pokédex.
- Fragmento del nombre.

Ejemplos de búsquedas válidas:

```text
pikachu
25
char
```

La búsqueda deberá ignorar espacios innecesarios y diferencias entre mayúsculas y minúsculas.

Mientras el usuario escribe o cuando envía el formulario, se mostrarán únicamente las tarjetas que coincidan con la búsqueda.

Si no existe ninguna coincidencia, deberá aparecer un mensaje claro y el contenedor de resultados quedará vacío.

Cuando la barra vuelva a estar vacía, deberán aparecer nuevamente los 151 Pokémon.

### 4.6. Información ampliada

Cada tarjeta incluirá un botón con un texto como `Ver detalles`.

Al pulsarlo se mostrará una zona ampliada, panel o ventana con la siguiente información:

- Nombre y número del Pokémon.
- Imagen frontal en mayor tamaño.
- Tipo o tipos.
- Altura.
- Peso.
- Experiencia base: `base_experience`.
- Habilidades: `abilities`.
- Estadísticas base: `stats`.

Como mínimo, deberán identificarse estas estadísticas:

- Puntos de salud (`hp`).
- Ataque (`attack`).
- Defensa (`defense`).
- Ataque especial (`special-attack`).
- Defensa especial (`special-defense`).
- Velocidad (`speed`).

El panel deberá poder cerrarse sin recargar la página.

### 4.7. Filtrado por tipo

La aplicación deberá incluir un selector que permita:

- Mostrar todos los Pokémon.
- Mostrar únicamente los Pokémon de un tipo determinado.

Por ejemplo:

```text
Todos
fire
water
grass
electric
```

Las opciones del selector deberán obtenerse a partir de los tipos presentes en los Pokémon cargados. No será necesario escribir manualmente toda la lista de tipos.

El filtro por tipo y la barra de búsqueda deberán poder utilizarse al mismo tiempo.

### 4.8. Estados y gestión de errores

La aplicación deberá contemplar, al menos, estos estados:

- Aplicación preparada para comenzar.
- Datos cargándose.
- Datos cargados correctamente.
- Búsqueda sin resultados.
- Error al comunicarse con PokéAPI.

Si se produce un error:

- Se mostrará un mensaje comprensible.
- La aplicación no deberá quedarse bloqueada.
- El usuario deberá poder volver a intentarlo.

No se mostrará en la interfaz un error técnico sin explicación como `undefined`, `[object Object]` o un mensaje propio de la consola.

---

## 5. Requisitos de diseño y usabilidad

La interfaz deberá:

- Utilizar una cuadrícula de tarjetas.
- Adaptarse a ordenadores y dispositivos móviles.
- Mantener tamaños y separaciones coherentes.
- Diferenciar visualmente los tipos de Pokémon.
- Mostrar claramente qué elementos son interactivos.
- Conservar la legibilidad de los textos sobre el fondo utilizado.
- Incluir textos alternativos adecuados en las imágenes.
- Permitir realizar una búsqueda pulsando el botón o la tecla Enter.

El diseño puede ser propio. No es necesario copiar exactamente la aplicación de referencia. **Aprovechen y sean creativos**. Háganlo a su gusto.

---

## 6. Organización recomendada

Reutiliza la estructura de la práctica guiada y amplíala hasta que el proyecto tenga, como mínimo, los siguientes elementos:

```text
pokedex/
├── index.html
├── README.md
├── assets/
│   └── images/
├── css/
│   └── style.css
└── js/
    ├── app.js
    └── Pokemon.js
```

El uso de `Pokemon.js` es obligatorio. Este archivo deberá contener la clase o estructura responsable de representar los datos necesarios de cada Pokémon.

No almacenes dentro de cada objeto toda la respuesta de la API si no vas a utilizarla. Selecciona y transforma únicamente la información necesaria para la aplicación.

---

## 7. Desarrollo por fases

### Fase 1. Preparación

1. Recupera el proyecto terminado en la práctica guiada anterior.
2. Haz una copia de seguridad o crea una rama nueva para esta actividad.
3. Comprueba que la búsqueda individual continúa funcionando antes de modificar el código.
4. Conserva los archivos `index.html`, `css/style.css` y `js/app.js` como punto de partida.
5. Amplía la estructura incorporando `README.md`, `assets/` y `js/Pokemon.js`.
6. Realiza un commit inicial que identifique claramente el código de partida.
7. Documenta el punto de partida en el primer apartado del `README.md` e incluye las capturas solicitadas.
8. Sube el commit inicial a GitHub y comprueba que puede consultarse desde el repositorio.
9. A partir de ese momento, modifica progresivamente el HTML, el CSS y JavaScript.

### Fase 2. Consulta de datos

1. Prueba una petición para un único Pokémon.
2. Examina la respuesta JSON en la consola.
3. Localiza cada propiedad necesaria.
4. Crea una representación simplificada del Pokémon.
5. Amplía la consulta hasta obtener los 151 Pokémon.
6. Controla el estado de carga y los posibles errores.

### Fase 3. Tarjetas

1. Crea y muestra una tarjeta de prueba.
2. Genera las tarjetas desde el array de Pokémon.
3. Representa correctamente los Pokémon de uno y dos tipos.
4. Implementa el cambio entre sprite trasero y frontal.
5. Adapta la cuadrícula a distintos tamaños de pantalla.

### Fase 4. Búsqueda y filtros

1. Normaliza el texto introducido por el usuario.
2. Filtra por nombre, fragmento o identificador.
3. Genera las opciones del selector de tipos.
4. Combina el filtro de texto con el filtro de tipo.
5. Muestra un mensaje cuando no existan coincidencias.

### Fase 5. Detalles

1. Añade el botón `Ver detalles` a las tarjetas.
2. Crea el panel de información ampliada.
3. Muestra habilidades y estadísticas.
4. Añade el mecanismo para cerrar el panel.

### Fase 6. Revisión y entrega

1. Realiza todas las pruebas indicadas en este documento.
2. Corrige errores de consola.
3. Completa el `README.md`.
4. Revisa el historial de commits.
5. Publica o entrega el enlace del repositorio.

---

## 8. Pruebas mínimas obligatorias

Antes de entregar, comprueba y documenta estos casos:

| Prueba | Resultado esperado |
|---|---|
| Abrir la aplicación | Se muestra la interfaz inicial sin errores |
| Iniciar la carga | Aparece un mensaje de carga |
| Finalizar la consulta | Se muestran 151 tarjetas |
| Buscar `pikachu` | Solo aparece Pikachu |
| Buscar `25` | Solo aparece Pikachu |
| Buscar `char` | Aparecen los Pokémon cuyo nombre contiene ese fragmento |
| Buscar un nombre inexistente | Se muestra un mensaje sin errores técnicos |
| Vaciar la búsqueda | Vuelven a mostrarse todos los Pokémon |
| Seleccionar el tipo `fire` | Solo aparecen Pokémon de tipo fuego |
| Combinar texto y tipo | Se cumplen simultáneamente ambos filtros |
| Colocar el cursor sobre una tarjeta | El sprite cambia de espalda a frente |
| Retirar el cursor | Vuelve a mostrarse el sprite trasero |
| Pulsar `Ver detalles` | Aparece toda la información ampliada solicitada |
| Cerrar los detalles | El panel desaparece sin recargar la página |
| Simular un fallo de conexión | Aparece un mensaje y se puede reintentar |
| Reducir el ancho de la ventana | Las tarjetas se adaptan sin desbordamientos |

Incluye en el `README.md` una pequeña tabla indicando si cada prueba se ha superado y añade capturas de pantalla de las funcionalidades principales.

---

## 9. Contenido del `README.md`

El `README.md` no será únicamente una descripción final del proyecto: será la **memoria paso a paso de su desarrollo**. Deberá actualizarse durante la realización de la actividad y utilizar títulos, explicaciones, fragmentos breves de código, capturas y resultados de pruebas.

Como mínimo, deberá incluir estos apartados:

### 1. Punto de partida

- Descripción de la mini-Pokédex obtenida en la práctica guiada.
- Estructura inicial del proyecto.
- Funcionalidades que ya estaban disponibles.
- Pruebas realizadas antes de comenzar las ampliaciones.
- Capturas que demuestren que el proyecto inicial funciona.
- Enlace o identificador del commit inicial.

### 2. Carga de los 151 Pokémon

- Cambios realizados respecto al código inicial.
- Explicación de la consulta y transformación de los datos.
- Problemas encontrados y soluciones aplicadas.
- Capturas de la colección cargada.

### 3. Construcción de las tarjetas

- Datos seleccionados de PokéAPI.
- Explicación de la generación dinámica de las tarjetas.
- Implementación del cambio entre el sprite trasero y el frontal.
- Capturas del resultado normal y del estado al pasar el cursor.

### 4. Barra de búsqueda y filtros

- Explicación del funcionamiento de la búsqueda.
- Explicación del filtro por tipo.
- Forma de combinar ambos filtros.
- Capturas de varios casos de prueba.

### 5. Información ampliada

- Explicación del panel de detalles.
- Datos adicionales mostrados.
- Capturas del panel abierto y cerrado.

### 6. Gestión de estados y errores

- Estado de carga.
- Búsquedas sin resultados.
- Errores de comunicación con PokéAPI.
- Capturas o evidencias de las pruebas realizadas.

### 7. Pruebas finales

- Tabla completa de pruebas.
- Resultado obtenido en cada caso.
- Correcciones realizadas después de las pruebas.

### 8. Conclusiones

- Dificultades encontradas.
- Conocimientos adquiridos.
- Posibles mejoras futuras.

Además, el archivo deberá incluir:

- Nombre del proyecto.
- Nombre del autor o autora.
- Descripción de la aplicación.
- Tecnologías utilizadas.
- Instrucciones para ejecutarla.
- Estructura del proyecto.
- Funcionalidades implementadas.
- Enlaces a commits relevantes cuando se termine cada fase.

> Las capturas deben guardarse dentro del propio repositorio, por ejemplo en `assets/readme/`, y mostrarse correctamente desde el `README.md`. No se admitirán imágenes enlazadas únicamente desde una ruta local del ordenador.

---

## 10. Ampliaciones voluntarias

Cuando todos los requisitos obligatorios funcionen, puedes incorporar una o varias mejoras:

- Ordenar por número o nombre.
- Ordenar por peso, altura o experiencia base.
- Mostrar el sprite `front_shiny`.
- Incorporar un interruptor entre versión normal y shiny.
- Añadir botones para avanzar al Pokémon anterior o siguiente.
- Mostrar algunos movimientos del Pokémon.
- Añadir barras visuales para las estadísticas.
- Guardar Pokémon favoritos mediante `localStorage`.
- Incorporar paginación.
- Permitir cambiar entre cuadrícula y lista.
- Añadir animaciones que no dificulten la lectura.

**Las ampliaciones no sustituyen ningún requisito obligatorio.**

---

## 11. Restricciones

- Se debe partir del código desarrollado en la práctica guiada anterior.
- No se aceptará rehacer la actividad utilizando otro proyecto completamente distinto.
- Las funcionalidades previas que se conserven deberán seguir funcionando después de cada ampliación.
- No se utilizarán frameworks ni bibliotecas de JavaScript.
- No se copiará una solución completa de Internet.
- No se escribirán manualmente los datos de los 151 Pokémon.
- No se realizarán nuevas peticiones al pasar el cursor sobre las tarjetas.
- No se aceptará una aplicación que solo funcione para Pokémon concretos.
- No se utilizará `document.write()`.
- No se admitirán errores en la consola durante el funcionamiento normal.
- El repositorio deberá contener varios commits descriptivos y repartidos durante el desarrollo.

---

## 12. Criterios de evaluación

| Apartado | Porcentaje |
|---|---:|
| Consulta y transformación de datos de PokéAPI | 20 % |
| Generación de las 151 tarjetas | 15 % |
| Cambio correcto entre sprites | 10 % |
| Barra de búsqueda | 10 % |
| Filtro por tipo y combinación de filtros | 10 % |
| Panel con información ampliada | 15 % |
| Gestión de carga, errores y casos sin resultados | 10 % |
| Diseño adaptable, accesibilidad y usabilidad | 5 % |
| Organización, calidad del código, Git y documentación | 5 % |
| **Total** | **100 %** |

Para superar la actividad deben funcionar, como mínimo:

- La consulta a PokéAPI.
- La visualización de las tarjetas.
- La búsqueda.
- El cambio de sprite.
- La gestión básica de errores.

---

## 13. Entrega

La entrega se realizará mediante una tarea del aula virtual. Deberás entregar **el enlace al repositorio público de GitHub** donde esté alojado el proyecto.

El repositorio entregado deberá contener:

- Todo el código fuente de la aplicación.
- Los recursos utilizados por el proyecto.
- El `README.md` con el desarrollo completo y explicado paso a paso.
- Las capturas empleadas en la documentación.
- Un historial de commits progresivo y descriptivo.

En la tarea del aula virtual deberás incluir:

1. El enlace al repositorio público de GitHub.
2. El enlace a la aplicación publicada, si se solicita.
3. Un comentario breve indicando qué ampliaciones voluntarias has realizado.

No se entregará el proyecto mediante un archivo comprimido, documentos independientes ni capturas sueltas. Toda la información deberá encontrarse organizada dentro del repositorio.

El proyecto deberá poder ejecutarse siguiendo únicamente las instrucciones de su `README.md`.

El historial del repositorio deberá permitir distinguir el código inicial procedente de la práctica guiada y las ampliaciones realizadas para esta actividad.

---

## 14. Lista de comprobación final

Antes de entregar, confirma:

- [ ] He partido del código de la práctica guiada anterior.
- [ ] He conservado una copia o rama con la versión inicial.
- [ ] El historial permite identificar el código de partida y las ampliaciones.
- [ ] He subido el punto de partida funcional antes de realizar las ampliaciones.
- [ ] El primer apartado del `README.md` demuestra que el punto de partida funcionaba.
- [ ] He documentado cada fase a medida que la desarrollaba.
- [ ] Las capturas están guardadas y se muestran desde el repositorio.
- [ ] La entrega es un enlace al repositorio público de GitHub.
- [ ] La aplicación obtiene los datos desde PokéAPI.
- [ ] Se muestran los 151 Pokémon.
- [ ] Cada tarjeta tiene número, nombre, tipos, altura y peso.
- [ ] Inicialmente se muestra el sprite trasero.
- [ ] Al pasar el cursor se muestra únicamente el sprite frontal.
- [ ] La búsqueda funciona por nombre, número y fragmento.
- [ ] El filtro por tipo funciona junto con la búsqueda.
- [ ] El panel ampliado muestra habilidades y estadísticas.
- [ ] Los estados de carga y error son comprensibles.
- [ ] La interfaz se adapta a móvil y escritorio.
- [ ] No aparecen errores durante el uso normal.
- [ ] El repositorio contiene commits descriptivos.
- [ ] El `README.md` está completo.
- [ ] Las pruebas obligatorias están documentadas.

Cuando todas las casillas estén marcadas, la actividad estará preparada para su entrega.
