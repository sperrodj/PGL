# Dossier de React Native con Expo — PGL (2.º DAM)

**Desarrollo de aplicaciones móviles con TypeScript, Expo Router y Android Studio**  
**Modalidad de trabajo:** una sola aplicación Expo y un solo repositorio GitHub para todas las prácticas.  
**Destinatarios:** alumnado de PGL. Este es el material de consulta y trabajo; seguid las prácticas en el orden indicado.

> **Norma principal:** todas las prácticas deben poder abrirse desde el menú de la aplicación. **No eliminéis ni sobrescribáis una práctica para hacer la siguiente.** Cuando una práctica amplíe otra, podéis reutilizar sus componentes, pero conservad una versión accesible de la anterior. Se mantendrán también las prácticas 9 y 9.1 como actividades distintas.

> **Imágenes del dossier:** las marcas «📸 INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página N» indican dónde se añadirá una referencia visual del PDF de partida. Si una captura incluye instrucciones de React Native CLI, TypeORM o rutas antiguas, se utilizará **solo como ejemplo visual**: seguid el código y las rutas Expo de esta versión.

## Índice de contenidos

1. Herramientas y preparación del emulador Android.
2. Proyecto Expo único: instalación, estructura, menú y navegación entre prácticas.
3. Componentes básicos, estado y Flexbox (prácticas 1–11 y 9.1).
4. Formularios, listas e iconos (prácticas 12–19).
5. Navegación con Expo Router: Stack, Drawer y Tabs (prácticas 20–27).
6. APIs, permisos, almacenamiento y WebView (prácticas 28–31).
7. SQLite, consultas, relaciones y transacciones (prácticas 32–40).
8. Entregas, documentación y enlaces oficiales.

---
# 1. Introducción

React Native permite desarrollar interfaces móviles con React, JavaScript o TypeScript. Sus componentes (`View`, `Text`, `TextInput`, `Image`, etc.) se representan mediante controles nativos de Android y iOS. No consiste en envolver una página web dentro de una aplicación.

**Expo** es un conjunto de herramientas y bibliotecas sobre React Native que facilita crear, ejecutar y distribuir aplicaciones móviles. Incluye utilidades de desarrollo y módulos nativos para cámara, ubicación, almacenamiento o SQLite. Usaremos **Expo con TypeScript**, **Expo Router** para navegar entre pantallas y un **dispositivo virtual de Android Studio** para las pruebas. Expo Go es una opción cómoda para el aprendizaje, pero algunas librerías nativas requieren una compilación de desarrollo personalizada.

**React web frente a React Native:** comparten componentes funcionales, props, estado y hooks, pero no se escriben elementos HTML como `<div>` o `<button>`. En su lugar utilizaremos componentes de `react-native`. Tampoco se aplican hojas CSS convencionales: se usan objetos de estilos y `StyleSheet.create()`.

> 📸 **[INSERTAR CAPTURA PROPIA — imagen de una aplicación Expo ejecutándose en un emulador Android]**

# 2. Preparación del entorno de trabajo

## 2.1. Herramientas necesarias

- **Node.js LTS** y npm.
- **Visual Studio Code** (extensiones de TypeScript y React opcionales).
- **Android Studio**, con Android SDK, Android Emulator y un dispositivo virtual configurado.
- Un navegador para consultar la documentación oficial y, opcionalmente, la app Expo Go en un móvil Android.

No instalaremos el antiguo `expo-cli` global ni crearemos el proyecto con `react-native init`. Tampoco es necesario modificar `MainActivity.kt` para trabajar con Expo Router.

Comprueba las versiones desde una terminal:

```bash
node --version
npm --version
```

## 2.2. Configuración del emulador de Android Studio

1. Abre **Android Studio** y completa su asistente de instalación.
2. En **SDK Manager**, comprueba que se han instalado Android SDK Platform, Build-Tools y Platform-Tools. Instala una imagen de sistema Android compatible con el equipo.
3. En **Device Manager**, selecciona **Create device** y elige un teléfono (por ejemplo, un Pixel).
4. Descarga, si hace falta, una imagen de sistema recomendada; completa la creación del AVD.
5. Inicia el emulador desde el botón de ejecución y espera hasta ver la pantalla de Android.
6. Si Expo no lo detecta, revisa la ruta del SDK (`ANDROID_HOME`/`ANDROID_SDK_ROOT` según el entorno), que `platform-tools` esté en `PATH` y que el emulador funcione desde Android Studio.

```bash
adb devices
```

El comando anterior debe mostrar un dispositivo virtual con estado `device`. En Windows, puede ser necesario habilitar la virtualización en la BIOS/UEFI y los componentes de hipervisor compatibles con el emulador.

> 📸 **[INSERTAR CAPTURA PROPIA — Android Studio > Device Manager, con un AVD creado]**
>
> 📸 **[INSERTAR CAPTURA PROPIA — emulador Android encendido y salida de `adb devices`]**

## 2.3. Creación y ejecución de un proyecto Expo

La plantilla recomendada incorpora TypeScript y Expo Router. Se puede fijar la versión de SDK indicada por la documentación del curso; **no mezcles una versión de Expo Go incompatible con el SDK del proyecto**.

```bash
npx create-expo-app@latest MiPrimeraApp --template default@sdk-58
cd MiPrimeraApp
code .
npx expo start
```

En el menú de Expo, pulsa **`a`** para abrir el proyecto en el emulador Android previamente iniciado. Alternativamente, usa el código QR con un dispositivo físico compatible y Expo Go. Para limpiar la caché de Metro:

```bash
npx expo start --clear
```

**Nota de compatibilidad (noviembre de 2026):** el sitio oficial señala que, durante la transición a SDK 58, el comando sin `--template` puede crear un proyecto SDK 57; para usar Expo Go en un móvil físico, comprueba el SDK que admite tu versión instalada de Expo Go. En clase trabajaremos fundamentalmente con el **emulador Android**.

> 📸 **[INSERTAR CAPTURA PROPIA — terminal con `npx expo start` y menú para pulsar `a`]**
>
> 📸 **[INSERTAR CAPTURA PROPIA — pantalla inicial de Expo ejecutándose en Android Studio]**

# 3. Estructura de carpetas de un proyecto Expo

La plantilla por defecto puede cambiar ligeramente según la versión. En proyectos actuales, las rutas pueden estar dentro de `src/app`. Una estructura inicial de Expo puede ser la siguiente; **la ampliaremos con el menú y las prácticas en el apartado 3.1**:

```text
MiPrimeraApp/
├── assets/                 # Imágenes, tipografías y otros recursos
├── src/
│   ├── app/                # Rutas / pantallas de Expo Router
│   │   ├── _layout.tsx     # Navegación y proveedores globales
│   │   ├── index.tsx       # Pantalla inicial (/)
│   │   ├── detalle.tsx     # Ruta /detalle
│   │   └── (tabs)/         # Grupo de rutas con pestañas, cuando se use
│   ├── components/         # Componentes reutilizables
│   ├── hooks/              # Hooks personalizados
│   ├── context/            # Contextos React
│   ├── services/           # Lógica y casos de uso
│   ├── api/                # Consultas HTTP
│   ├── db/                 # Acceso a SQLite
│   ├── constants/          # Constantes
│   └── theme/              # Estilos y colores compartidos
├── app.json                # Configuración de Expo
├── package.json            # Dependencias y scripts
└── tsconfig.json           # Configuración TypeScript
```

- **`src/app/index.tsx`** es siempre el menú principal del dossier, nunca una práctica que se reemplace.
- **`src/app/_layout.tsx`** define la navegación raíz y puede envolver las rutas con proveedores de contexto.
- Los archivos dentro de `src/app/` crean rutas: evita guardar ahí componentes auxiliares.
- **`src/components/`** reúne piezas reutilizables (por ejemplo, `BotonPersonalizado`).
- **`src/hooks/`** contiene la lógica compartida (por ejemplo, `useCalculadora`).
- **`src/db/`** contendrá funciones SQL, conexiones y tipos para persistencia local.

> 📸 **[INSERTAR CAPTURA PROPIA — explorador de VS Code con la estructura de carpetas del proyecto Expo]**


