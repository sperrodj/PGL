# 1. Introducción al desarrollo móvil

## 1. ¿Qué es el desarrollo móvil?

El desarrollo móvil comprende el diseño, la creación, las pruebas, la distribución y el mantenimiento de aplicaciones pensadas para teléfonos y tabletas. No consiste únicamente en escribir código: también hay que entender quién utilizará la aplicación, qué problema resuelve y en qué condiciones se usará.

Una persona puede consultar una app durante unos segundos, mientras camina, con una sola mano, en una pantalla pequeña y con una conexión que cambia. La aplicación debe responder bien a ese contexto.

| Aspecto | Ordenador de escritorio | Dispositivo móvil |
|---|---|---|
| Interacción habitual | Ratón y teclado | Pantalla táctil y gestos |
| Pantalla | Normalmente amplia | Tamaños, densidades y formatos diversos |
| Conexión | A menudo estable | Puede cambiar o interrumpirse |
| Energía | Alimentación frecuente | Batería limitada |
| Recursos | Dependen del equipo | Cámara, ubicación, sensores y notificaciones |
| Uso | Sesiones a menudo prolongadas | Consultas breves y situacionales |

**Ejemplo:** una tabla con muchas columnas puede funcionar en un ordenador. En un móvil quizá sea mejor mostrar un resumen y abrir el detalle al seleccionar una fila.

## 2. Particularidades del móvil

### Pantallas y orientación

Hay teléfonos estrechos, grandes, tabletas y dispositivos plegables. La interfaz debe organizarse según el espacio disponible y seguir siendo usable al cambiar la orientación o aumentar el tamaño del texto.

### Interacción táctil

Se toca, desliza, arrastra o mantiene pulsado. Un dedo no tiene la precisión de un cursor, así que conviene dar espacio a los controles y no ocultar acciones importantes detrás de un efecto *hover*.

### Sensores y funciones del dispositivo

| Recurso | Uso posible |
|---|---|
| Cámara | Escanear un QR o adjuntar una fotografía |
| Ubicación | Mostrar una ruta o servicios cercanos |
| Micrófono | Grabar una nota de voz |
| Acelerómetro | Detectar movimiento u orientación |
| Notificaciones | Avisar de una cita o pedido |
| Almacenamiento | Conservar preferencias o datos locales |

El acceso a estos recursos está sujeto a las reglas de la plataforma y, en muchos casos, a la autorización de la persona usuaria.

### Ciclo de vida

Una aplicación puede pasar del primer plano al segundo plano, volver más tarde o ser detenida por el sistema. Debe evitar perder innecesariamente lo que la persona estaba haciendo. Por ejemplo, si alguien cambia de aplicación mientras rellena un formulario, sería conveniente conservar el borrador.

## 3. Tipos de aplicaciones

Los enfoques pueden solaparse. Por ejemplo, una PWA es una aplicación web y algunas soluciones híbridas comparten mucho código entre plataformas.

### Aplicación nativa

Se crea específicamente para un sistema operativo y utiliza sus herramientas y APIs.

- Android: Kotlin y Android Studio son opciones habituales.
- iOS: Swift y Xcode son opciones habituales.

**Ventajas:** integración directa con la plataforma y acceso amplio a sus funciones.  
**Inconvenientes:** mantener aplicaciones para Android e iOS puede implicar más trabajo y conocimientos distintos.

### Aplicación web adaptada al móvil

Se usa desde el navegador y normalmente se construye con HTML, CSS y JavaScript o TypeScript.

**Ventajas:** se accede mediante un enlace y las actualizaciones se publican en el servidor.  
**Limitaciones:** las funciones disponibles dependen del navegador y del sistema operativo.

### Aplicación web progresiva (PWA)

Es una aplicación web que puede incorporar capacidades como instalación, icono propio, caché y ciertos usos sin conexión, según la implementación y la compatibilidad de cada plataforma.

- El **Web App Manifest** describe datos como el nombre y los iconos.
- Un **Service Worker** puede gestionar recursos y estrategias de caché.
- El uso sin conexión debe diseñarse expresamente; no aparece automáticamente por instalar la PWA.

### Aplicación híbrida

Combina tecnologías web con un contenedor que permite distribuir la aplicación como app móvil. La interfaz puede ejecutarse en una vista web y comunicarse con funciones nativas mediante APIs o complementos. Ionic con Capacitor es un ejemplo de este enfoque.

