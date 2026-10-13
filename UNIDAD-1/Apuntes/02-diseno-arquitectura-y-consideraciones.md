# 2. Diseño, arquitectura y consideraciones del desarrollo móvil

En este tema pensamos qué debe tener en cuenta una aplicación móvil antes de comenzar a programarla. Los ejemplos son conceptuales: la implementación con React Native y Expo se trabajará en la unidad siguiente.

## 1. Diseñar una interfaz móvil

Una buena interfaz ayuda a completar las tareas principales con pocos pasos y sin confusión.

- Priorizar la información importante.
- Mantener una jerarquía visual clara.
- Usar textos legibles y controles táctiles cómodos.
- Dejar separación entre acciones distintas.
- Evitar depender solo del color para comunicar algo.
- Mostrar qué está ocurriendo cuando una operación tarda.
- Prevenir errores y explicar cómo corregirlos.

### Diseño adaptable

La interfaz debe responder al espacio disponible, la orientación y los ajustes del dispositivo. Es mejor usar distribuciones flexibles que fijar tamaños pensando en un solo móvil.

**Ejemplo:** en un teléfono, una biblioteca puede mostrar la lista de libros y abrir el detalle en otra pantalla. En una tableta, puede mostrar la lista y el detalle a la vez.

También hay que respetar las zonas seguras: cámara frontal, esquinas redondeadas y elementos del sistema no deben tapar contenido o botones importantes.

### Interacción táctil

Los botones deben ser fáciles de pulsar y dar respuesta visual. Una acción importante no debería aparecer solo al pasar el ratón, porque el móvil no dispone de un cursor como mecanismo principal.

### Navegación

Algunos patrones comunes son:

- **Pila de pantallas:** abrir un detalle desde una lista y volver atrás.
- **Pestañas:** alternar entre las secciones principales.
- **Menú lateral:** agrupar destinos secundarios cuando sea apropiado.
- **Ventana modal:** completar una tarea breve sin abandonar la pantalla actual.

La persona debe poder anticipar qué ocurrirá al tocar un control y encontrar una forma clara de volver.

## 2. Arquitectura básica

La arquitectura organiza las responsabilidades de la aplicación. Separarlas ayuda a entender, mantener y probar el proyecto.

```text
PERSONA USUARIA
       |
       v
PRESENTACIÓN: pantallas, formularios y controles
       |
       v
LÓGICA: reglas, cálculos y validaciones
       |
       v
DATOS: almacenamiento local o comunicación con un servidor
```

- **Presentación:** muestra información y recoge acciones.
- **Lógica:** aplica reglas, por ejemplo comprobar si una reserva es válida.
- **Datos:** guarda información o la obtiene de otro sistema.

Es un modelo didáctico. En proyectos reales estas responsabilidades pueden dividirse en más partes.

### Ejemplo: añadir una tarea

1. La pantalla recoge el texto escrito.
2. La lógica comprueba que no esté vacío.
3. La capa de datos guarda la tarea en el dispositivo o la envía al servidor.
4. La pantalla muestra la tarea o informa del problema.

### Aplicación local y cliente-servidor

Una aplicación local puede funcionar con datos que solo están en el dispositivo, como una calculadora. Una aplicación cliente-servidor intercambia información con un servidor, como una tienda que consulta pedidos.

```text
MÓVIL -- solicitud HTTPS --> API / SERVIDOR
MÓVIL <-- respuesta JSON --- API / SERVIDOR
```

Una **API** define cómo solicitar información o realizar operaciones. **JSON** es un formato que permite intercambiar datos. Si la respuesta tarda o falla, la aplicación debe indicarlo y permitir recuperarse cuando sea posible.

## 3. Conectividad y funcionamiento sin conexión

La conexión puede ser rápida, lenta o inexistente. Una pantalla que espera datos debería distinguir entre estos estados:

| Estado | Qué puede comunicar la interfaz |
|---|---|
| Cargando | La operación está en curso |
| Correcto | Los datos se han recibido |
| Sin resultados | La consulta funcionó, pero no encontró elementos |
| Error | Algo impidió completar la operación |
| Sin conexión | No se puede acceder al servicio remoto |

«No hay autobuses disponibles» no significa lo mismo que «No se pudo consultar el servidor».

### Almacenamiento local y caché

El **almacenamiento local** conserva información en el dispositivo, como preferencias o borradores. La **caché** reutiliza recursos guardados para evitar volver a descargarlos; no siempre es almacenamiento permanente.