## 3.1. Regla de organización: una aplicación, todas las prácticas

El archivo **`src/app/index.tsx` será el menú del dossier**. No lo sustituiremos para realizar ejercicios. Cada práctica contará con su propia pantalla y una ruta de acceso. En las prácticas que usan navegación (Stack, Drawer, Tabs) y bases de datos agruparemos las pantallas relacionadas dentro de su propio directorio.

**Organización del proyecto que utilizaremos en clase** (se crean carpetas conforme sean necesarias; no hace falta crearlas todas el primer día):

```text
MiPrimeraApp/
├── assets/
│   └── imagenes/
├── src/
│   ├── app/
│   │   ├── _layout.tsx               # Stack raíz
│   │   ├── index.tsx                 # MENÚ GENERAL: NO SE REEMPLAZA
│   │   ├── practicas/
│   │   │   ├── p01.tsx ... p09.tsx
│   │   │   ├── p09-extra.tsx         # Práctica 9.1
│   │   │   └── p10.tsx ... p19.tsx
│   │   ├── stack/
│   │   │   ├── p20/ ... p23/         # Cada ejercicio con su _layout.tsx
│   │   ├── drawer/
│   │   │   ├── p24/ ... p26/         # Navegadores Drawer independientes
│   │   ├── tabs/
│   │   │   └── p27/                  # Navegador Tabs de práctica 27
│   │   ├── api/p28.tsx
│   │   ├── permisos/p29.tsx
│   │   ├── almacenamiento/p30.tsx
│   │   ├── almacenamiento/p31.tsx
│   │   └── datos/
│   │       ├── p32.tsx ... p33.tsx
│   │       ├── tienda/               # Prácticas 34–37 (Tabs propias)
│   │       └── viviendas/            # Prácticas 38–40 (Tabs propias)
│   ├── components/
│   │   └── practicas/               # Vistas y controles reutilizables
│   ├── context/                     # Contexto de tareas y otros estados
│   ├── hooks/                       # useCalculadora, etc.
│   ├── services/                    # lógica no visual
│   └── db/                          # SQLite: tablas y funciones
├── capturas/                        # Imágenes para el README de GitHub
├── README.md
├── app.json
├── package.json
└── tsconfig.json
```

> **Atención:** la carpeta `src/app` contiene **rutas**, no todos los componentes. Los archivos auxiliares se guardan fuera de ella. Una ruta como `src/app/practicas/p04.tsx` puede importar un componente desde `src/components/practicas/Practica04.tsx`.

### Preparación del menú (obligatoria antes de la práctica 1)

1. Crea un proyecto con la plantilla `default` de Expo que incluya Expo Router y TypeScript.
2. Si la plantilla incorpora pantallas de ejemplo o un grupo `(tabs)`, retira **solo las rutas de demostración de la plantilla**, antes de empezar a trabajar. **No borres nunca rutas de prácticas ya realizadas.**
3. Crea `src/app/practicas` y los archivos de la práctica 1.
4. Coloca este código en `src/app/_layout.tsx`:

```tsx
import { Stack } from 'expo-router';

export default function RootLayout() {
  return <Stack screenOptions={{ headerShown: true }} />;
}
```

Este layout raíz reconocerá las rutas que crees sin exigir que todas existan desde el primer día. Cuando configures otros navegadores, podrás ocultar la cabecera exterior en los tramos que lo necesiten, por ejemplo con una pantalla `Stack.Screen` para `drawer` o `tabs` y `headerShown: false`.

5. Crea `src/app/index.tsx` con el siguiente menú. Cada enlace aparecerá progresivamente cuando hayas creado la ruta correspondiente (de momento, **añade solo las prácticas que existan**):

```tsx
import { Link, type Href } from 'expo-router';
import { FlatList, StyleSheet, Text, View } from 'react-native';

const actividades: { nombre: string; ruta: Href }[] = [
  { nombre: 'Práctica 1 · Hola mundo', ruta: '/practicas/p01' },
  // Añade cada práctica cuando su pantalla exista:
  // { nombre: 'Práctica 2 · Contador', ruta: '/practicas/p02' },
  // { nombre: 'Práctica 9.1 · Imágenes aleatorias', ruta: '/practicas/p09-extra' },
  // { nombre: 'Práctica 20 · Stack', ruta: '/stack/p20' },
];

export default function MenuPrincipal() {
  return (
    <View style={styles.contenedor}>
      <Text style={styles.titulo}>Dossier React Native · Expo</Text>
      <FlatList
        data={actividades}
        keyExtractor={(item) => item.nombre}
        renderItem={({ item }) => (
          <Link href={item.ruta} style={styles.enlace}>
            {item.nombre}
          </Link>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  contenedor: { flex: 1, padding: 20 },
  titulo: { fontSize: 23, fontWeight: 'bold', marginBottom: 16 },
  enlace: { color: '#175b81', fontSize: 17, paddingVertical: 12 },
});
```

6. Ejemplo completo de una **ruta** para la práctica 1:

```tsx
// src/app/practicas/p01.tsx
import Practica01 from '../../components/practicas/Practica01';
export default function PantallaPractica01() {
  return <Practica01 />;
}
```

Cuando la práctica esté terminada, vuelve al menú desde la navegación del emulador y abre otra. Para ejercicios que utilizan varias pantallas, configura su propio `_layout.tsx` con `<Stack />`, `<Drawer />` o `<Tabs />`.

### Tabla de rutas de las prácticas

| Prácticas | Pantalla o entrada en el menú | Organización |
|---|---|---|
| 1–9 | `/practicas/p01` a `/practicas/p09` | Rutas independientes |
| 9.1 | `/practicas/p09-extra` | Pantalla propia, reutiliza imágenes de la 9 |
| 10–19 | `/practicas/p10` a `/practicas/p19` | Rutas independientes; calculadora compartida entre 10–11 |
| 20–23 | `/stack/p20` a `/stack/p23` | Un Stack independiente para cada práctica |
| 24–26 | `/drawer/p24` a `/drawer/p26` | Un Drawer independiente para cada práctica |
| 27 | `/tabs/p27` | Tabs y Stack anidado |
| 28 | `/api/p28` | Pantalla de API Pokémon |
| 29 | `/permisos/p29` | Pantalla de geolocalización |
| 30–31 | `/almacenamiento/p30`, `/almacenamiento/p31` | Historial y noticias |
| 32–33 | `/datos/p32`, `/datos/p33` | SQLite y persistencia de tareas |
| 34–37 | `/datos/tienda` | Tienda: se amplía en cada práctica |
| 38–40 | `/datos/viviendas` | Propietarios y casas: se amplía en cada práctica |

**Conservación de versiones:** en los ejercicios acumulativos (5–7, 10–11, 23/33, 24–26, 34–37 y 38–40) se permite compartir funciones, componentes o base de datos. No obstante, **cada práctica debe tener un punto de entrada identificable**. Cuando varias prácticas abran la misma aplicación progresiva, añade una selección o una pantalla de demostración de la funcionalidad correspondiente a cada una (por ejemplo, pantalla «Práctica 35 — categorías»). Las funciones posteriores no deben impedir demostrar la anterior.

> 📸 **[INSERTAR CAPTURA PROPIA — menú principal del dossier en el emulador, con enlaces a varias prácticas]**
>
> 📸 **[INSERTAR CAPTURA PROPIA — árbol de carpetas del proyecto único en VS Code]**


# 4. Primeros pasos

El siguiente ejemplo será el contenido de **`src/components/practicas/Practica01.tsx`**. Después se mostrará desde la ruta `src/app/practicas/p01.tsx`. **No sustituyas el menú `src/app/index.tsx`**:

```tsx
import { StyleSheet, Text, View } from 'react-native';

export default function Inicio() {
  return (
    <View style={styles.contenedor}>
      <Text style={styles.texto}>¡Hola mundo! Soy Ana.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  contenedor: {
    alignItems: 'center',
    backgroundColor: 'lightpink',
    flex: 1,
    justifyContent: 'center',
  },
  texto: { fontSize: 22 },
});
```

`flex: 1` hace que la vista ocupe el espacio disponible, y `justifyContent` / `alignItems` permiten alinear su contenido. Para nombres de propiedades se usa *camelCase* (`backgroundColor` en lugar de `background-color`).