### Aplicación multiplataforma

Permite compartir una parte importante del código entre sistemas operativos. No significa necesariamente que la aplicación sea una página web dentro de una ventana.

React Native y Flutter son ejemplos de tecnologías multiplataforma con enfoques distintos. Puede seguir siendo necesario adaptar partes del proyecto y probar cada sistema.

### Comparación rápida

| Enfoque | ¿Se instala? | Acceso al dispositivo | Código compartido |
|---|---|---|---|
| Nativo | Habitualmente sí | Amplio y directo | Menor entre plataformas |
| Web móvil | No es necesario | Según navegador | Alto |
| PWA | Puede instalarse | Según navegador y plataforma | Alto |
| Híbrido | Habitualmente sí | Mediante el contenedor y complementos | Alto |
| Multiplataforma | Habitualmente sí | Mediante el framework y sus integraciones | Alto, pero no siempre total |

## 4. Lenguaje, framework, IDE, SDK y API

Estos términos describen cosas diferentes:

- **Lenguaje:** sirve para expresar instrucciones, por ejemplo JavaScript, TypeScript, Kotlin, Swift o Dart.
- **Framework:** ofrece herramientas y convenciones para construir una aplicación.
- **IDE o editor:** programa que usamos para escribir, ejecutar y depurar código.
- **SDK:** conjunto de herramientas y bibliotecas para desarrollar para una plataforma.
- **API:** interfaz que permite solicitar datos o utilizar una función de otro sistema.

Conocer esta diferencia evita confundir, por ejemplo, React Native (framework) con TypeScript (lenguaje) o Visual Studio Code (editor).

## 5. Android e iOS

Android e iOS comparten ideas como las pantallas, los permisos, las notificaciones y las tiendas de aplicaciones, pero sus herramientas, guías de interfaz y dispositivos no son idénticos.

| Aspecto | Android | iOS |
|---|---|---|
| Desarrollo nativo habitual | Kotlin y Android Studio | Swift y Xcode |
| Dispositivos | Muchos fabricantes y formatos | Dispositivos Apple |
| Diseño | Guías y componentes de Android | Guías y componentes de Apple |
| Pruebas | Variedad amplia de modelos | Variedad dentro del ecosistema Apple |

Si una aplicación se dirige a ambas plataformas, hay que comprobar que se comporta correctamente en cada una. Puede variar la navegación, la solicitud de permisos, la apariencia de los controles o la disponibilidad de ciertas funciones.

## 6. Elegir una tecnología

No existe una tecnología que sea la mejor para todos los proyectos. Antes de decidir, preguntamos:

1. ¿Quién utilizará la aplicación y desde qué dispositivos?
2. ¿Debe funcionar desde un navegador o instalarse?
3. ¿Necesita cámara, ubicación, Bluetooth o notificaciones?
4. ¿Debe seguir siendo útil sin conexión?
5. ¿Qué nivel de rendimiento requiere?
6. ¿Qué conocimientos tiene el equipo?
7. ¿Cuánto tiempo y presupuesto hay para crear y mantener el producto?
8. ¿Qué datos personales se van a tratar?

### Ejemplos

**Menú digital de un restaurante:** si solo muestra platos, alérgenos, precios y fotos, una web adaptable accesible mediante QR puede ser suficiente. No requiere instalar una app para consultar información sencilla.

**Registro de inspecciones en zonas sin cobertura:** necesita guardar datos y fotos localmente y sincronizarlos después. Es importante elegir una solución móvil que permita diseñar bien ese almacenamiento y la sincronización.

**Aplicación de gráficos 3D complejos:** conviene evaluar herramientas especializadas y probar el rendimiento antes de elegir.

> La elección debe justificarse con los requisitos del proyecto. En un caso real también se estudiarían el equipo, la distribución, el mantenimiento y las pruebas.

## Actividad breve

Elige una aplicación que uses habitualmente y responde:

1. ¿Qué problema resuelve y quién la utiliza?
2. ¿Se usa desde el navegador o se instala?
3. ¿Qué funciones del dispositivo aprovecha?
4. ¿Necesita conexión constante?
5. ¿Podría ofrecerse como una web adaptada al móvil? Justifica tu respuesta.