Un diseño *offline-first* intenta mantener disponibles las funciones esenciales sin conexión. Puede guardar cambios y sincronizarlos al recuperar la red. En ese caso hay que decidir qué ocurre si el mismo dato se modifica desde dos dispositivos.

### Consumo de recursos

La batería, los datos móviles, la memoria y la capacidad de procesamiento son limitados. Conviene evitar:

- descargar imágenes enormes para mostrarlas en pequeño;
- repetir solicitudes sin necesidad;
- mantener sensores activos cuando no se usan;
- cargar miles de resultados a la vez;
- ejecutar animaciones o tareas en segundo plano que no aportan valor.

## 4. Permisos, privacidad y seguridad

### Permisos

Un permiso autoriza el acceso a una función o información, como la cámara, el micrófono o la ubicación. Una aplicación debe:

1. Solicitar solo permisos necesarios.
2. Explicar para qué se utilizarán.
3. Pedirlos cuando la persona intente usar la función relacionada.
4. Gestionar la denegación y ofrecer alternativas cuando sea posible.

**Ejemplo:** una app de rutas puede explicar y pedir la ubicación al pulsar «Mostrar mi posición», en vez de solicitarla al abrirse sin contexto.

### Privacidad y seguridad

- **Privacidad:** qué datos se recogen, para qué, cuánto tiempo y con quién se comparten.
- **Seguridad:** cómo se protegen los datos, las cuentas y las operaciones.

Buenas prácticas iniciales:

- Recoger solo los datos necesarios e informar de su uso.
- Utilizar HTTPS para comunicarse con servidores.
- No incluir claves secretas del servidor en el código distribuido.
- Validar las operaciones sensibles también en el servidor.
- Proteger credenciales y datos personales almacenados.
- Mantener las dependencias actualizadas.

Ocultar un botón en la interfaz no protege por sí solo una operación. El servidor también debe comprobar que la persona tiene autorización.

## 5. Accesibilidad y usabilidad

La **usabilidad** se refiere a lo fácil que resulta completar tareas. La **accesibilidad** procura que personas con diferentes capacidades, necesidades y contextos puedan utilizar la aplicación.

- Mantener contraste suficiente y texto legible.
- Permitir aumentar el tamaño del texto.
- Describir iconos y controles para tecnologías de apoyo.
- No transmitir información únicamente mediante colores.
- Ofrecer mensajes de error concretos.
- Evitar gestos complejos cuando exista una alternativa sencilla.
- Mantener un orden de navegación comprensible.

**Ejemplo:** no basta con marcar un campo en rojo. También debemos explicar «Introduce un correo electrónico válido» y señalar el campo afectado.

## 6. Pruebas y distribución

Un emulador ayuda a probar una aplicación, pero también conviene comprobarla en dispositivos reales cuando sea posible.

Antes de darla por lista, revisamos:

- tamaños de pantalla y orientación;
- conexión lenta y ausencia de red;
- permisos aceptados y rechazados;
- textos largos y teclado abierto;
- accesibilidad y tamaño de fuente;
- comportamiento en las plataformas previstas.

Distribuir una aplicación requiere preparar identificador, iconos, versiones, firma, descripción y capturas, además de cumplir las reglas aplicables de cada plataforma. La distribución y publicación se estudiarán cuando corresponda.

## Actividad: planifica una aplicación de rutas

Una empresa quiere que su personal registre rutas de senderismo, incluso en zonas sin cobertura. En parejas, preparad una ficha:

1. Objetivo y personas usuarias.
2. Tres funciones principales.
3. Datos que se guardarán localmente.
4. Qué ocurrirá al perder la conexión y al recuperarla.
5. Permisos necesarios y cuándo se pedirían.
6. Dos medidas de privacidad, seguridad o accesibilidad.
7. Cómo probaríais la aplicación antes de distribuirla.

## Autoevaluación

1. ¿Qué diferencia hay entre una aplicación web y una PWA?
2. ¿Toda aplicación multiplataforma es una web dentro de una ventana?
3. ¿Por qué una pantalla distingue entre «sin resultados» y «error»?
4. ¿Qué responsabilidades separa la arquitectura básica?
5. ¿Qué diferencia hay entre privacidad y seguridad?
6. ¿Por qué hay que validar también en el servidor?
7. ¿Qué significa diseñar con enfoque *offline-first*?