### Práctica 1 — Expo

**Actividad:** Muestra «¡Hola mundo! Soy [tu nombre]» con `View` y `Text`, fondo rosa y contenido centrado. Ejecuta la pantalla en el emulador.

**Ubicación en el proyecto único:** `src/app/practicas/p01.tsx`.

**Cómo continuar sin perder trabajos anteriores:** La ruta debe importar `Practica01` desde `src/components/practicas/Practica01.tsx`; no cambies `src/app/index.tsx`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 6. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 5. Componentes básicos y maquetación

## 5.1. View, Text, Button y useState

`View` es el contenedor genérico, `Text` presenta texto y `Button` crea un botón básico con `title` y `onPress`. Para que la pantalla reaccione a los eventos, se utiliza `useState`.

```tsx
import { useState } from 'react';
import { Button, StyleSheet, Text, View } from 'react-native';

export default function Contador() {
  const [contador, setContador] = useState(0);
  return (
    <View style={styles.caja}>
      <Text>Contador: {contador}</Text>
      <Button title="Incrementar" onPress={() => setContador(c => c + 1)} />
    </View>
  );
}

const styles = StyleSheet.create({
  caja: { borderColor: 'black', borderWidth: 2, padding: 20 },
});
```

El componente `Button` tiene personalización limitada. Más adelante construiremos botones propios utilizando `Pressable`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 7. Referencia del contador; utilizar el código Expo anterior]**

### Práctica 2 — Expo

**Actividad:** Reproduce el contador con dos botones: incrementar y decrementar. Cambia el color de fondo, el borde y el radio de las esquinas del contenedor.

**Ubicación en el proyecto único:** `src/app/practicas/p02.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea una variante nueva del contador y conserva la práctica 1. Usa `useState` para el valor.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 8. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.2. Separar pantallas y componentes

Para organizar el contador, crea `src/components/practicas/ContadorBotones.tsx` y `src/components/practicas/InicioScreen.tsx`. La pantalla de la práctica 3 (`src/app/practicas/p03.tsx`) puede limitarse a importar y presentar ese componente:

```tsx
import InicioScreen from '../../components/practicas/InicioScreen';
export default function Practica03() { return <InicioScreen />; }
```

Los estilos son objetos de React Native, y pueden agruparse en `StyleSheet.create()`. El componente recibe propiedades (*props*) tipadas con TypeScript.

### Práctica 3 — Expo

**Actividad:** Extrae el contador a un componente `InicioScreen`, separa los estilos con `StyleSheet.create()` y muéstralo en su propia ruta.

**Ubicación en el proyecto único:** `src/app/practicas/p03.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Puedes reutilizar la lógica de la práctica 2, pero conserva la ruta `p02.tsx` para demostrar la versión anterior.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 9. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.3. Layout con Flexbox

React Native utiliza Flexbox de forma predeterminada: no hace falta indicar `display: flex`. A diferencia de CSS web, el valor inicial habitual de `flexDirection` es **`column`**. Con `flex: 1` y `flex: 2` los elementos reparten el espacio flexible en proporción 1:2 (según las restricciones del contenedor).

```tsx
import { StyleSheet, Text, View } from 'react-native';

export function Caja({ titulo, color }: { titulo: string; color: string }) {
  return <View style={[styles.caja, { backgroundColor: color }]}><Text>{titulo}</Text></View>;
}
const styles = StyleSheet.create({
  caja: { alignItems: 'center', height: 100, justifyContent: 'center', width: 100 },
});
```

En `style={[estiloBase, estiloAdicional]}`, el segundo objeto sobrescribe propiedades coincidentes. También existe el operador `...`, pero los arrays de estilos resultan más legibles.

### Práctica 4 — Expo

**Actividad:** Crea tres componentes `Caja` de 100 × 100, cada uno con un color recibido por props. Colócalos en un contenedor que ocupe el espacio disponible (`flex: 1`).

**Ubicación en el proyecto único:** `src/app/practicas/p04.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Utiliza `Caja.tsx` como componente reutilizable. Esta práctica debe mostrar el escenario sin los botones dinámicos de la práctica 5.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 10. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 5 — Expo

**Actividad:** Añade a cada caja los botones para aumentar y disminuir su propio valor `flex`. Muestra ese valor y comprueba cómo varía el espacio que ocupa cada caja.

**Ubicación en el proyecto único:** `src/app/practicas/p05.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Reutiliza `Caja.tsx` con props o una variante de caja dinámica sin alterar la pantalla de la práctica 4.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 11. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.4. Interacciones: Pressable y TouchableOpacity

Aunque `TouchableOpacity` y `TouchableHighlight` continúan existiendo, en los ejemplos nuevos se recomienda `Pressable` para crear botones personalizados y responder a `onPress`.

```tsx
import { Alert, Pressable, Text } from 'react-native';

export function BotonPersonalizado() {
  return (
    <Pressable
      accessibilityRole="button"
      onPress={() => Alert.alert('Acción', 'Botón pulsado')}
      style={({ pressed }) => ({
        backgroundColor: pressed ? '#8db7a1' : '#b9e1ce',
        borderRadius: 10, padding: 14,
      })}
    >
      <Text>¡Púlsame!</Text>
    </Pressable>
  );
}
```

El estilo puede cambiar mientras el usuario mantiene presionado el control. Si se pide concretamente comparar los touchables del dossier original, puedes hacerlo, pero no es obligatorio para desarrollar con Expo.

### Práctica 6 — Expo

**Actividad:** Sustituye los botones de la práctica anterior por botones personalizados; el de aumentar `flex` debe mostrar una alerta y el de disminuir debe ofrecer una respuesta visual al tocarlo.

**Ubicación en el proyecto único:** `src/app/practicas/p06.tsx`.

**Cómo continuar sin perder trabajos anteriores:** La pantalla de la práctica 5 debe seguir funcionando. En la 6 personaliza los controles con `Pressable` o con los `Touchable` del original.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 12. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 7 — Expo

**Actividad:** Haz que `Caja` reciba un nombre y un estilo personalizado mediante props. Combina el estilo predeterminado y el recibido; una de las cajas tendrá bordes redondeados.

**Ubicación en el proyecto único:** `src/app/practicas/p07.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Amplía la caja con `style?: StyleProp<ViewStyle>` y compón estilos como `[styles.base, style]`. Si actualizas un componente compartido, verifica las prácticas 4–6.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 13. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.5. flexWrap y flexDirection

`flexDirection` acepta `row`, `column` y sus variantes `-reverse`; `flexWrap` admite `wrap` y `nowrap`. Con `wrap`, los elementos que no caben pasan a otra fila o columna. `gap`, `rowGap` y `columnGap` controlan las separaciones.

### Práctica 8 — Expo

**Actividad:** Parte de una pantalla sin círculos. Añade círculos con un botón; varía sus colores RGB según el identificador y permite alternar `row/column` y `wrap/nowrap`.

**Ubicación en el proyecto único:** `src/app/practicas/p08.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Conserva el escenario de cajas: esta práctica es una pantalla nueva con círculos.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 14. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.6. TextInput, Image, justifyContent y alignItems

- `justifyContent` distribuye los elementos en el eje principal.
- `alignItems` los alinea en el eje transversal.
- `TextInput` utiliza `value` y `onChangeText` para controlar lo escrito.
- `Image` permite `source={{ uri: 'https://...' }}` para recursos remotos y `source={require('../../assets/imagen.png')}` para recursos locales con ruta literal.

```tsx
import { Image, TextInput, View } from 'react-native';
import { useState } from 'react';

export default function EjemploEntrada() {
  const [texto, setTexto] = useState('');
  return (
    <View style={{ flex: 1, padding: 20, justifyContent: 'center' }}>
      <TextInput
        value={texto}
        onChangeText={setTexto}
        placeholder="Escribe algo"
        style={{ borderWidth: 1, padding: 12 }}
      />
      <Image
        source={{ uri: 'https://reactnative.dev/img/tiny_logo.png' }}
        style={{ height: 80, marginTop: 20, width: 80 }}
      />
    </View>
  );
}
```

No se pueden construir libremente las rutas dentro de `require()` con concatenación. Si quieres elegir entre imágenes locales, define antes un mapa con llamadas `require()` literales. Para márgenes seguros y recortes de pantalla, utiliza `SafeAreaView` importado de `react-native-safe-area-context` dentro de los componentes que lo necesiten.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 15. Ejemplo de alineación de elementos]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 16. Imágenes: adaptar rutas o usar imágenes propias]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 17. Ejemplo de recursos locales con `require()`]**

### Práctica 9 — Expo

**Actividad:** Reproduce una pantalla con imágenes, un `TextInput` que cambie `justifyContent` y botones que modifiquen `alignItems`. Al menos una imagen debe ser local y cargarse mediante `require()`.

**Ubicación en el proyecto único:** `src/app/practicas/p09.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Las imágenes remotas necesitan una URI accesible; los recursos locales van en `assets/imagenes/` y usan `require()` literal.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 17. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

Para alternar imágenes locales, crea un diccionario de recursos:

```tsx
const imagenes = [
  require('../../assets/estrella.png'),
  require('../../assets/gato.png'),
];
// Al pulsar, guarda en un estado el índice seleccionado.
```

**Importante:** coloca primero los archivos de imagen con esos nombres; son recursos de ejemplo, no incluidos en esta entrega.

### Práctica 9.1 — Expo

**Actividad:** Carga varias imágenes locales predefinidas y cambia aleatoriamente la imagen mostrada al pulsar un botón. Usa un array con los recursos importados estáticamente.

**Ubicación en el proyecto único:** `src/app/practicas/p09-extra.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Es una práctica independiente de la 9: ambas deben mantenerse accesibles.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 18. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.7. Pantallas completas: calculadora

En una calculadora, los botones pueden agruparse por filas mediante contenedores con `flexDirection: 'row'`. Emplea componentes reutilizables, `Pressable` y `Text` con `numberOfLines={1}` y `adjustsFontSizeToFit` si necesitas ajustar el resultado al ancho disponible.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 19. Referencia visual de la calculadora]**

### Práctica 10 — Expo

**Actividad:** Construye la calculadora visual del dossier mediante filas de botones personalizables; implementa las operaciones y muestra el resultado en pantalla.

**Ubicación en el proyecto único:** `src/app/practicas/p10.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Guarda la interfaz y las operaciones en un componente reutilizable `Calculadora`; la ruta p10 debe abrir la versión de esta práctica.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 19. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 5.8. Hooks personalizados

Un hook agrupa lógica que puede reutilizarse sin mezclarla con la interfaz:

```tsx
// src/hooks/useCalculadora.ts
import { useState } from 'react';
export function useCalculadora() {
  const [pantalla, setPantalla] = useState('0');
  const agregar = (digito: string) => setPantalla(actual => actual === '0' ? digito : actual + digito);
  const limpiar = () => setPantalla('0');
  return { pantalla, agregar, limpiar };
}
```

El hook mostrado gestiona entradas y limpieza; la lógica de operaciones puede añadirse como parte de la práctica.

### Práctica 11 — Expo

**Actividad:** Extrae la lógica de la calculadora anterior a un hook personalizado y deja el componente de pantalla centrado en la interfaz.

**Ubicación en el proyecto único:** `src/app/practicas/p11.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea `src/hooks/useCalculadora.ts`; la ruta p11 debe utilizar el hook. Conserva p10 como versión anterior.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 20. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 6. Formularios, Switch y Alert

## 6.1. Alert y mensajes con entrada de texto

`Alert.alert()` permite mostrar mensajes y botones. En Android **no hay un `Alert.prompt` nativo equivalente al de iOS**; para pedir texto, construiremos un `Modal` con un `TextInput`.

```tsx
import { Alert, Button } from 'react-native';
export function Confirmacion() {
  const confirmar = () => Alert.alert('Cambiar color', '¿Aplicar el color seleccionado?', [
    { text: 'Cancelar', style: 'cancel' },
    { text: 'Aceptar', onPress: () => console.log('Aceptado') },
  ]);
  return <Button title="Cambiar color" onPress={confirmar} />;
}
```

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 21. Referencia de confirmación con Alert]**

### Práctica 12 — Expo

**Actividad:** Crea un botón «Cambiar de color» que proponga un color aleatorio. Antes de cambiar el fondo, muestra un diálogo de confirmación con aceptar y cancelar.

**Ubicación en el proyecto único:** `src/app/practicas/p12.tsx`.

**Cómo continuar sin perder trabajos anteriores:** `Alert.alert` sirve para confirmar el cambio en Android; no necesita librerías adicionales.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 21. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 13 — Expo

**Actividad:** Adapta el ejercicio del color para que el usuario introduzca un color. Usa un modal propio con `TextInput` y botones de aceptar/cancelar para que funcione en Android e iOS.

**Ubicación en el proyecto único:** `src/app/practicas/p13.tsx`.

**Cómo continuar sin perder trabajos anteriores:** No uses `Alert.prompt` para Android: crea un `Modal` de React Native con `TextInput`. No necesitas `react-native-prompt-android`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 22. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 6.2. Switch y formularios controlados

`Switch` se controla con `value` y `onValueChange`, mientras que los campos de texto utilizan `value` y `onChangeText`.

```tsx
import { useState } from 'react';
import { Switch, Text, TextInput, View } from 'react-native';

type Persona = { nombre: string; edad: string; jubilado: boolean; casado: boolean };
export default function FormularioPersona() {
  const [persona, setPersona] = useState<Persona>({ nombre: '', edad: '', jubilado: false, casado: false });
  return (
    <View style={{ gap: 12, padding: 20 }}>
      <TextInput placeholder="Nombre" value={persona.nombre}
        onChangeText={nombre => setPersona(p => ({ ...p, nombre }))} />
      <TextInput placeholder="Edad" keyboardType="numeric" value={persona.edad}
        onChangeText={edad => setPersona(p => ({ ...p, edad }))} />
      <Text>Jubilado</Text>
      <Switch value={persona.jubilado}
        onValueChange={jubilado => setPersona(p => ({ ...p, jubilado }))} />
      <Text>Casado</Text>
      <Switch value={persona.casado}
        onValueChange={casado => setPersona(p => ({ ...p, casado }))} />
    </View>
  );
}
```

Se mantiene la edad como cadena mientras se escribe, porque `TextInput` trabaja con texto. Valídala y conviértela a número cuando corresponda.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 22. Control Switch]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 23. Formulario de persona]**

### Práctica 14 — Expo

**Actividad:** Incluye un `Switch` que active o desactive un borde rojo alrededor de la pantalla. Su control deslizante debe ser marrón si está desactivado y naranja si está activado.

**Ubicación en el proyecto único:** `src/app/practicas/p14.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea una pantalla independiente del formulario de la práctica 15.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 22. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 15 — Expo

**Actividad:** Completa el formulario de persona (nombre, edad, casado/a y jubilado/a), incluyendo el `Switch` del estado «casado». Mantén los valores en un objeto de estado tipado.

**Ubicación en el proyecto único:** `src/app/practicas/p15.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Usa `TextInput` para nombre/edad y `Switch` para casado y jubilado.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 24. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 7. Listas

`FlatList` renderiza listas largas de forma eficiente. Recibe `data`, una función `renderItem` y `keyExtractor`; admite encabezados y separadores opcionales. Para poco contenido, `ScrollView` también es válido, pero renderiza todos sus hijos.

```tsx
import { FlatList, Text, View } from 'react-native';
const productos = [
  { id: '1', nombre: 'Manzana', precio: 1.5, stock: 12 },
  { id: '2', nombre: 'Pan', precio: 1.2, stock: 5 },
];
export default function ListaProductos() {
  return <FlatList
    data={productos}
    keyExtractor={item => item.id}
    ListHeaderComponent={<Text style={{ fontSize: 24 }}>Lista de productos</Text>}
    renderItem={({ item, index }) => (
      <View style={{ backgroundColor: index % 2 ? '#eee' : '#d8f2df', padding: 12 }}>
        <Text>{item.nombre} · {item.precio.toFixed(2)} € · Stock: {item.stock}</Text>
      </View>
    )}
  />;
}
```

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 25. Lista con FlatList]**

### Práctica 16 — Expo

**Actividad:** Sustituye el listado de personas por productos con nombre, precio y stock. Usa `FlatList`, sin separadores, con filas pares e impares de colores distintos y cabecera «Lista Productos».

**Ubicación en el proyecto único:** `src/app/practicas/p16.tsx`.

**Cómo continuar sin perder trabajos anteriores:** El listado no debe depender de componentes del formulario salvo que los reutilices conscientemente.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 26. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 17 — Expo

**Actividad:** Construye una lista de tarjetas `CardPersona` con id, nombre, altura y peso para calcular IMC. Permite añadir tarjetas nuevas y eliminar cualquiera desde el componente padre.

**Ubicación en el proyecto único:** `src/app/practicas/p17.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Gestiona la colección de tarjetas en la pantalla padre; identifica cada tarjeta mediante un id estable.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 26. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 8. Iconos

La plantilla Expo permite usar bibliotecas de iconos compatibles. Para los ejemplos de clase utilizaremos **`@expo/vector-icons`**; no se deben modificar los archivos Gradle ni añadir manualmente fuentes a Android.

```bash
npx expo install @expo/vector-icons
```

```tsx
import { Ionicons } from '@expo/vector-icons';
import { View } from 'react-native';
export function Iconos() {
  return <View style={{ padding: 20 }}><Ionicons name="car-outline" size={42} color="blue" /></View>;
}
```

Consulta los nombres válidos en el catálogo de iconos vinculado desde la documentación de Expo.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 28. Referencia visual del catálogo de iconos, no copiar la instalación Gradle]**

### Práctica 18 — Expo

**Actividad:** Crea un diario: al guardar una entrada muestra el texto, la fecha y hora (con segundos) y un icono de estado de ánimo según los emoticonos `:-)` o `:-(`.

**Ubicación en el proyecto único:** `src/app/practicas/p18.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Puedes usar `Ionicons` de `@expo/vector-icons`, sin tocar Gradle.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 29. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 19 — Expo

**Actividad:** Crea un editor de texto con varios iconos de emociones. Al pulsar un icono, añade al final del `TextInput` su emoticono de texto correspondiente.

**Ubicación en el proyecto único:** `src/app/practicas/p19.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Mantén el diario de la práctica 18; esta segunda pantalla manipula el texto con iconos.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 29. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 9. Navegación con Expo Router

Expo Router utiliza **rutas basadas en archivos**: cada archivo de `src/app` se convierte en una pantalla. No hace falta crear `NavigationContainer` a mano. Los layouts `_layout.tsx` determinan si una zona de la app usa Stack, pestañas o menú lateral.

## 9.1. Stack Navigation

Un **Stack** permite navegar entre pantallas y volver a la anterior. Para conservar el menú general, lo crearemos **dentro de la práctica 20**, no en la raíz.

```text
src/app/stack/p20/
  _layout.tsx
  index.tsx
  detalle.tsx
```

```tsx
// src/app/stack/p20/_layout.tsx
import { Stack } from 'expo-router';
export default function StackPractica20() {
  return <Stack />;
}
```

```tsx
// src/app/stack/p20/index.tsx
import { useRouter } from 'expo-router';
import { Button, Text, View } from 'react-native';
export default function InicioPractica20() {
  const router = useRouter();
  return (
    <View style={{ flex: 1, justifyContent: 'center', padding: 20 }}>
      <Text>Primera pantalla</Text>
      <Button title="Ir al detalle" onPress={() => router.push('/stack/p20/detalle')} />
    </View>
  );
}
```

```tsx
// src/app/stack/p20/detalle.tsx
import { useRouter } from 'expo-router';
import { Button, Text, View } from 'react-native';
export default function DetallePractica20() {
  const router = useRouter();
  return (
    <View>
      <Text>Segunda pantalla</Text>
      <Button title="Volver" onPress={() => router.back()} />
    </View>
  );
}
```

`router.push()` abre otra ruta, `router.replace()` sustituye la actual y `router.back()` vuelve a la anterior. El menú principal seguirá accesible cuando regreses del ejercicio.

### Práctica 20 — Expo

**Actividad:** Crea una navegación Stack con pantalla de inicio y otra pantalla a la que se acceda mediante un botón; comprueba la navegación hacia delante y hacia atrás.

**Ubicación en el proyecto único:** `src/app/stack/p20/index.tsx` (+ `_layout.tsx` y rutas adicionales si hacen falta).

**Cómo continuar sin perder trabajos anteriores:** Crea `src/app/stack/p20/_layout.tsx` con `<Stack />` y al menos dos rutas (`index.tsx`, `detalle.tsx`). El menú principal enlaza a `/stack/p20`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 33. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 9.2. Enviar parámetros entre pantallas

En Expo Router, enviamos parámetros por la ruta y los recuperamos con `useLocalSearchParams`:

```tsx
// En la primera pantalla
router.push({ pathname: '/stack/p21/tercera', params: { userName: nombre } });
```

```tsx
// src/app/stack/p21/tercera.tsx
import { useLocalSearchParams } from 'expo-router';
import { Text, View } from 'react-native';
export default function Tercera() {
  const { userName } = useLocalSearchParams<{ userName?: string }>();
  return <View><Text>Saludos {userName ?? 'invitado'}. ¡Bienvenido!</Text></View>;
}
```

En el dossier original, esta tarea usaba `navigation.navigate` y `route.params`. En Expo Router la resolución de rutas está asociada a la estructura de archivos.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 34. Pasar parámetros: reemplazar por `router.push`]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 35. Recibir parámetros: reemplazar por `useLocalSearchParams`]**

### Práctica 21 — Expo

**Actividad:** Crea un Stack con tres pantallas. Desde la primera envía el nombre escrito en un `TextInput` a la tercera y muestra «Saludos [nombre]. ¡Bienvenido!».

**Ubicación en el proyecto único:** `src/app/stack/p21/index.tsx` (+ `_layout.tsx` y rutas adicionales si hacen falta).

**Cómo continuar sin perder trabajos anteriores:** Crea un Stack separado (`/stack/p21`), con `index.tsx`, `segunda.tsx` y `tercera.tsx`. Envía params con `router.push` y recíbelos mediante `useLocalSearchParams`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 35. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 9.3. Opciones de cabecera

Las opciones se aplican a una pantalla desde `_layout.tsx`:

```tsx
<Stack.Screen name="segunda" options={{
  title: 'Mi pantalla',
  headerStyle: { backgroundColor: '#345b72' },
  headerTintColor: '#ffffff',
  headerTitleAlign: 'center',
  headerBackVisible: false,
}} />
```

`headerShown` oculta o muestra la cabecera; `headerTintColor` afecta a elementos de la cabecera. Consulta la documentación para conocer qué opciones admite cada tipo de Stack y plataforma.

### Práctica 22 — Expo

**Actividad:** Personaliza un Stack de dos pantallas: color de cabecera, título blanco centrado, nombre personalizado y ocultación del botón de retroceso del encabezado.

**Ubicación en el proyecto único:** `src/app/stack/p22/index.tsx` (+ `_layout.tsx` y rutas adicionales si hacen falta).

**Cómo continuar sin perder trabajos anteriores:** Crea `/stack/p22`, separado de p20 y p21, y configura opciones del `<Stack.Screen />` en su `_layout.tsx`.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 37. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 9.4. Context API y navegación

Si varias pantallas de una misma práctica necesitan compartir tareas o preferencias, define un contexto y sitúa su proveedor en el `_layout.tsx` **de esa práctica**, envolviendo su `<Stack />`:

```tsx
import { Stack } from 'expo-router';
import { TareasProvider } from '../../../context/TareasContext';
export default function RootLayout() {
  return <TareasProvider><Stack /></TareasProvider>;
}
```

El archivo `TareasContext.tsx` debe implementar y exportar `TareasProvider`. El ejemplo muestra cómo integrarlo, no incluye la definición del contexto. Para la práctica 23, sitúa el proveedor en `src/app/stack/p23/_layout.tsx`, no en el layout raíz, para evitar afectar al resto de prácticas.

### Práctica 23 — Expo

**Actividad:** Desarrolla una lista de tareas con pantalla de listado y pantalla de edición/alta. Permite completar, editar y borrar tareas; gestiona su estado mediante Context API.

**Ubicación en el proyecto único:** `src/app/stack/p23/index.tsx` (+ `_layout.tsx` y rutas adicionales si hacen falta).

**Cómo continuar sin perder trabajos anteriores:** Usa `/stack/p23` con rutas `index.tsx` y `editar.tsx` (o `[id].tsx`). Crea `src/context/TareasContext.tsx` y envuelve **solo este Stack** en su proveedor.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 38. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 9.5. Drawer Navigation

El Drawer presenta un menú lateral. Para usarlo con Expo Router, instala las dependencias compatibles:

```bash
npx expo install @react-navigation/drawer react-native-reanimated react-native-worklets react-native-gesture-handler
```

En este dossier utiliza `src/app/drawer/p24/` con su propio `_layout.tsx`, `index.tsx` y `acerca.tsx`; conserva el menú en `src/app/index.tsx`.

```tsx
// src/app/drawer/p24/_layout.tsx
import { Drawer } from 'expo-router/drawer';
export default function DrawerLayout() {
  return <Drawer>
    <Drawer.Screen name="index" options={{ drawerLabel: 'Inicio', title: 'Inicio' }} />
    <Drawer.Screen name="acerca" options={{ drawerLabel: 'Acerca de', title: 'Acerca de' }} />
  </Drawer>;
}
```

En el layout raíz, puedes montar la sección `drawer` dentro del Stack con `headerShown: false` para evitar dos cabeceras. **La disponibilidad de Drawer y su configuración dependen del SDK y de las dependencias de navegación instaladas**.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 40. Referencia visual del menú lateral]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 41. Comparación de pantallas Drawer]**

### Práctica 24 — Expo

**Actividad:** Crea un Drawer con una opción «Stack» y otra «Acerca de». Dentro del Stack muestra enlaces a actividades anteriores, incluida la calculadora de la práctica 10.

**Ubicación en el proyecto único:** `src/app/drawer/p24/index.tsx` (+ `_layout.tsx` y rutas adicionales).

**Cómo continuar sin perder trabajos anteriores:** Crea `/drawer/p24` y su `_layout.tsx` con `<Drawer />`; la sección Stack puede tener un layout anidado. Enlaza a la calculadora existente, no la dupliques.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 41. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 25 — Expo

**Actividad:** Personaliza el Drawer de la práctica anterior: colores, ancho de 200 puntos y contenido personalizado. Comprueba su comportamiento con la pantalla en vertical y horizontal.

**Ubicación en el proyecto único:** `src/app/drawer/p25/index.tsx` (+ `_layout.tsx` y rutas adicionales).

**Cómo continuar sin perder trabajos anteriores:** Puedes compartir la implementación del Drawer 24, pero en `/drawer/p25` muestra una variante personalizada para poder comparar resultados.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 42. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 26 — Expo

**Actividad:** Desarrolla la aplicación de mascotas que se muestra como referencia en el dossier original, conservando sus pantallas y navegación lateral. Documenta las funcionalidades que implementes.

**Ubicación en el proyecto único:** `src/app/drawer/p26/index.tsx` (+ `_layout.tsx` y rutas adicionales).

**Cómo continuar sin perder trabajos anteriores:** Reserva `/drawer/p26` para la app de mascotas. La referencia visual de la página 44 determina su composición; no alteres las otras Drawer.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 44. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 9.6. Tab Navigation

Las pestañas permiten cambiar entre secciones. En Expo Router se definen con un grupo de rutas y un layout:

```text
src/app/tabs/p27/
  _layout.tsx
  index.tsx
  mascotas.tsx
```

```tsx
// src/app/tabs/p27/_layout.tsx
import { Tabs } from 'expo-router';
import { Ionicons } from '@expo/vector-icons';
export default function TabsLayout() {
  return <Tabs>
    <Tabs.Screen name="index" options={{ title: 'Inicio',
      tabBarIcon: ({ color, size }) => <Ionicons name="home-outline" color={color} size={size} /> }} />
    <Tabs.Screen name="mascotas" options={{ title: 'Mascotas',
      tabBarIcon: ({ color, size }) => <Ionicons name="paw-outline" color={color} size={size} /> }} />
  </Tabs>;
}
```

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 45. Referencia visual de Tabs]**

### Práctica 27 — Expo

**Actividad:** Crea una aplicación con dos pestañas e iconos. En la pestaña de operaciones matemáticas incorpora un Stack con enlaces a ejercicios anteriores, incluida la calculadora.

**Ubicación en el proyecto único:** `src/app/tabs/p27/index.tsx` (+ `_layout.tsx` y pestañas).

**Cómo continuar sin perder trabajos anteriores:** Crea `/tabs/p27/_layout.tsx` con `<Tabs />`, y dentro de la pestaña de operaciones crea un Stack anidado si procede. Enlaza a ejercicios anteriores.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 45. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 10. Consumo de APIs

React Native permite utilizar `fetch()` y `async/await` como en las aplicaciones web. Controla carga, errores y tipos de los datos. No expongas claves privadas de APIs en el código de la app.

```tsx
import { useEffect, useState } from 'react';
import { ActivityIndicator, FlatList, Text, View } from 'react-native';

type Pokemon = { name: string; url: string };
export default function PokemonScreen() {
  const [datos, setDatos] = useState<Pokemon[]>([]);
  const [cargando, setCargando] = useState(true);
  const [error, setError] = useState('');
  useEffect(() => {
    let activo = true;
    async function cargar() {
      try {
        const res = await fetch('https://pokeapi.co/api/v2/pokemon?limit=20');
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const json: { results: Pokemon[] } = await res.json();
        if (activo) setDatos(json.results);
      } catch (e) {
        if (activo) setError(e instanceof Error ? e.message : 'Error de red');
      } finally {
        if (activo) setCargando(false);
      }
    }
    cargar();
    return () => { activo = false; };
  }, []);
  if (cargando) return <ActivityIndicator />;
  if (error) return <Text>{error}</Text>;
  return <View style={{ flex: 1 }}><FlatList data={datos} keyExtractor={p => p.name}
    renderItem={({ item }) => <Text style={{ padding: 12 }}>{item.name}</Text>} /></View>;
}
```

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 47. Resultado visual de consulta a API Pokémon]**

### Práctica 28 — Expo

**Actividad:** Desarrolla la aplicación de Pokémon de la sección de APIs, realizando peticiones con `fetch`, gestionando carga y errores, y presentando la información obtenida.

**Ubicación en el proyecto único:** `src/app/api/p28.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea una pantalla propia para Pokémon. No necesita sustituir el menú ni la sección de navegación.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 47. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 11. Permisos y geolocalización

En Expo se usan los módulos `expo-*` para solicitar permisos y acceder a prestaciones del dispositivo. Para geolocalización:

```bash
npx expo install expo-location
```

```tsx
import * as Location from 'expo-location';
import { useState } from 'react';
import { Button, Text, View } from 'react-native';
export default function Ubicacion() {
  const [resultado, setResultado] = useState('Sin consultar');
  const consultar = async () => {
    const permiso = await Location.requestForegroundPermissionsAsync();
    if (permiso.status !== 'granted') { setResultado('Permiso denegado'); return; }
    try {
      const posicion = await Location.getCurrentPositionAsync({});
      setResultado(JSON.stringify(posicion.coords, null, 2));
    } catch { setResultado('No se pudo obtener la ubicación'); }
  };
  return <View style={{ padding: 20 }}>
    <Button title="Consultar ubicación" onPress={consultar} />
    <Text>{resultado}</Text>
  </View>;
}
```

En el emulador de Android Studio se pueden simular coordenadas desde los controles extendidos del dispositivo virtual (Location). Solicita solo los permisos que necesite la app y explica al usuario para qué se utilizan. Los permisos en segundo plano y determinadas configuraciones nativas pueden requerir un **development build**.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 48. Resultado de geolocalización]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 49. Permiso concedido / denegado]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 50. Información de ubicación]**
>
> 📸 **[INSERTAR CAPTURA PROPIA — simulación de ubicación en controles del emulador Android Studio]**

### Práctica 29 — Expo

**Actividad:** Solicita permiso de ubicación y muestra latitud, longitud y otros datos en pantalla con `JSON.stringify`. Comprueba y documenta los casos con permiso concedido y denegado.

**Ubicación en el proyecto único:** `src/app/permisos/p29.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea una ruta independiente y solicita los permisos desde `expo-location`. Muestra el resultado en la interfaz.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 50. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 12. Almacenamiento local y WebView

## 12.1. AsyncStorage

Guarda datos sencillos en pares clave-valor de forma asíncrona. No es una base de datos relacional ni es adecuado para guardar contraseñas o secretos.

```bash
npx expo install @react-native-async-storage/async-storage
```

```tsx
import AsyncStorage from '@react-native-async-storage/async-storage';

type Registro = { fecha: string; latitud: number; longitud: number };
export async function guardarHistorial(historial: Registro[]) {
  await AsyncStorage.setItem('historial', JSON.stringify(historial));
}
export async function leerHistorial(): Promise<Registro[]> {
  const dato = await AsyncStorage.getItem('historial');
  return dato ? JSON.parse(dato) as Registro[] : [];
}
```

Convierte los objetos a JSON al guardar y usa `JSON.parse()` al recuperarlos, validando su contenido cuando provenga de fuentes no confiables.

### Práctica 30 — Expo

**Actividad:** Amplía la geolocalización para guardar latitud, longitud y fecha/hora en AsyncStorage. Muestra un historial y comprueba que se conserva después de cerrar y reabrir la aplicación.

**Ubicación en el proyecto único:** `src/app/almacenamiento/p30.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Reutiliza la lógica de ubicación mediante un servicio o hook compartido con p29. Guarda registros en AsyncStorage usando una clave propia.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 51. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 12.2. WebView

Para presentar una página web dentro de una pantalla de la app:

```bash
npx expo install react-native-webview
```

```tsx
import { WebView } from 'react-native-webview';
export default function Pagina() {
  return <WebView source={{ uri: 'https://reactnative.dev/' }} style={{ flex: 1 }} />;
}
```

Evita cargar contenido no confiable y presta atención a enlaces externos y dominios permitidos. Las vistas web **no reemplazan** el uso de componentes nativos cuando se busca una interfaz móvil.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 52. Ejemplo de WebView / persistencia]**

### Práctica 31 — Expo

**Actividad:** Desarrolla el lector de noticias del ejemplo, incorporando `WebView` y el almacenamiento necesario. Comprueba qué contenido sigue disponible cuando activas el modo avión y vuelves a abrir la aplicación.

**Ubicación en el proyecto único:** `src/app/almacenamiento/p31.tsx`.

**Cómo continuar sin perder trabajos anteriores:** La pantalla de noticias debe ser independiente de p30. `WebView` solo carga contenido sin conexión si se ha preparado almacenamiento o caché apropiados; documenta las pruebas.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 52. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

# 13. Bases de datos: SQLite en Expo

El dossier original utiliza **SQLite + TypeORM** para representar entidades, repositorios y relaciones. Con Expo trabajaremos inicialmente con la biblioteca oficial **`expo-sqlite`**, que permite ejecutar SQL directamente en una base de datos local. Para conservar los objetivos didácticos, traduciremos los conceptos del ORM (`@Entity`, `@Column`, `find`, relaciones y transacciones) a **tablas, consultas y claves foráneas SQL**. No instalaremos TypeORM de forma automática: su integración depende de versión, driver y configuración nativa, y no es necesaria para resolver estas prácticas.

```bash
npx expo install expo-sqlite
```

## 13.1. Crear tablas y operar con datos

```tsx
// src/db/database.ts
import * as SQLite from 'expo-sqlite';

export type Persona = { id: number; nombre: string; edad: number };
const dbPromesa = SQLite.openDatabaseAsync('clase.db');

export async function prepararBD() {
  const db = await dbPromesa;
  await db.execAsync(`
    PRAGMA foreign_keys = ON;
    CREATE TABLE IF NOT EXISTS personas (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      nombre TEXT NOT NULL,
      edad INTEGER NOT NULL
    );
  `);
}

export async function crearPersona(nombre: string, edad: number) {
  const db = await dbPromesa;
  return db.runAsync(
    'INSERT INTO personas (nombre, edad) VALUES (?, ?)',
    nombre, edad
  );
}

export async function listarPersonas(): Promise<Persona[]> {
  const db = await dbPromesa;
  return db.getAllAsync<Persona>('SELECT id, nombre, edad FROM personas ORDER BY id DESC');
}
```

Llama a `prepararBD()` al iniciar la sección de base de datos y espera a que termine antes de insertar o consultar. Usa **parámetros** (`?`) en consultas que reciben datos del usuario; no los concatenes en SQL. Para operaciones agrupadas de creación de esquema, `execAsync()` resulta cómodo, pero no escapa parámetros.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 55. Modelo de entidades / resultado; sustituir código de TypeORM por SQL]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 56. Ejemplo de consultas y guardado de datos]**

### Práctica 32 — Expo

**Actividad:** Crea una pantalla SQLite para guardar personas. Añade `TextInput` para nombre y edad, botones de inserción y visualización de registros, y confirma la persistencia.

**Ubicación en el proyecto único:** `src/app/datos/p32.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Crea `src/db/personas.ts` para la tabla y consultas; no modifiques las tablas de tareas, productos o viviendas.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 56. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 33 — Expo

**Actividad:** Vuelve a la lista de tareas de la práctica 23 y persístela en SQLite. Comprueba que se conservan altas, cambios y eliminaciones después de cerrar la aplicación.

**Ubicación en el proyecto único:** `src/app/datos/p33.tsx`.

**Cómo continuar sin perder trabajos anteriores:** Reutiliza o adapta el contexto de la práctica 23, pero añade una capa `src/db/tareas.ts` para CRUD persistente. Mantén ambas rutas accesibles.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 57. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 13.2. Consultas, filtros y correspondencias con TypeORM

| Concepto del dossier TypeORM | Equivalente SQL con `expo-sqlite` |
|---|---|
| `@Entity('productos')` | `CREATE TABLE productos (...)` |
| `@PrimaryGeneratedColumn()` | `id INTEGER PRIMARY KEY AUTOINCREMENT` |
| `@Column({ nullable: true })` | columna que permite `NULL` |
| `@Column({ unique: true })` | restricción `UNIQUE` |
| `repository.save()` | `INSERT` o `UPDATE` parametrizados |
| `repository.find()` | `SELECT ... FROM ...` |
| `findOneBy({ id })` | `SELECT ... WHERE id = ? LIMIT 1` |
| `relations` | consultas con `JOIN` o consultas relacionadas |
| `dataSource.transaction()` | `withTransactionAsync()` o `withExclusiveTransactionAsync()` |

```tsx
const db = await SQLite.openDatabaseAsync('clase.db');
const productos = await db.getAllAsync<{ nombre: string; precio: number }>(
  'SELECT nombre, precio FROM productos WHERE precio >= ? ORDER BY nombre ASC',
  10
);
```

## 13.3. Productos y categorías (relación 1:N)

Una categoría puede tener muchos productos, mientras que cada producto pertenece a una categoría. Se modela con una clave foránea en `productos`.

```sql
CREATE TABLE IF NOT EXISTS categorias (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  nombre TEXT NOT NULL UNIQUE
);
CREATE TABLE IF NOT EXISTS productos (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  nombre TEXT NOT NULL,
  precio REAL NOT NULL,
  stock INTEGER NOT NULL DEFAULT 0,
  descontinuado INTEGER NOT NULL DEFAULT 0,
  categoria_id INTEGER,
  FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);
```

SQLite representa habitualmente los valores booleanos con `0` y `1`. La relación se consulta con `JOIN`:

```sql
SELECT p.nombre AS producto, c.nombre AS categoria
FROM productos p
LEFT JOIN categorias c ON c.id = p.categoria_id;
```

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 58. Filtros con TypeORM: pasar a SQL]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 60. Diagrama de relación 1:N]**

### Práctica 34 — Expo

**Actividad:** Inicia una tienda de alimentación con pestañas para registrar y consultar productos (nombre, precio, stock y campo «descatalogado»). Utiliza SQLite.

**Ubicación en el proyecto único:** `src/app/datos/tienda/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** Inicia las rutas de `/datos/tienda` y conserva vistas reconocibles para productos y su listado.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 59. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 35 — Expo

**Actividad:** Amplía la tienda creando categorías y una pestaña para registrarlas. Define la relación entre productos y categorías mediante claves externas en SQLite.

**Ubicación en el proyecto único:** `src/app/datos/tienda/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** Añade la tabla de categorías y una vista específica para esta práctica. No borres la información existente durante la migración sin avisar.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 61. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 36 — Expo

**Actividad:** Amplía el formulario de productos para asignarles una categoría existente. Valida la categoría y guarda correctamente la relación.

**Ubicación en el proyecto único:** `src/app/datos/tienda/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** La vista de productos debe ofrecer selección de categoría; utiliza su identificador para la clave externa.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 61. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 37 — Expo

**Actividad:** Añade una pestaña con un `FlatList` de categorías y visualiza para cada una sus productos asociados (por ejemplo, con `JSON.stringify`).

**Ubicación en el proyecto único:** `src/app/datos/tienda/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** La vista de categorías con sus productos debe poder abrirse desde el menú de la tienda.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 62. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 13.4. Relaciones N:M: propietarios y casas

Una persona puede tener varias casas y una casa varios propietarios. La relación N:M se representa con una **tabla intermedia**:

```sql
CREATE TABLE IF NOT EXISTS propietarios (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  nombre TEXT NOT NULL
);
CREATE TABLE IF NOT EXISTS casas (
  referencia_catastral TEXT PRIMARY KEY,
  direccion TEXT NOT NULL
);
CREATE TABLE IF NOT EXISTS propietarios_casas (
  propietario_id INTEGER NOT NULL,
  referencia_catastral TEXT NOT NULL,
  PRIMARY KEY (propietario_id, referencia_catastral),
  FOREIGN KEY (propietario_id) REFERENCES propietarios(id),
  FOREIGN KEY (referencia_catastral) REFERENCES casas(referencia_catastral)
);
```

Para **vincular** entidades, inserta una fila en `propietarios_casas`; para **desvincular**, elimina **esa fila**, sin borrar al propietario ni a la casa.

```tsx
await db.runAsync(
  'INSERT INTO propietarios_casas (propietario_id, referencia_catastral) VALUES (?, ?)',
  idPropietario, referencia
);
await db.runAsync(
  'DELETE FROM propietarios_casas WHERE propietario_id = ? AND referencia_catastral = ?',
  idPropietario, referencia
);
```

Esta es una diferencia respecto al ejemplo con `@ManyToMany` y `@JoinTable`: con SQL gestionamos expresamente la tabla intermedia. Si la relación tiene atributos propios, como fecha de compra o porcentaje de propiedad, se añaden como columnas a esa tabla.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 63. Modelo N:M propietarios y casas]**
>
> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 65. Guardado de relaciones; sustituir repositorios por operaciones SQL]**

### Práctica 38 — Expo

**Actividad:** Crea un ejercicio nuevo de propietarios y casas con pestañas para introducir propietarios, introducir casas, vincular ambos y consultar sus relaciones muchos a muchos.

**Ubicación en el proyecto único:** `src/app/datos/viviendas/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** Crea un conjunto de tablas y rutas **distintas de la tienda**. Usa una tabla intermedia para la relación N:M.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 65. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

### Práctica 39 — Expo

**Actividad:** Añade una pestaña para eliminar la relación entre un propietario (por id) y una casa (por referencia catastral) sin eliminar los registros principales.

**Ubicación en el proyecto único:** `src/app/datos/viviendas/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** Amplía la app de viviendas con una vista «Desvincular»; deja disponibles las pantallas de alta y vinculación.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 65. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

## 13.5. Transacciones

Una transacción garantiza que varias operaciones relacionadas se confirmen juntas o se deshagan si alguna falla. Expo SQLite expone `withTransactionAsync()` y `withExclusiveTransactionAsync()`. Para un conjunto de escrituras que deben ejecutarse juntas sin interferencias, en Android/iOS puede utilizarse `withExclusiveTransactionAsync()`:

```tsx
import * as SQLite from 'expo-sqlite';
const db = await SQLite.openDatabaseAsync('clase.db');

export async function guardarCasasEnLote(
  casas: { referencia: string; direccion: string }[]
) {
  await db.withExclusiveTransactionAsync(async tx => {
    for (const casa of casas) {
      await tx.runAsync(
        'INSERT INTO casas (referencia_catastral, direccion) VALUES (?, ?)',
        casa.referencia, casa.direccion
      );
    }
  });
}
```

Si una inserción falla, se revierte la transacción. El comportamiento de transacciones y sus restricciones puede variar por plataforma: este ejemplo se plantea para Android/iOS. Comprueba que las tablas existen antes de ejecutarlo.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 67. Transacciones originales con TypeORM; adaptar a `expo-sqlite`]**

### Práctica 40 — Expo

**Actividad:** Implementa `saveAtOnce(casas)` para guardar varias casas en una única transacción SQLite. Comprueba que, si se produce un error, los cambios del lote se revierten.

**Ubicación en el proyecto único:** `src/app/datos/viviendas/` (pantalla específica de esta funcionalidad).

**Cómo continuar sin perder trabajos anteriores:** Agrega la demostración de transacciones a la app de viviendas y prueba éxito y rollback. No pierdas las funciones de las prácticas 38–39.

> 📸 **[INSERTAR CAPTURA DEL DOSSIER ORIGINAL — página 67. Referencia visual; sustituir comandos o código antiguo por el ejemplo Expo de estos apuntes.]**

---

# 14. Entregas, comprobación y documentación

Este dossier se entrega mediante **un único repositorio de GitHub**. Deberás subir el proyecto Expo y actualizar su `README.md` durante todo el bloque. No incluyas `node_modules/` ni credenciales privadas.

Para **cada práctica**, documenta: objetivo, breve explicación técnica, archivos o rutas principales, capturas del proceso y del resultado en el emulador, estado (completada/en proceso/pendiente) y dificultades si las hubo. Guarda las imágenes para el README en `capturas/`.

**Antes de terminar cada jornada de clase** debes subir un commit descriptivo con la fecha y el avance real realizado y entregar el enlace al repositorio en la actividad correspondiente del Aula Virtual. También se evalúa el cumplimiento de las entregas parciales y la calidad de la documentación.

**Comprobación final:** ejecuta `npx expo start`, abre Android Studio, entra en cada opción del menú y asegúrate de que las prácticas anteriores siguen funcionando. Revisa especialmente las rutas Stack/Drawer/Tabs y las tablas de SQLite.

## Recomendaciones generales para las entregas

- Cada práctica se realiza **por separado**, manteniendo el número original. Usa las rutas de la tabla de este dossier y conserva **un solo proyecto Expo** con todas las actividades accesibles desde el menú principal.
- Comprueba cada pantalla en el **emulador de Android Studio**, no solamente en el navegador web.
- Conserva tipos TypeScript, nombres claros, componentes reutilizables y estilos con `StyleSheet.create()`.
- Registra correctamente los permisos, trata los errores de red y usa parámetros en SQLite.
- Cuando en una práctica se indique una captura del dossier original, úsala como **referencia de resultado visual**: el código antiguo de React Native CLI/Gradle/TypeORM no debe copiarse literalmente.

## Documentación oficial de consulta

- Expo: https://docs.expo.dev/
- Creación del proyecto: https://docs.expo.dev/get-started/create-a-project/
- Entorno y emulador: https://docs.expo.dev/get-started/set-up-your-environment/
- Estructura y navegación Expo Router: https://docs.expo.dev/router/introduction/
- Stack: https://docs.expo.dev/router/advanced/stack/
- Tabs: https://docs.expo.dev/router/advanced/tabs/
- SQLite: https://docs.expo.dev/versions/latest/sdk/sqlite/
- React Native: https://reactnative.dev/docs/getting-started

**Fin del dossier.**

