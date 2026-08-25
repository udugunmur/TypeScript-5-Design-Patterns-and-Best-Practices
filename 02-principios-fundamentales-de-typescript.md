# Parte 1: Introducción a TypeScript 5

## Capítulo 2: Principios fundamentales de TypeScript

Hasta ahora, hemos discutido las construcciones de programación básicas de TypeScript, por ejemplo, interfaces, clases, objetos y enumeraciones (*enums*). En este capítulo, examinaremos más detalles sobre TypeScript. Exploraremos tipos avanzados que proporcionan al compilador una comprensión más precisa de tu código, lo que conduce a programas más limpios, concisos y legibles.

Este capítulo no solo te dotará de características avanzadas de TypeScript, sino que también introducirá el concepto de patrones de diseño. Exploraremos sus orígenes, su conexión (y a veces separación) con la Programación Orientada a Objetos (POO / *OOP*) y su papel en la superación de limitaciones en tu código. En los siguientes capítulos, los analizaremos uno por uno en cuanto a sus consideraciones prácticas.

En este capítulo, cubriremos los siguientes temas:

- Trabajo con tipos avanzados
- Desarrollo en el navegador
- Desarrollo en el servidor
- Introducción a los patrones de diseño en TypeScript

Al final de este capítulo, estarás capacitado para escribir programas complejos en TypeScript, aprovechar la POO para representar conceptos del mundo real a través de objetos y trabajar con confianza con TypeScript tanto en entornos de navegador como de servidor.

> [!NOTE]
> Los enlaces a todas las fuentes mencionadas en este capítulo, así como a los materiales de lectura complementarios, se proporcionan en la sección *Lecturas complementarias* hacia el final de este capítulo.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub aquí:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter02_Core_Principles_and_use_cases

---

### Trabajo con tipos avanzados

Nuestro viaje con TypeScript no se detiene en los tipos fundamentales. Ofrece un rico conjunto de construcciones de tipos avanzados que encontrarás frecuentemente en el código del mundo real. Al comprender estas construcciones y cómo funcionan juntas, puedes crear representaciones de tipos más precisas y robustas. En las secciones posteriores, revisemos algunos tipos de utilidad de uso común que están disponibles de forma predeterminada (*out of the box*).

#### Tipos de utilidad (*Utility types*)

Cuando configuras el objetivo de compilación de TypeScript (por ejemplo, ES5, ES6), el compilador incluye automáticamente un archivo de definición global correspondiente (como `lib.es5.d.ts` o `lib.es6.d.ts`). Estos archivos proporcionan una gran cantidad de tipos de utilidad predefinidos para tus proyectos.

Como recordatorio rápido, ES5 (o ECMAScript 5) es un estándar de JavaScript que introdujo varias mejoras clave en JavaScript, como el modo estricto (*strict mode*), nuevos métodos de array para una manipulación simplificada, soporte nativo de JSON y métodos de objetos adicionales. ES6 (o ECMAScript 2015) introdujo funciones flecha (*arrow functions*), clases para programación orientada a objetos, módulos de importación para una mejor organización del código, promesas (*promises*) y plantillas de literales (*template literals*) para interpolación de cadenas. Proporcionamos enlaces a los documentos estándar en la sección *Lecturas complementarias* de este capítulo.

Exploremos algunas utilidades clave y veámoslas en la práctica:

- **`Record`:** La utilidad `Record<K, T>` ayuda a definir tipos de objetos donde las claves de propiedad provienen de un tipo (`K`) y los valores de propiedad de otro (`T`). Esto es particularmente útil para crear objetos de configuración. El tipo de utilidad `Record` es estructuralmente similar a las firmas de índice (*index signatures*) en TypeScript. Ambos te permiten definir un tipo de objeto donde las claves son de un tipo específico y los valores pueden ser de otro tipo. Proporcionamos un ejemplo de una firma de índice en el siguiente ejemplo. Imagina que estás administrando grupos de objetos (*object pools*) en tu aplicación. Es posible que desees una función para recuperar estadísticas de uso (`free`, `used`, `size`) para cada grupo y devolverlas como un objeto estructurado. He aquí cómo se pueden usar eficazmente `Record` y otros tipos:

`Records.ts`:
```typescript
type UsageStats = Record<'free' | 'used' | 'size', number>; 
// type UsageStats = { free: number; used: number; size: number } 

type Metric = { 
    name: string; 
    totalFree: number; 
    totalUsed: number; 
    totalSize: number 
} 

function stats<T extends Metric[]>(data: T): Record<string, UsageStats> { 
    return data.forEach((item) => { 
        const name = item.name 
        stats[name] = { 
            free: item.totalFree, 
            used: item.totalUsed, 
            size: item.totalSize 
        } 
    }) 
}
```

Este fragmento de código calcula y devuelve estadísticas de uso (`free`, `used`, `size`) para varias entidades (representadas por objetos) en un sistema. Lo logra aprovechando el tipo `Record`. El segundo tipo en la sección comentada resaltada muestra cómo escribir un tipo de firma de índice, que es básicamente un tipo de objeto con una estructura clave-valor específica.

`Record<string, UsageStats>` define la estructura del objeto devuelto: las claves son cadenas (`string`) y los valores son objetos de tipo `UsageStats`. El tipo `UsageStats` en sí mismo garantiza que cada objeto de valor tenga propiedades como `free`, `used` y `size`. La función itera a través de un array de objetos (que se asume que contienen datos de entidad) y construye el objeto de estadísticas final devolviendo un tipo `Record`.

- **`Partial`:** `Partial<T>` nos ayuda a crear estructuras de objetos flexibles en nuestro código. Nos permite definir un tipo donde todas las propiedades de otro tipo se vuelven opcionales. Esto es particularmente útil en escenarios como el manejo de entradas del usuario o el tratamiento con objetos con datos potencialmente faltantes. He aquí un ejemplo que demuestra el uso de `Partial` en TypeScript:

`partial.ts`:
```typescript
interface Product { 
    name: string 
    price: number 
    stock?: number // Optional property with default value 
    imageUrl?: string // Optional property 
} 

function createPartialProduct(initialData: Partial<Product>): Product { 
    const defaultProduct: Product = { 
        name: "Unnamed Product", 
        price: 0.0, 
        stock: 10, 
    } 
    return { ...defaultProduct, ...initialData } 
} 

const partialProduct = createPartialProduct({ 
    name: "Cool NFT Item", 
    price: 29.99, 
    imageUrl: https://example.com/cool.png, 
}) 
console.log(partialProduct) 

const minimalProduct = createPartialProduct({ 
    name: "Mystery Item", 
    price: 9.99 
}) 
console.log(minimalProduct) // { name: "Mystery Item", price: 9.99, stock: 10 }
```

La función `createPartialProduct` aprovecha `Partial<Product>`. Esto significa que espera un objeto donde cualquier propiedad de `Product` puede faltar. Luego crea dos instancias de producto: `partialProduct` con todas las propiedades proporcionadas y `minimalProduct` solo con el nombre y el precio. En ambos casos, la función garantiza un objeto de producto completo al completar los valores faltantes con los valores predeterminados.

- **`Required`:** `Required<T>` toma un tipo existente `T` y crea un nuevo tipo donde todas las propiedades de `T` se vuelven obligatorias. Esto ayuda a hacer cumplir la integridad de los datos y evita errores inesperados en tiempo de ejecución debido a datos faltantes. Por ejemplo, consideremos un escenario donde estás creando un sistema de gestión de perfiles de usuario en tu aplicación TypeScript. Defines una interfaz, `UserProfile`, para representar los datos del usuario:

`required.ts`:
```typescript
interface UserProfile { 
    name: string; 
    email: string; // Optional properties 
    bio?: string; // Optional user biography 
    location?: string; // Optional user location 
}
```

Si bien la interfaz refleja la estructura general del perfil de usuario, puede haber situaciones en las que necesites garantizar que exista un tipo de perfil completo antes de realizar ciertas acciones. Por lo tanto, creas un nuevo tipo basado en `UserProfile` donde todas las propiedades se vuelven obligatorias:

`required.ts`:
```typescript
type RequiredUserProfile = Required<Pick<UserProfile, 'name' | 'email'> > 

function displayPublicProfile(profile: RequiredUserProfile): void { 
    console.log(`Name: ${profile.name}, Email: ${profile.email}`) 
} 

const incompleteProfile = { name: "John Doe" } // Compilation error: email is missing 

const completeProfile: RequiredUserProfile = { 
    name: "Jane Doe", 
    email: "jane.doe@example.com" 
} 
displayPublicProfile(completeProfile)
```

Aquí, `RequiredUserProfile` se crea aplicando `Required` a un tipo de utilidad `Pick` de `UserProfile`. El tipo de utilidad `Pick` selecciona las propiedades `name` y `email` de `UserProfile`, y luego `Required` las hace obligatorias. La función `displayPublicProfile` aprovecha este tipo para garantizar que reciba un perfil con nombre y correo electrónico para su visualización pública.

- **`Pick`:** `Pick<T, K>` te permite crear un nuevo tipo seleccionando un conjunto específico de propiedades (`K`) de un tipo existente (`T`). Esto promueve la claridad del código y reduce el riesgo de trabajar con datos irrelevantes dentro de tus componentes. Considera un escenario en el que estás creando un componente de React para un botón. El elemento de botón HTML subyacente tiene varios atributos, pero para este botón específico, solo te interesa manejar las interacciones del usuario (eventos click o submit) y el estilo (`className`, `focus`). He aquí cómo se puede usar `Pick`:

`pick.ts`:
```typescript
type ButtonAttributes = Pick< 
    Partial<HTMLElement>, // Use Partial to make all attributes optional 
    'onclick' | 'className' | 'onfocus'>; 

function createLoggingButton({ onclick, className, onfocus }: ButtonAttributes): HTMLButtonElement { 
    const button = document.createElement('button'); 
    ... 
    return button; 
}
```

En el ejemplo anterior, `Pick` se utilizó para crear un tipo, `ButtonAttributes`, que extrae solo las propiedades relevantes (`onclick`, `className` y `onfocus`) de la interfaz `HTMLElement` de un `HTMLButtonElement`. Esto asegura que el desarrollador tampoco tenga que tipar esas propiedades manualmente.

- **`Omit`:** Si necesitas excluir ciertas propiedades, utiliza en su lugar el tipo `Omit<T, K>`. Es lo opuesto a `Pick`, y te ayuda a crear un nuevo tipo eliminando propiedades especificadas (`K`) de un tipo existente (`T`). Esto es particularmente útil cuando deseas modificar la estructura de un tipo o crear variaciones opcionales. Imagina que estás creando un formulario de registro de usuario en tu aplicación TypeScript. Defines una interfaz de usuario integral que captura todos los datos potenciales del usuario:

`Omit.ts`:
```typescript
interface User { 
    name: string 
    email: string 
    password: string 
    confirmPassword: string 
    bio?: string // Optional user bio 
    location?: string // Optional user location 
}
```

Sin embargo, al procesar los datos enviados del formulario, es posible que no necesites ciertas propiedades como `confirmPassword` (utilizada para validación pero no almacenada) o campos opcionales potencialmente faltantes como `bio` y `location`. `Omit` se puede usar para crear un nuevo tipo que excluya estas propiedades innecesarias, asegurando que trabajes con una estructura de datos más limpia para su posterior procesamiento:

`omit.ts`:
```typescript
// Expected form data 
type UserInput = Pick<User, "name" | "email" | "password" | "confirmPassword"> 
// Excludes unnecessary properties 
type ProcessedUserData = Omit<User, "confirmPassword" | "bio" | "location">
```

Aquí, `Omit` toma la interfaz de usuario existente y elimina las propiedades enumeradas (`confirmPassword`, `bio` y `location`). Esto da como resultado un nuevo tipo `ProcessedUserData` que excluye estas propiedades.

> [!NOTE]
> Tanto los tipos `Partial` como `Required` solo funcionan en el primer nivel de la lista de propiedades de un tipo. Esto significa que no se aplicarán recursivamente a propiedades profundamente anidadas de objetos. Si sabes que un objeto contiene múltiples objetos anidados, tendrás que marcarlos explícitamente como `Partial` o `Required` también.

He aquí una tabla de resumen de los tipos de utilidad analizados hasta ahora (*Figura 2.1: Resumen de los tipos de utilidad más comunes en TypeScript*):

| Tipo de Utilidad | Descripción | Ejemplo de Uso |
| :--- | :--- | :--- |
| `Partial` | Crea un tipo con todas las propiedades del tipo dado establecidas como opcionales. | `type User = Partial<{ name: string; age: number; }>;` |
| `Required` | Crea un tipo con todas las propiedades del tipo dado establecidas como obligatorias. | `type User = Required<{ name?: string; age?: number; }>;` |
| `Pick` | Crea un tipo seleccionando el conjunto de propiedades `K` del tipo `T`. | `type User = Pick<{ name: string; age: number; }, 'name'>;` |
| `Omit` | Crea un tipo omitiendo el conjunto de propiedades `K` del tipo `T`. | `type User = Omit<{ name: string; age: number; }, 'age'>;` |

Al reconocer cuándo y por qué utilizar tipos de utilidad en tus programas, puedes mejorar la legibilidad y la corrección de tus tipos de datos. Continuaremos aprendiendo sobre comprobaciones y transformaciones de tipos avanzadas en las secciones siguientes.

#### Uso de tipos avanzados y aserciones

Esta sección se enfoca más allá de los tipos de utilidad fundamentales que has encontrado hasta ahora. A medida que trabajas en proyectos más grandes, es posible que sientas la necesidad de crear tipos de utilidad personalizados específicos para los requisitos de tu aplicación. Además, puede haber situaciones en las que desees imponer comprobaciones de tipos más estrictas basadas en lógica condicional.

Por nombrar algunas, en muchas aplicaciones, ciertas reglas de negocio dictan cómo deben estructurarse o validarse los datos. Las comprobaciones de tipos más estrictas te permiten hacer cumplir estas reglas a nivel de tipo, garantizando que solo se puedan procesar datos válidos. Además, al interactuar con APIs externas, la estructura de los datos devueltos puede variar. Con comprobaciones más estrictas implementadas, puedes asegurarte de que tu aplicación maneje correctamente diferentes respuestas de API según el contexto.

Aquí, exploraremos cómo utilizar todo el potencial del sistema de tipos de TypeScript para lograr estos objetivos.

El poder del sistema de tipos de TypeScript se extiende más allá de los tipos de utilidad predefinidos. Descubrirás técnicas para generar tipos completamente nuevos y únicos adaptados a tus necesidades específicas. Esta sección profundizará en conceptos como tipos con marca (*branded types*) y propiedades de símbolos únicos, equipándote con las herramientas para crear tipos personalizados y diferenciados dentro de tus aplicaciones TypeScript.

Al explorar estas técnicas avanzadas de manipulación de tipos, obtendrás la capacidad de realizar comprobaciones de tipos más granulares, lo que conducirá a una base de código más robusta y mantenible.

##### El operador `keyof`

Imagina que estás creando un formulario en tu aplicación TypeScript. Defines una interfaz, `SignupFormState`, para representar la estructura de los datos del formulario de registro:

`keyof.ts`:
```typescript
interface SignupFormState { 
    email: string; 
    name: string; 
}
```

Ahora, necesitas definir una interfaz para el payload de una acción que actualiza el estado del formulario de registro. Este payload de acción debe tener dos propiedades:
- `key`: Para identificar qué campo del formulario necesita actualización (por ejemplo, `email` o `name`).
- `value`: El nuevo valor para el campo del formulario especificado.

Aquí es donde el operador `keyof` resulta útil:

`keyof.ts`:
```typescript
interface ActionPayload<T> { 
    key: keyof T; // Capture all keys of type T using keyof 
    value: string; 
} 

// Usage with SignupFormState 
type SignupFormAction = ActionPayload<SignupFormState>; 

const updateEmailAction: SignupFormAction = { 
    key: 'email', // Autocomplete suggests all keys from SignupFormState 
    value: 'new_email@example.com' 
}; 

const updateNameAction: SignupFormAction = { 
    key: 'name', 
    value: 'John Doe' 
}; 

// Example of an invalid action 
const updateInvalidAction: SignupFormAction = { 
    key: 'username', // Error: Type '"username"' is not assignable to type 'keyof SignupFormState' 
    value: 'invalid_user' 
};
```

Al utilizar `keyof`, puedes crear una interfaz genérica, `ActionPayload<T>`, que captura todas las claves de cualquier tipo `T`. En nuestro ejemplo, cuando usamos `ActionPayload<SignupFormState>`, crea un tipo de unión (`"email" | "name"`) que representa todos los nombres de clave posibles (`email` o `name`) dentro del estado del formulario de registro.

Durante la creación de la acción, puedes utilizar la lista de autocompletado proporcionada por tu IDE (*Figura 2.2: El autocompletado en keyof muestra las propiedades disponibles*). Al configurar la propiedad `key` de un objeto de acción (por ejemplo, `updateEmailAction`), el IDE sugerirá todas las claves válidas de `SignupFormState`.

Al utilizar `keyof` eficazmente, puedes crear componentes reutilizables y con seguridad de tipos que son flexibles y personalizables. A continuación, veamos cómo imponer rigurosidad en el sistema de tipado utilizando tipos con marca (*branded types*).

##### Tipos con marca únicos (*Unique branded types*)

En el ámbito de los lenguajes de programación, los sistemas de tipos juegan un papel crucial para garantizar la fiabilidad y el mantenimiento del código. Existen dos enfoques principales para los sistemas de tipos: estructural y nominal. Una vez que comprendas su diferencia, el concepto de tipos con marca se volverá relevante, tal como explicamos más adelante en la sección.

Los sistemas de tipos nominales otorgan la máxima importancia al nombre específico o identidad de un tipo. Si dos tipos comparten una estructura idéntica, un sistema nominal los considera entidades distintas.

Los sistemas de tipos estructurales, por otro lado, priorizan la estructura de un tipo, centrándose en los nombres y tipos de datos de sus propiedades. La compatibilidad entre tipos depende únicamente de esta estructura. Recuerda que TypeScript utiliza tipado estructural. Esto significa que al comparar tipos, se centra en la estructura (nombres de propiedad y tipos) en lugar del nombre del tipo en sí.

Imagina que tienes dos tipos: `Point3d` con propiedades `x`, `y` y `z`, y `Color` con propiedades `red`, `green` y `blue`. Luego definimos otro tipo con una estructura de tipos similar a la del tipo `Point3d`:

`branded.ts`:
```typescript
// Assume Point3d is defined in a library package (not exported) 
type Point3d = { x: number; y: number; z: number }; 

// Defined in Library Package 1 
type Color = { red: number; green: number; blue: number }; 

type Dot = { x: number; y: number; z: number }; // Defined in Consumer Package 

const data: Dot = { x: 1, y: 2, z: 3 } 
const red: Color = { red: 255, y: 0, z: 0 }
```

En este ejemplo, el tipo `Point3d` se define en un paquete de biblioteca (digamos que es el Paquete de Biblioteca 1), pero no se exporta para su uso en otros paquetes. El tipo `Dot` se define en un paquete consumidor donde estás usando la biblioteca.

Aunque a `data` no se le asignó explícitamente un tipo de `Point3d`, coincide estructuralmente con los requisitos de `Point3d`. En el tipado estructural, siempre que las propiedades y sus tipos coincidan, los objetos se consideran compatibles. Por lo tanto, el siguiente uso de llamadas a funciones es TypeScript válido:

`branded.ts`:
```typescript
function accept(p: Point3d) {} // valid 

accept(data) 
// not valid 
accept(red)
```

La llamada a la función `accept(data)` está permitida. TypeScript ve que `data` tiene las propiedades necesarias (`x`, `y` y `z`) con los tipos correctos (`number`) para cumplir los requisitos del parámetro `Point3d` en la función `accept`. Sin embargo, la llamada `accept(red)` no está permitida ya que el objeto pasado como argumento tiene propiedades diferentes.

Sin embargo, puede haber situaciones en las que desees un sistema de tipos más estricto, similar al tipado nominal que se encuentra en lenguajes como Java o Go. ¡Aquí es donde entran los tipos con marca (*branded types*)! He aquí el concepto central:

`branded.ts`:
```typescript
type NominalTyped<Type, Brand> = Type & { __type: Brand };
```

En el código anterior, definimos un tipo genérico, `NominalTyped<Type, Brand>`. Esto nos permite marcar cualquier tipo con una marca personalizada (`Brand`), que actúa como un identificador único. Ahora, integrémoslo en nuestro parámetro de función para que solo acepte tipos `Point3d` auténticos:

`branded.ts`:
```typescript
type EuclideanPoint = NominalTyped<{ x: number; y: number; z: number }, 'Point3d'>; 

function accept2(point: EuclideanPoint) { 
    console.log(`Point coordinates: (${point.x}, ${point.y}, ${point.z})`); 
}
```

Definimos un tipo con marca, `EuclideanPoint`, extendiendo `Point3d` con una marca de símbolo único. Esta marca actúa como una bandera, indicando que este punto específico está destinado a cálculos de distancia. La función `accept2` ahora solo acepta argumentos de tipo `EuclideanPoint`, asegurando que el cálculo funcione con puntos específicamente diseñados para este propósito. Cualquier intento de pasar un objeto `Point3d` normal daría lugar a un error de tipo, evitando posibles problemas.

El uso de símbolos únicos como marcas es crucial para hacer cumplir distinciones de tipos más fuertes, incluso cuando las estructuras subyacentes son idénticas. Esto mejora la seguridad de tipos y evita la asignación accidental entre tipos estructurados de manera similar.

El siguiente ejemplo demuestra la importancia de utilizar símbolos únicos para marcar tipos con el fin de evitar asignaciones accidentales entre tipos de estructura similar:

`branded.ts`:
```typescript
const Brand1 = Symbol("Brand1"); 
const Brand2 = Symbol("Brand2"); 

type TypeA = NominalTyped<number, typeof Brand1>; 
type TypeB = NominalTyped<number, typeof Brand2>; 

const a: TypeA = 10 as TypeA; 
const b: TypeB = 10 as TypeB; 

// This should cause a TypeScript error: 
const c: TypeA = b; // Error: Type 'TypeB' is not assignable to type 'TypeA'
```

En este ejemplo, `TypeA` y `TypeB` están marcados con símbolos únicos. Aunque ambos son tipos numéricos con marca, TypeScript los considera distintos debido a las marcas de símbolo únicas. Intentar asignar un `TypeB` a una variable `TypeA` generará un error de tipo.

Al incorporar tipos con marca, dispones de otra herramienta en tu caja de utilidades para mejorar la seguridad y la claridad de tipos de tu código TypeScript, imitando algunas de las ventajas de los sistemas de tipos nominales.

##### Tipos condicionales (*Conditional types*)

TypeScript ofrece una potente característica llamada tipos condicionales, que te permite definir tipos basados en lógica condicional. Esto amplía las capacidades del sistema de tipos más allá de las simples comparaciones estructurales.

Así es como funciona. La sintaxis general para un tipo condicional es `A extends B ? C : D`. Aquí, `A`, `B`, `C` y `D` representan parámetros de tipo que pueden contener cualquier tipo. La condición `A extends B` comprueba si el tipo `A` es estructuralmente compatible con el tipo `B`. Si la condición es verdadera, se devuelve el tipo `C`. De lo contrario, se devuelve el tipo `D`.

Supongamos que deseas crear un tipo que detecte si el parámetro de tipo genérico es un array. Lo escribirías así:

`conditional.ts`:
```typescript
type IsArray<T> = T extends any[] ? true : false; 

type Test1 = IsArray<number>; // false 
type Test2 = IsArray<string[]>; // true 
type Test3 = IsArray<boolean[]>; // true
```

El tipo `IsArray` utiliza una comprobación condicional. Si el tipo `T` proporcionado pudiera ser potencialmente cualquier array (`T extends any[]`), el tipo resultante es `true`, indicando que efectivamente es un array. De lo contrario, el tipo es `false`. Esto te permite capturar la información de tipo en una variable.

También puedes usar tipos condicionales para crear un tipo que elimine condicionalmente `null` o `undefined` de un tipo de unión. He aquí cómo hacerlo:

`conditional.ts`:
```typescript
type NonNullable<T> = T extends null | undefined ? never : T; 

type Example = NonNullable<string | number | null>; // string | number
```

En este ejemplo, el tipo `NonNullable<T>` comprueba si `T` es `null` o `undefined`. Si lo es, devuelve `never`, eliminándolo eficazmente de la unión. El tipo `Example` se evalúa como `string | number`, ya que `null` se elimina de la unión.

Los tipos condicionales se utilizan colectivamente con la palabra clave `infer`. Puedes darle un nombre a un tipo o parámetro genérico para que luego puedas realizar comprobaciones condicionales.

Imagina que estás trabajando con un programa que utiliza cajas para almacenar diferentes tipos de datos. Estas cajas tienen una sola propiedad llamada `value` que contiene los datos reales. Pero, ¿cómo sabes qué tipo de datos hay dentro de una caja sin abrirla?

Aquí es donde entran los tipos condicionales con la palabra clave `infer`. Te permiten definir un tipo que comprueba la estructura de otro tipo. En este ejemplo, creamos un tipo llamado `Box<T>` para representar una caja que puede contener cualquier tipo `T`:

`infer.ts`:
```typescript
// Define a box type that can hold any type of value 
interface Box<T> { 
    value: T 
} 

// Define a type to unpack a box and reveal its value type 
type UnpackBox<A> = A extends Box<infer E> ? E : A 

// Example usage: 
type intStash = UnpackBox<{ value: 10 }> // type is number 
type stringStash = UnpackBox<{ value: "123" }> // type is string 
type booleanStash = UnpackBox<true> // type is boolean
```

Este código define una interfaz `Box<T>` que representa un contenedor que puede contener cualquier tipo de valor. Luego, introduce un tipo condicional, `UnpackBox<A>`, que extrae el tipo del valor contenido dentro de un `Box`. Si `A` coincide con la estructura de `Box<infer E>`, devuelve `E`, el tipo del valor; de lo contrario, devuelve `A`. El código proporciona ejemplos que demuestran el uso de `UnpackBox`, mostrando que deduce correctamente el tipo del valor dentro de la interfaz `Box` para varios escenarios: un número, una cadena y un booleano.

Ahora, ampliemos el ejemplo para incluir tipos condicionales más complejos, como desempaquetar recursivamente cajas anidadas o inferencia de tipos condicional basada en múltiples condiciones. Así es como se hace:

`infer.ts`:
```typescript
// Define a type to recursively unpack nested boxes 
type DeepUnpack<T> = T extends Box<infer U> ? DeepUnpack<U> : T; 

type nestedBox = Box<Box<number>>; // Box containing another Box 
type unpackedNested = DeepUnpack<nestedBox>; // type is number 

type deeplyNestedBox = Box<Box<Box<string>>>; // Box containing a Box containing another Box 
type unpackedDeeplyNested = DeepUnpack<deeplyNestedBox>; // type is string
```

Aquí, el tipo `DeepUnpack<T>` comprueba recursivamente si `T` es un `Box`. Si lo es, desempaqueta el tipo de valor `U` y vuelve a aplicar `DeepUnpack`. Esto continúa hasta que llega a un tipo que no es una interfaz `Box`. En el ejemplo de uso, `nestedBox` es una caja que contiene otra caja con un número y `deeplyNestedBox` es una caja que contiene una caja que contiene otra caja con una cadena.

Al practicar estos conceptos avanzados, generas tipos sofisticados que modelan con mayor precisión los objetos de dominio que deseas utilizar. En última instancia, produces tipos para aprovechar el proceso de compilación contra errores, como asignaciones incorrectas u operaciones no válidas.

##### Tipos mapeados (*Mapped types*)

Los tipos mapeados en TypeScript te permiten tomar un tipo existente y transformarlo en uno nuevo, aplicando una regla específica a cada propiedad dentro del tipo original. Esto te permite crear variaciones de tipos existentes que se adapten a tus necesidades.

He aquí algunos ejemplos para mostrar su potencial:

`mapped.ts`:
```typescript
interface User { 
    name: string 
    avatar?: string // Optional avatar 
} 

type OptionalAvatarUser = { 
    // Keyof gets all user properties 
    [P in keyof User]?: User[P] 
} 

const user1: OptionalAvatarUser = { name: "Alice" } 
const user2: OptionalAvatarUser = { name: "Bob", avatar: "avatar.png" }
```

Aquí, el tipo `OptionalAvatarUser` utiliza una sintaxis de tipo mapeado. Itera sobre todas las propiedades (`P`) de la interfaz de usuario usando el operador `keyof`. Dentro de los corchetes, el tipo de cada propiedad (`User[P]`) se hace opcional añadiendo un signo de interrogación (`?`). Esto crea un nuevo tipo donde `name` sigue siendo una cadena pero `avatar` puede ser `string` o `undefined`. Las dos variables `user1` y `user2` son válidas utilizando este mapeo.

Los tipos mapeados también se pueden usar para crear una reasignación de claves utilizando la palabra clave `as`. Supongamos que tienes un objeto de producto con propiedades como `id` (`number`) y `price` (`number`). Es posible que desees crear un nuevo tipo que muestre ambas propiedades pero con etiquetas legibles por humanos. Un tipo mapeado puede lograr esto:

`mapped.ts`:
```typescript
interface Product { 
    id: number; 
    price: number; 
} 

type ProductDetails = { 
    [P in keyof Product as `product ${P}`]: string; // Transform property names 
}; 

const product1: ProductDetails = { 
    "product id": "123", 
    "product price": "100", // Converted to string 
};
```

Este ejemplo define un tipo `ProductDetails`. Utiliza un tipo mapeado nuevamente, iterando sobre las propiedades de `Product`. Sin embargo, esta vez, usamos una plantilla literal (`as product ${P}`) para transformar los nombres de las propiedades anteponiendo la palabra `product ` antes de cada una. Además, el tipo permanece como `string` para todas las propiedades, lo que te permite almacenar detalles de productos formateados.

Estos son solo algunos ejemplos de cómo se pueden usar los tipos mapeados en TypeScript. Ofrecen una forma potente y flexible de manipular y transformar tipos existentes, lo que genera un código más limpio y expresivo.

Ahora, continuemos nuestra exploración de TypeScript viendo cómo desarrollar aplicaciones con TypeScript en el entorno del navegador.

---

### Desarrollo en el navegador

El poder de TypeScript como superconjunto de JavaScript se adapta perfectamente al entorno del navegador. Te capacita para escribir código JavaScript más limpio y mantenible, garantizando al mismo tiempo la seguridad de tipos para una experiencia de desarrollo más confiable. Sin embargo, el navegador presenta su propio conjunto de consideraciones.

Por lo tanto, comprender el Modelo de Objetos del Documento (*Document Object Model* / DOM) es crucial. Al dominar la API del DOM, puedes interactuar eficazmente con estos elementos, manipulándolos y respondiendo a eventos del usuario como clics y desplazamientos. La seguridad de tipos de TypeScript ayuda a evitar errores comunes en este proceso, pero la compatibilidad del navegador aún requiere consideración.

Herramientas como Webpack y Vite simplifican el desarrollo. Estos empaquetadores (*bundlers*) automatizan tareas como la compilación y minificación, optimizando tu código para producción. Gestionan dependencias, crean paquetes que contienen tu código y librerías, y mejoran los tiempos de carga. Los frameworks de interfaz de usuario como React van un paso más allá: proporcionan una arquitectura basada en componentes para crear aplicaciones web complejas e interactivas de manera eficiente, a menudo aprovechando TypeScript para la seguridad de tipos dentro de tus componentes.

La siguiente tabla resume brevemente las diferencias entre Node.js y el JavaScript del frontend (*Figura 2.3: Diferencias entre los entornos de Node.js y el Navegador*):

| Característica | Node.js (Backend) | JavaScript del Frontend (Navegador) |
| :--- | :--- | :--- |
| **Estructura de archivos** | Cada archivo se trata como un módulo individual. | Múltiples archivos se combinan en un solo archivo usando empaquetadores y luego se cargan en el DOM como una etiqueta `<script>`. |
| **Gestión de módulos** | Utiliza CommonJS o módulos ES para importar/exportar. | Utiliza módulos ES de JavaScript y Expresiones de Función Ejecutadas Inmediatamente (IIFEs). |
| **Contexto de ejecución** | Se ejecuta en el servidor, permitiendo el acceso al sistema de archivos y a la red. | Se ejecuta en el navegador, interactuando con el DOM. |
| **Entorno** | Diseñado para aplicaciones del lado del servidor, manejando peticiones y respuestas. | Diseñado para aplicaciones del lado del cliente, enfocado en las interacciones del usuario y la UI. |
| **APIs** | Proporciona módulos integrados para el sistema de archivos, HTTP y más. | Acceso a las APIs del DOM para manipular HTML/CSS y manejar eventos. |
| **Rendimiento** | Optimizado para manejar múltiples solicitudes simultáneamente. | El rendimiento puede verse afectado por el renderizado del navegador y el manejo de eventos. Principalmente monohilo, pero se puede delegar trabajo mediante Web Workers. |

Al combinar tu comprensión del entorno del navegador, el DOM y el manejo de eventos con las fortalezas de TypeScript y herramientas eficientes, puedes construir aplicaciones web escalables y dinámicas. TypeScript garantiza la seguridad de tipos, lo que conduce a experiencias web más robustas y confiables.

Primero, deseas entender cómo trabajar con el DOM y TypeScript, por lo que a continuación proporcionaremos un par de ejemplos prácticos.

> [!NOTE]
> Algunos de los ejemplos no pueden funcionar al usar Node.js ya que dependen del objeto global `window`, por lo que necesitarás usar un navegador moderno como Chrome o Firefox.

#### Entendiendo el DOM

Cuando cargas una página HTML, el navegador convierte el código en un árbol estructurado conocido como Modelo de Objetos del Documento (DOM). Este árbol representa los elementos visuales de la página, y cada nodo corresponde a un elemento como `<div>` o `<p>`.

Aunque exploraremos los patrones de diseño en capítulos posteriores, vale la pena señalar que el DOM utiliza estos patrones para su creación y manipulación eficientes. Por ejemplo, el patrón Factory Method, explorado en el Capítulo 3, asegura que se creen los elementos de nodo adecuados en función de las etiquetas (por ejemplo, `div`, `p`), mientras que el patrón Visitor, explorado en el Capítulo 6, permite un recorrido eficiente de la estructura del DOM.

Para darte una demostración rápida, consideremos un documento HTML simple y su correspondiente árbol DOM:

`DOM/index.html`:
```html
<!doctype html> 
<html lang="en"> 
<head> 
    <title>Document</title> 
</head> 
<body> 
    <div id="section-1"> 
        <span>Typescript 5 Design Patterns</span> 
    </div> 
    <p class="paragraph"></p> 
    <button type="submit">Submit</button> 
    <script src="dist/index.js"></script> 
</body> 
</html>
```

En el código anterior, el elemento de nivel superior es `<html>`, con dos hijos: `<head>` y `<body>`. El `<body>` contiene más elementos secundarios (`<div>`, `<p>`, `<button>`, `<script>`) que también pueden tener sus propios elementos anidados (*Figura 2.4: Árbol DOM del ejemplo HTML proporcionado*).

El árbol mencionado se puede manipular o recorrer utilizando la API del DOM, que es un conjunto de funciones y métodos proporcionados por el entorno del navegador que permite el control completo de su contenido.

Para configurar un nuevo proyecto con acceso al DOM en TypeScript, puedes incluir las definiciones de tipo `lib.dom.d.ts` en tu archivo `tsconfig.json`:

```json
{ 
    "compilerOptions": { 
        "lib": ["DOM"], 
        "outDir": "./" 
    } 
}
```

Estas definiciones de tipos proporcionan tipos esenciales para los elementos del DOM, como `Element`, `HTMLElement` y `ShadowRoot`. El alcance completo de la API del DOM es extenso, y se puede encontrar documentación detallada en https://html.spec.whatwg.org/.

Necesitarás un documento HTML para hacer referencia al TypeScript compilado. Puedes usar el documento HTML anterior e `index.ts` como ejemplo:

`DOM/index.ts`:
```typescript
const p = document.querySelector<HTMLParagraphElement> (".paragraph"); 
const spanArea = document.createElement("span"); 
spanArea.textContent = "This is a text we added dynamically"; 
p?.appendChild(spanArea); 
const actionButton = document .querySelector<HTMLButtonElement>("button"); 
actionButton?.addEventListener("click", () => { 
    window.alert("You Clicked the Submit Button"); 
});
```

Utilizando la API del DOM, podemos realizar una serie de operaciones para consultar, crear y modificar objetos de nodo. Veamos cada una de esas operaciones en los siguientes puntos:

- **Creación de un nuevo elemento:** Creando un nuevo elemento del tipo span:
  ```typescript
  const spanArea = document.createElement("span");
  ```
  Este código crea un nuevo elemento HTML de tipo `span`. La variable `spanArea` se infiere de tipo `HTMLSpanElement`, que hereda del tipo más general `HTMLElement`. Esto proporciona seguridad de tipos y sugerencias de autocompletado de código para propiedades y métodos disponibles específicos de los elementos `span`.

- **Búsqueda de elementos existentes:** Consultando elementos por nombre de clase o propiedad de ID:
  ```typescript
  const p = document .getElementsByClassName< HTMLParagraphElement> ("paragraph")[0]; 
  const div = document .getElementById <HTMLDivElement>("section-1");
  ```
  El método `getElementsByClassName` toma un nombre de clase como argumento y devuelve una interfaz `HTMLCollectionOf<Element>`. Esta colección puede contener múltiples elementos con la clase especificada. En este ejemplo, accedemos al primer elemento (`[0]`) asumiendo que solo hay un párrafo con esa clase. El método `getElementById` toma un ID de elemento como argumento y devuelve una sola interfaz `HTMLElement` si se encuentra. El atributo ID debe ser único dentro del documento para garantizar que se dirija al elemento correcto.

- **Asociación de manejadores de eventos (*event handlers*):** Añadiendo un escuchador de eventos en el evento clic de un botón:
  ```typescript
  actionButton?.addEventListener("click", () => { 
      window.alert("You Clicked the Submit Button"); 
  });
  ```
  Este fragmento de código demuestra cómo adjuntar un escuchador de eventos de clic a un elemento de botón. El método `addEventListener` toma dos argumentos: el tipo de evento (`"click"` en este caso) y una función de devolución de llamada (*callback*) que se ejecuta cuando ocurre el evento. Aquí, usamos el operador de encadenamiento opcional (`?.`) para acceder de manera segura al método `addEventListener`. Comprueba si el elemento `actionButton` existe antes de intentar llamar al método, lo que evita posibles errores.

Puedes verificar el código anterior compilando `index.ts` a `index.js` e iniciando un servidor HTTP estático para ver el documento `index.html`.

Sigue estos pasos para configurar un sitio web básico que utilice los scripts anteriores:

1. Comienza instalando un servidor estático simple de node usando este paquete:
   ```bash
   $ npm -g install node-static
   ```
2. Para compilar el archivo TypeScript, solo necesitas ejecutar la siguiente tarea: *Build Chapter 2 HTML DOM Example*. Ejecuta esto desde la lista de tareas de VSCode a través de **Terminal | Run Task...** (*Figura 2.5: Tarea Build Chapter 2*).
3. Esta tarea compilará `index.ts` en `index.js` y los colocará en la carpeta `dist`. Luego, con la ayuda de un servidor estático, puedes inspeccionar la página:
   ```bash
   $ cd chapters/chapter-2_Core_Principles_and_use_cases/src/DOM 
   $ static
   ```
4. Abre un navegador y navega a `http://localhost:8080`. Verás la página mostrada en la *Figura 2.6: Vista en el navegador del ejemplo DOM*.

Con VSCode y algunas tareas de compilación, podemos poner en marcha rápidamente un proyecto simple con facilidad y sin necesidad de muchas herramientas adicionales. Sin embargo, en programas más grandes con miembros adicionales en el equipo, es útil tener un empaquetador de módulos como Webpack o Vite. Veamos cómo puedes lograr eso a continuación.

#### Uso de TypeScript con Vite

Vite y TypeScript forman un dúo poderoso para el desarrollo optimizado con React y TypeScript. Esta es una herramienta que genera la configuración del proyecto (*scaffolding*), automatiza las compilaciones de producción, crea un servidor de desarrollo con recarga automática y, en esencia, hace que el desarrollo sea más amigable.

He aquí una guía concisa para comenzar en tres sencillos pasos:

1. **Crear un nuevo proyecto de Vite:** Utiliza el siguiente comando en tu terminal preferida, que crea un nuevo proyecto de Vite con la plantilla de TypeScript puro (*vanilla*):
   ```bash
   $ npx create-vite@latest my-app --template vanilla-ts
   ```
   Reemplaza `my-app` con el nombre de tu aplicación preferida. El indicador `--template` especifica la plantilla a utilizar.

2. **Compilar el proyecto:** Una vez creado el proyecto, cambia al directorio de la nueva carpeta de la aplicación y compila el código:
   ```bash
   $ cd my-app 
   $ npm run build 
   vite v5.2.11 building for production... 
   ✓ 7 modules transformed. 
   dist/index.html 0.46 kB │ gzip: 0.29 kB 
   dist/assets/index-Cz4zGhbH.css 1.21 kB │ gzip: 0.63 kB 
   dist/assets/index-Bd-pKGJy.js 3.05 kB │ gzip: 1.62 kB 
   ✓ built in 55ms
   ```
   Una compilación exitosa creará esos recursos basados en el archivo de configuración `vite.config.ts`.

3. **Ejecutar la aplicación:** Para probar la aplicación, ejecuta el siguiente comando:
   ```bash
   $ npm run preview
   ```
   Esto iniciará el servidor de vista previa de la carpeta `dist` compilada, accesible típicamente en `http://localhost:4173/`.

Hablando de usar TypeScript en el navegador, exploremos a continuación cómo usar TypeScript con React y cómo crear componentes con seguridad de tipos.

#### Uso de React

React, una popular librería de UI desarrollada por Facebook, te permite crear interfaces escalables e intuitivas. Al aprender React con TypeScript, ganarás experiencia con patrones de diseño como la composición, la inmutabilidad y la ausencia de estado (*statelessness*).

Muchos frameworks como Next.js tienen soporte integrado con React y TypeScript, pero algunas de las definiciones de tipos requieren algo de tiempo para acostumbrarse.

Para comenzar, hemos incluido un proyecto de muestra que utiliza TypeScript, React y Vite que procesa un componente simple en la página web. Este se encuentra en la carpeta `react` dentro del código fuente de este capítulo en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter02_Core_Principles_and_use_cases/src/react/src

Sin embargo, debes tener en cuenta algunas diferencias en la configuración:
- Añadimos la opción del compilador `"jsx": "react-jsx"` para habilitar las factorías JSX y TSX. Esto nos permite escribir código idiomático que se asemeja a HTML dentro del código fuente. El compilador creará los constructores necesarios para la librería React DOM.
- La opción del compilador `lib` incluye `DOM` y `DOM.Iterable`, que son librerías que admiten funcionalidades relacionadas con el DOM en JavaScript.

He aquí un desglose de tres formas comunes de declarar componentes funcionales de React con propiedades (*props*) e hijos (*children*):

- **Uso de la anotación de tipo `React.FC<Props>`:** Este enfoque define explícitamente el tipo de props que espera el componente mediante una interfaz de tipo u objeto. He aquí un ejemplo:

`components/Greeting.tsx`:
```typescript
import React from "react" 

interface GreetingProps { 
    name: string 
} 

const Greeting: React.FC<GreetingProps> = ({ name = "World" }) => { 
    return <h1>Hello, {name}!</h1> 
} 

export default Greeting
```

Este fragmento de código utiliza un componente funcional de React (FC) con seguridad de tipos mediante TypeScript. Define una interfaz de tipo llamada `GreetingProps` que especifica el tipo esperado para la propiedad `name` (`string`). El componente, `Greeting`, se declara como un FC usando `React.FC<GreetingProps>`, lo que indica que acepta props de ese tipo. Devuelve JSX que muestra un elemento de encabezado con el mensaje de saludo personalizado por la propiedad `name`. El tipo `FC` incluye algunos campos adicionales específicos de React, como `defaultProps` y `displayName`.

- **Uso de una firma de función con el argumento props:** Una forma más sencilla es simplemente declarar un argumento `props` que especifique el tipo de las propiedades del componente que puede aceptar sin campos adicionales específicos de React:

`components/Greeting2.tsx`:
```typescript
import React from "react" 

interface GreetingProps { 
    name: string 
} 

const Greeting2 = (props: GreetingProps) => { 
    const { name = "World" } = props 
    return <h1>Hello, {name}!</h1> 
} 

export default Greeting2
```

Aquí, es una función que acepta directamente un argumento llamado `props` de tipo `GreetingProps` con solo una propiedad llamada `name`.

- **Uso de `React.PropsWithChildren` para soporte de elementos secundarios (*children*):** Una tercera forma es útil si deseas incluir una propiedad `children` para renderizar el árbol de hijos del componente. He aquí un ejemplo:

`components/Button.tsx`:
```typescript
import { PropsWithChildren } from "react" 

export type ButtonProps = { 
    onClick: (e: React .MouseEvent<HTMLButtonElement>) => void 
} 

const Button: React .FC<PropsWithChildren<ButtonProps>> = ({ children, onClick }) => { 
    return <button onClick={onClick}> {children}</button> 
} 

export default Button
```

Aquí, el tipo `PropsWithChildren` de React maneja componentes que pueden tener elementos secundarios. El componente `Button` en sí se declara como un `FC` que envuelve este tipo, mejorando sus tipos finales.

Ahora, si deseas compilar y ver este proyecto de React que muestra los componentes, solo necesitas navegar a la carpeta del proyecto, ejecutar la siguiente línea de comandos y visitar `http://localhost:5173/`:

```bash
$ npm run dev
```

La captura de pantalla en la *Figura 2.7: Ejemplo de React con TypeScript en el navegador* muestra lo que deberías esperar ver.

Al igual que con otros ejemplos, el uso de React y TypeScript es una buena combinación, ya que podemos aprovechar los patrones más destacados para el desarrollo de interfaces de usuario. Por patrones destacados nos referimos a que podemos utilizar la composición, la programación funcional y la seguridad de tipos para crear aplicaciones web escalables.

Ahora que has entendido los conceptos básicos del desarrollo de estas aplicaciones con TypeScript en el navegador, exploraremos los conceptos básicos del desarrollo en el entorno del servidor.

---

### Desarrollo en el servidor

Ahora que conoces los conceptos básicos del desarrollo de aplicaciones en el navegador, puedes ampliar tus conocimientos al lado del servidor.

Aquí, el enfoque cambia, ya que obtenemos acceso a mucha más potencia computacional en comparación con las aplicaciones de navegador instaladas por el usuario. Es posible que las soluciones del lado del cliente no se traduzcan directamente al servidor, y la seguridad se vuelve primordial. Las aplicaciones de servidor a menudo interactúan con bases de datos que almacenan datos confidenciales, lo que requiere medidas de seguridad sólidas para proteger esta información.

Esta sección sienta las bases para explorar los matices del desarrollo del lado del servidor, equipándote con las habilidades necesarias para diseñar aplicaciones seguras y eficientes que impulsan la magia entre bastidores de la web.

#### Entendiendo el entorno del servidor

Al trabajar en el lado del servidor, tu código maneja solicitudes enviadas a través de un puerto de red (generalmente TCP/IP) por clientes como navegadores web o aplicaciones móviles. Los servidores pueden realizar diversas tareas, incluido el servicio de páginas web (servidores HTTP), microservicios internos que se comunican dentro de tu aplicación o herramientas especializadas como demonios (*daemons*, procesos de larga duración que se ejecutan en segundo plano).

Exploraremos algunas consideraciones importantes cuando se trata de trabajar en un entorno de servidor con TypeScript, comenzando por elegir el entorno de ejecución en la siguiente sección.

#### Elecciones de runtime — Node.js vs Deno vs Bun.js

Tradicionalmente, Node.js ha sido el entorno de ejecución dominante para el desarrollo de JavaScript del lado del servidor. Sin embargo, un par de nuevos marcos han surgido como una alternativa segura que puede evaluar de forma nativa el código TypeScript.

Como ejemplo, mostraremos un servidor HTTP simple construido con Deno y Bun.js. Proporcionamos enlaces a sus páginas de inicio para obtener instrucciones de instalación en la sección *Lecturas complementarias* de este capítulo.

El siguiente fragmento de código demuestra cómo configurar un servidor HTTP simple en Deno usando TypeScript. Ten en cuenta que no tienes que compilar el código ya que Deno entiende TypeScript de forma nativa:

`src/deno/server.ts`:
```typescript
const port = 8080; 
const handler = async (req: Request) => { 
    const body = "Hello, World!"; 
    return new Response(body, { status: 200 }); 
}; 
Deno.serve({ port }, handler); 
console.log(`HTTP server listening on port ${port}`);
```

Este código implementa un servidor HTTP básico utilizando Deno. Define un puerto (`8080`) y un controlador de solicitudes (`handler`) que crea una respuesta y un código de estado de 200 (OK). La función `Deno.serve` inicia el servidor en el puerto especificado y la función `handler` se utiliza para procesar las solicitudes entrantes.

Puedes ejecutar este servidor con el siguiente comando:

```bash
$ deno run --allow-net server.ts 
Listening on http://localhost:8080/ 
HTTP server listening on port 8080
```

El indicador `--allow-net` le indica al servidor que permita conexiones de red desde dominios externos.

Luego puedes navegar a `http://localhost:8000` y ver el mensaje (*Figura 2.8: Vista en el navegador del servidor Deno*).

He aquí un ejemplo similar de un servidor Bun respondiendo con `"Hello World!"`:

`src/bun/server.ts`:
```typescript
const server = Bun.serve({ 
    port: 3000, 
    fetch(request) { 
                return new Response("Hello World!") 
    }, 
}) 
console.log(`HTTP server listening on port:${server.port}`)
```

Este fragmento de código utiliza la función `Bun.serve` en su lugar para crear el servidor y configurar su puerto. La función `fetch` maneja las solicitudes entrantes de una manera minimalista similar.

La conclusión clave aquí es que ambos enfoques logran el mismo objetivo: establecer un servidor que escucha conexiones entrantes y responde a solicitudes de clientes a través de la red.

Es importante tener en cuenta que, si bien hemos utilizado TypeScript para la definición del código, es probable que el código de tiempo de ejecución real se transpile a JavaScript en función de la implementación interna del entorno elegido. Esto significa que el código de tiempo de ejecución en sí mismo podría ser de naturaleza más dinámica, con menos énfasis en la seguridad de tipos en comparación con el código fuente de TypeScript.

Ahora hablemos de otra característica de las aplicaciones de servidor, en relación con su ciclo de vida como procesos en ejecución.

#### Procesos de larga duración (*Long-living processes*)

Las aplicaciones de servidor están diseñadas para funcionar durante períodos prolongados, por lo que es fundamental garantizar su resistencia frente a errores y apagados inesperados. Esta sección explora estrategias para crear procesos de servidor resistentes en TypeScript.

##### Cómo proteger los procesos contra interrupciones

Un aspecto crucial es la gestión de recursos. Los procesos del servidor pueden ser intensivos en recursos, consumiendo memoria, CPU o almacenamiento significativos. La implementación de técnicas de supervisión y gestión de recursos es esencial para evitar el agotamiento de recursos y mantener la estabilidad general del sistema.

Otra capa de protección proviene de los gestores de procesos. Herramientas como PM2 o nodemon pueden actuar como guardianes de los procesos de tu servidor. En el desafortunado caso de una caída (*crash*), estas herramientas pueden reiniciar automáticamente el proceso, restaurando potencialmente el servicio y minimizando el tiempo de inactividad para tus usuarios.

El uso de gestores o monitores de procesos hará que tus aplicaciones sean más resistentes y puedan ofrecer mayores posibilidades de mantener la disponibilidad de tus servicios.

Si bien TypeScript puede proteger contra errores de tipo en tiempo de compilación, existen muchas clases de errores que ocurren en tiempo de ejecución. En tales casos, deseas poder capturar esos errores y responder en consecuencia. La siguiente sección explora esta opción.

##### Apagados elegantes y mantenimiento de un estado limpio

Si bien los gestores de procesos pueden manejar caídas inesperadas, puede haber situaciones en las que necesites iniciar un apagado controlado. Esto podría deberse a errores críticos que requieren la finalización de la aplicación o con fines de mantenimiento.

He aquí un ejemplo que utiliza la variable global `process` de Node.js, que es responsable de interactuar con el proceso de la aplicación actual:

`src/shutdown/server.ts`:
```typescript
import * as http from "http" 

const shutdownHandler = () => { 
    console.log("Received shutdown signal, gracefully exiting...") 
    // Perform additional cleaning work here 
    process.exit(0) // Indicate successful termination 
} 

//create a server object: 
http 
    .createServer((req: http.IncomingMessage, res: http.ServerResponse) => { 
        res.write("Hello World!") //write a response to the client 
        res.end() //end the response 
    }) 
    .listen(8080) //the server object listens on port 8080 

// Register shutdown signal listeners (SIGINT, SIGTERM) 
process.on("SIGINT", shutdownHandler) 
process.on("SIGTERM", shutdownHandler)
```

El código proporcionado registra manejadores de apagado para dos señales: `SIGINT` y `SIGTERM`. Envía la señal `SIGINT` usando el comando `kill` seguido del PID (Mac o Linux):

```bash
$ kill -INT <PID>
```

Por ejemplo, he aquí los comandos utilizados para averiguar el número de PID y terminar el proceso:

```bash
$ lsof -i :8080 
COMMAND   PID          USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME 
node    10416 theo.despoudis   22u  IPv6 0xc21ccc733b9504cf      0t0  TCP *:http-alt (LISTEN) 
$ kill -INT 10416 
# (en la pestaña que inició el ejemplo server.ts)
Received shutdown signal, gracefully exiting...
```

Este código identifica y finaliza un proceso de servidor Node.js que se ejecuta en el puerto 8080. El comando `lsof` con el indicador `-i` encuentra el ID de proceso (PID) según el número de puerto. Luego usa el comando `kill`, proporcionando el número de PID, lo que activará el manejador de apagado en nuestro programa. Esto demuestra cómo interactúa el sistema operativo con nuestra aplicación y qué sucede después.

> [!IMPORTANT]
> El envío de `SIGINT` (`Ctrl + C`) se utiliza normalmente para una terminación más suave, lo que permite que el proceso realice tareas de limpieza. `SIGTERM` es una señal de terminación más contundente y es posible que los procesos no tengan la oportunidad de realizar la limpieza.

Idealmente, debes diseñar cuidadosamente tus servicios teniendo en cuenta la ausencia de estado (*statelessness*). Eso significa que puedes y debes poder reiniciar esos procesos en cualquier momento sin pérdida de información. Al adherirse a buenas prácticas de ingeniería, como evitar estados globales o estructuras mutables que retengan datos indefinidamente, se te permite escalar servidores hacia arriba o hacia abajo sin efectos secundarios.

#### Frameworks con soporte para TypeScript

Una vez que exploras los fundamentos del scripting del lado del servidor con TypeScript, los frameworks pueden acelerar significativamente tu proceso de desarrollo. Estos frameworks proporcionan componentes prediseñados, mecanismos de enrutamiento, soporte de middleware y otras funcionalidades para simplificar la creación de aplicaciones sólidas del lado del servidor. Echaremos un vistazo breve a algunos de los más destacados.

##### Frameworks populares con TypeScript

El desarrollo del lado del servidor con TypeScript ofrece una variedad de frameworks para agilizar el proceso. Dos opciones destacadas incluyen las siguientes:

- **Express.js:** Este framework de Node.js bien consolidado y ligero proporciona una base flexible para construir aplicaciones web y APIs. Su enfoque minimalista permite un alto grado de personalización, lo que lo hace adecuado para diversos requisitos de proyectos.
- **Nest.js:** Construido sobre Express.js e inspirado en Angular, Nest.js es un framework progresivo del lado del servidor para Node.js. Aprovecha TypeScript para hacer cumplir la seguridad de tipos y promueve un enfoque de desarrollo estructurado. Las características clave de Nest.js incluyen la inyección de dependencias para administrar las dependencias de los objetos y la modularidad para organizar tu aplicación en componentes bien definidos.

Esta sección te proporciona un breve ejemplo de cada uno, brindándote una vista de alto nivel de lo que implica crear una aplicación web simple en TypeScript. Comencemos primero con Express.js y continuaremos con Nest.js.

##### Ejemplo de Express.js con TypeScript

Express.js es un framework web popular y minimalista para Node.js que simplifica la creación de aplicaciones web y APIs. Proporciona una base para manejar solicitudes HTTP entrantes, definir rutas, administrar middleware y estructurar la lógica de tu aplicación. Al aprovechar TypeScript junto con Express.js, obtienes los beneficios de la seguridad de tipos, una mantenibilidad mejorada y un mejor soporte de herramientas.

He aquí una versión simple que demuestra el enrutamiento y el middleware:

`src/express/server.ts`:
```typescript
import express from "express" 
import { json } from "body-parser" 

const app = express() 
const port = process.env.PORT || 3000 

app.use(json()) 

app.get("/health", (req, res) => { 
    res.send("OK!") 
}) 

app.listen(port, () => console.log(`Server listening on port ${port}`))
```

Este fragmento de código muestra la creación de una aplicación de servidor simple utilizando Express.js y TypeScript. La línea `app.use(json())` integra la función de middleware JSON con la aplicación Express. Esto asegura que cualquier solicitud entrante con un cuerpo JSON se analice automáticamente, haciendo que los datos sean accesibles en la propiedad `req.body` del objeto de solicitud.

Luego, el código define un controlador de ruta para el endpoint `/health` usando `app.get()`. Esto significa que el servidor responderá cada vez que reciba una solicitud GET a la ruta `/health`. La función del controlador de ruta simplemente envía una respuesta de texto, `"OK!"`, de vuelta al cliente.

Para ejecutar este ejemplo, puedes usar el siguiente comando después de ejecutar `npm run build`:

```bash
$ npm run build 
$ node chapters/chapter-2_Core_Principles_and_use_cases/dist/express/server.js
```

Luego, puedes navegar al endpoint `http://localhost:3000/health` para ver el mensaje `OK!` (*Figura 2.9: Página de verificación de estado del servidor Express.js*).

Veamos cómo funciona Nest.js con la misma funcionalidad.

##### Ejemplo de Nest.js con TypeScript

Nest.js ofrece un enfoque más estructurado para crear aplicaciones del lado del servidor. He aquí un ejemplo básico que demuestra un controlador y una ruta. El ejemplo se divide en tres archivos, que es prácticamente el producto mínimo viable para una aplicación web simple escrita en este framework:

`src/nest/app.module.ts`:
```typescript
import { Module } from '@nestjs/common'; 
import { AppController } from './app.controller'; 

@Module({ 
    imports: [], 
    controllers: [AppController], 
}) 
export class AppModule {}
```

`src/nest/app.controller.ts`:
```typescript
import { Controller, Get } from '@nestjs/common'; 

@Controller() 
export class AppController { 
    @Get('/health') 
    getHealth() { 
        return 'OK!'; 
    } 
}
```

`src/nest/main.ts`:
```typescript
import { ValidationPipe } from "@nestjs/common" 
import { NestFactory } from "@nestjs/core" 
import { AppModule } from "./app.module" 

async function bootstrap() { 
    const app = await NestFactory.create(AppModule) 
    app.useGlobalPipes(new ValidationPipe()) 
    await app.listen(3000) 
    console.log(`Application is running on: ${await app.getUrl()}`) 
} 
bootstrap()
```

Exploremos cómo funciona el ejemplo de Nest.js y destaquemos las diferencias clave en la estructura en comparación con Express.js:

- **Módulos:** Nest.js gira en torno al concepto de módulos. Estos módulos encapsulan la funcionalidad y proporcionan una forma de organizar la lógica de tu aplicación. Debes declarar cualquier dependencia de módulo utilizando el campo `imports` y cualquier controlador utilizando el campo `controllers`.
- **Controladores:** Los controladores son fragmentos de código predefinidos que aceptan solicitudes entrantes y ejecutan cierta lógica sobre ellas. Esto es similar a las funciones de callback asociadas a una ruta en particular. Cuando el usuario visita una ruta específica, el controlador se activa. Cuando decoras una clase con la anotación `@Controller`, la marcas como una clase válida para usar en el campo `controllers` cuando declaras un módulo.
- **Inicialización (*Bootstrapping*):** Se refiere al proceso de inicializar e iniciar una aplicación Nest.js. Implica crear una instancia de aplicación, definir configuraciones y registrar los componentes necesarios.

Si bien ambos frameworks manejan el desarrollo del lado del servidor, Nest.js proporciona un enfoque más estructurado y con opiniones firmes (*opinionated*), con características integradas como la inyección de dependencias y la modularidad. Express.js ofrece un enfoque más flexible, lo que permite una mayor personalización. La elección entre ellos depende de los requisitos y preferencias de tu proyecto.

#### Manejo de errores

El manejo de errores es el proceso de implementar políticas e instrucciones lógicas cuando una aplicación encuentra fallos o argumentos no válidos. Dado que los entornos de servidor son muy sensibles y críticos, es fundamental manejar los errores inesperados de la manera más adecuada y elegante.

Exploraremos varias técnicas para manejar errores en TypeScript, considerando diferentes entornos de ejecución como Node.js, Deno y Bun.js.

##### Clases de error personalizadas

TypeScript nos permite crear clases de error personalizadas que extienden la clase integrada `Error`. Este enfoque ayuda a crear tipos de error más específicos y significativos que proporcionan un mensaje especializado para fines de depuración o introspección:

`Errors.ts`:
```typescript
class DatabaseConnectionError extends Error { 
    constructor(message: string) { 
        super(message) 
        this.name = "DatabaseConnectionError" 
    } 
} 

try { 
    throw new DatabaseConnectionError("Unable to connect to the database.") 
} catch (error) { 
    if (error instanceof DatabaseConnectionError) { 
        console.error(error.message) // Output: Unable to connect to the database. 
    } 
}
```

Aquí, `DatabaseConnectionError` extiende la clase `Error` y establece una propiedad `name` única para ella. La idea principal de esto es que si lanzas una instancia de esta clase, más adelante podrás verificar su tipo de instancia (usando el operador `instanceof`) y realizar comprobaciones de limpieza adicionales según ese tipo específico de error.

##### Uso de tipos de unión para el manejo de errores

Por supuesto, si prefieres no utilizar clases, puedes aprovechar alternativamente los tipos de unión para crear tipos de error. La idea principal es codificar los casos de éxito y de error como tipos y luego utilizar el sistema de tipos para verificar cada caso de uso al realizar comprobaciones:

`errors.ts`:
```typescript
type SuccessResponse = { 
    success: true; 
    value: number 
} 

type ErrorResponse = { 
    success: false; 
    error: string 
} 

function divide(dividend: number, divisor: number): SuccessResponse | ErrorResponse { 
    if (divisor === 0) { 
        return { success: false, error: "Cannot divide by zero." } 
    } 
    return { success: true, value: dividend / divisor } 
} 

const result = divide(10, 0) 

if (result.success) { 
    console.log("Division result:", result.value) 
} else { 
    console.error("Division error:", result.error) // Output: Division error: Cannot divide by zero. 
}
```

Define dos tipos: `SuccessResponse` para cálculos exitosos y `ErrorResponse` para errores. La función `divide` toma dos números y devuelve uno de estos tipos de respuesta. Comprueba si el divisor es cero, devolviendo una respuesta de error si es verdadero, o una respuesta de éxito con el resultado de la división en caso contrario. El tipo de retorno de la función utiliza una unión de estas respuestas, aprovechando las uniones discriminadas de TypeScript.

Por supuesto, este enfoque, aunque proporciona seguridad de tipos y gestión explícita de errores, requiere una verificación manual de los estados de error en cada paso, lo que conduce a un estilo de programación más procedimental. A diferencia de las excepciones que se propagan automáticamente por la pila de llamadas (*call stack*), los errores en este sistema deben pasarse explícitamente a través de llamadas a funciones. Este enfoque obliga a los desarrolladores a manejar conscientemente los posibles errores, lo que mejora la fiabilidad pero potencialmente hace que el código sea más detallado (*verbose*) como efecto secundario.

##### Manejo centralizado de errores

Para aplicaciones más grandes, especialmente aquellas que utilizan frameworks, centralizar el manejo de errores puede mejorar la mantenibilidad y garantizar respuestas de error consistentes en toda la aplicación. La idea principal es tener una clase global o una función que maneje un conjunto común de errores para toda la aplicación:

```typescript
export function errorHandler(err, req, res, next) { 
    res.status(500).json({ error: err.message }); 
} 

import { globalErrorHandler } from "./error-handler"; 

app.use(globalErrorHandler);
```

Aquí, este middleware global de manejo de errores (a través del objeto `errorHandler`) proporciona una forma centralizada de gestionar errores en una aplicación Express.js/Bun.js, asegurando respuestas de error coherentes en toda la aplicación.

Sin embargo, también es bastante genérico (*coarse-grained*), lo que significa que solo es útil para manejar el caso de uso general de errores (códigos de error 500, por ejemplo). Si deseas manejar tipos específicos de errores o proporcionar información detallada sobre el error, lo cual podría ser crucial para la depuración, entonces necesitarás incluir sentencias `try/catch` dentro de los controladores de la API.

Ahora que sabes cómo trabajar con TypeScript y has explorado su ecosistema, comenzarás a aprender más sobre los patrones de diseño para comprender su practicidad.

---

### Introducción a los patrones de diseño en TypeScript

Los patrones de diseño son soluciones probadas en batalla que ofrecen enfoques reutilizables para desafíos comunes en el desarrollo de software.

Aunque los patrones de diseño existen desde 1994 (¡el libro de la Banda de los Cuatro!), sus principios fundamentales siguen siendo relevantes. No deben considerarse reliquias obsoletas, sino herramientas valiosas para evaluar a través del prisma de los lenguajes de programación modernos y las mejores prácticas. Estos patrones son el resultado de la experiencia al trabajar en sistemas grandes y los futuros desarrolladores deberían invertir su tiempo en comprender sus orígenes y su razón de ser (*raison d'être*).

#### ¿Por qué existen los patrones de diseño?

El desarrollo de software es como un vasto paisaje con muchos caminos a seguir. A menudo existen múltiples formas de llegar a una solución, y algunos enfoques son demostrablemente mejores que otros. Los patrones de diseño proporcionan una forma estructurada y repetible de abordar problemas recurrentes. Nos ayudan a evitar errores como el alto acoplamiento, la baja cohesión y la gestión ineficiente de recursos. Nos ayudan a encontrar la ruta óptima para lograr nuestros objetivos minimizando la complejidad del código.

La programación orientada a objetos (POO) y la programación funcional ofrecen bases sólidas para construir sistemas a gran escala. Pero a veces, se necesitan experiencia y pragmatismo para moldear el código existente en estructuras más elegantes y adaptables. Los patrones de diseño proporcionan un marco básico para lograrlo al enseñarnos cómo diseñar sistemas que eviten problemas comunes.

#### ¿Siguen siendo relevantes los patrones de diseño ahora?

En el momento de escribir este libro, los patrones de diseño siguen siendo relevantes, pero su aplicación ha evolucionado junto con los avances en lenguajes como TypeScript. He aquí el porqué:

- **Adaptabilidad a los lenguajes modernos:** Si bien algunos patrones pueden necesitar ajustes para las características de TypeScript, los conceptos centrales siguen siendo aplicables. Las interpretaciones modernas de los patrones de diseño aprovechan estas características para crear soluciones más limpias y con mayor seguridad de tipos. Por ejemplo, la forma en que instanciamos objetos en TypeScript ha cambiado. Dependiendo de cómo queramos que se comporten esos objetos, tenemos flexibilidad para crearlos. Sin embargo, aún podemos abstraer toda la creación de objetos utilizando el patrón Factory y seguir obteniendo los mismos beneficios.
- **Terminología común:** Los patrones de diseño proporcionan un lenguaje común para los desarrolladores. La comprensión de estos patrones te permite comunicar ideas de diseño de manera efectiva con tu equipo y comprender el código escrito utilizando estos patrones en otros proyectos. Por ejemplo, los desarrolladores a menudo necesitan asegurarse de que solo exista una instancia de un objeto en el sistema en un momento dado, por lo que tiene sentido utilizar el término Singleton para describir este requisito.
- **Conocimiento fundamental:** El aprendizaje de patrones de diseño fortalece tus habilidades para resolver problemas y tu comprensión de los principios de diseño de software. Este conocimiento sirve como un paso fundamental para explorar conceptos arquitectónicos más avanzados y consideraciones de diseño en el desarrollo de software moderno. Esta comprensión es crucial para captar conceptos como la programación reactiva (cubierta en el Capítulo 8). Si bien la programación reactiva puede parecer compleja al principio, una base sólida en patrones de diseño te preparará para conectar los puntos. Verás cómo estos patrones juegan un papel en la construcción de sistemas reactivos que manejan eficientemente flujos de datos y operaciones asíncronas.

Al aprovechar la seguridad de tipos, la comunicación clara y una base sólida para conceptos avanzados, los patrones de diseño te permiten construir aplicaciones robustas, mantenibles y escalables. TypeScript es un lenguaje excelente para comenzar a practicar estos principios y explicamos cómo hacerlo en los siguientes capítulos.

#### Patrones de diseño en TypeScript

Esto nos lleva al esfuerzo principal de este libro sobre cómo aplicar los patrones de diseño clásicos a TypeScript como un primer paso natural. El sistema de tipos moderno y expresivo de TypeScript ayuda a superar las limitaciones encontradas en lenguajes más antiguos como C++ y Java, lo que permite un código más limpio y optimizado.

A lo largo de este libro, exploraremos enfoques tanto clásicos como modernos de los patrones de diseño. Examinaremos si un patrón específico conserva su valor en el contexto de un lenguaje más expresivo como TypeScript. Nuestro objetivo es dotarte de los conocimientos necesarios para evaluar cuándo y cómo aplicar los patrones de diseño de manera eficaz.

Seguiremos un enfoque estructurado para cada patrón: primero, definiendo claramente el problema que resuelve. Luego, entraremos en la solución propuesta, explicando cómo aborda el problema y sus beneficios clave. Para mejorar la comprensión, los diagramas del Lenguaje Unificado de Modelado (UML) visualizan la estructura del patrón. Los ejemplos de código mostrarán implementaciones prácticas, seguidas de debates sobre alternativas o variaciones modernas que podrías considerar. Consolidaremos tus conocimientos explorando casos de uso concretos donde cada patrón es valioso. Finalmente, para brindar una perspectiva integral, reconoceremos cualquier inconveniente o crítica potencial asociada con cada patrón de diseño.

Como nota útil, es importante recordar que los patrones de diseño no son una solución mágica. Deben aplicarse cuidadosamente según el contexto específico de tu proyecto y sus necesidades. Este libro te guiará a través de la evaluación de cuándo y cómo aprovechar los patrones de diseño de manera efectiva en tus proyectos TypeScript.

---

### Resumen

Este capítulo exploró primitivas de lenguaje avanzadas en TypeScript que te permiten definir tipos precisos dentro de tus abstracciones. Los tipos de utilidad como `Pick`, `Record` y `Partial` son tipos integrados que te permiten proporcionar tipos exactos; mejorarás la seguridad de tu código.

Además, destacamos la versatilidad de TypeScript como un lenguaje multiparadigma. Proporcionamos ejemplos de cómo se puede configurar TypeScript para ejecutarse tanto en entornos de servidor como de navegador utilizando empaquetadores o entornos de ejecución que ofrecen soporte nativo para TypeScript.

Frameworks como Next.js y Express.js brindan un excelente soporte para TypeScript, desbloqueando la seguridad de tipos en toda la base de código de la aplicación.

Se introdujeron los patrones de diseño: soluciones establecidas y confiables para problemas comunes de software. Estos patrones, creados por expertos en software, ofrecen herramientas valiosas para gestionar la complejidad en proyectos a gran escala, independientemente del estilo de programación elegido (POO u otros).

El siguiente capítulo profundiza en los patrones de diseño, comenzando con los patrones creacionales. Obtendrás una comprensión integral de estas herramientas esenciales para mejorar tus habilidades de desarrollo de software.

---

### Preguntas y respuestas

Siéntete libre de revisar las siguientes preguntas y sus respuestas correspondientes para resolver cualquier duda u obtener información adicional:

1. **¿Cuándo podrías usar `Pick<T, K>` u `Omit<T, K>`?**
   - **Respuesta:** `Pick<T, K>` crea un nuevo tipo que incluye solo un conjunto específico de propiedades (`K`) del tipo original `T`. `Omit<T, K>` crea un nuevo tipo que excluye un conjunto específico de propiedades (`K`) del tipo original `T`. Estos tipos de utilidad son útiles para seleccionar datos específicos de objetos o eliminar propiedades no deseadas.

2. **¿Cómo afecta la opción `target` en `tsconfig.json` a la compilación para navegador frente a la del lado del servidor?**
   - **Respuesta:** La opción `target` especifica la versión de ECMAScript a la que debe apuntar tu código JavaScript compilado. Para entornos de navegador, puedes elegir un objetivo inferior para garantizar la compatibilidad con navegadores más antiguos. Para entornos del lado del servidor con Node.js, puedes utilizar una versión de destino más moderna.

3. **¿Cómo controla TypeScript la compatibilidad del navegador al compilar para producción?**
   - **Respuesta:** El código TypeScript debe compilarse a JavaScript antes de que los navegadores puedan entenderlo. Los empaquetadores como Webpack y Vite pueden ayudar a combinar y transpilar tu código TypeScript, junto con otras dependencias, en un único archivo JavaScript que funcione en todos los navegadores con el nivel de soporte adecuado.

---

### Lecturas complementarias

- La especificación de ECMAScript 5 se encuentra en: https://262.ecma-international.org/5.1/
- La especificación de ECMAScript 2015 (ES6) se encuentra en: https://262.ecma-international.org/6.0/
- Para obtener una guía más avanzada de TypeScript, lee *Mastering TypeScript 4th Edition*, Nathan Rozentals, Packt Publishing. Está disponible en: https://www.packtpub.com/product/mastering-typescript-fourth-edition/9781800564732
- Consulta el libro clásico de patrones de diseño, *Design Patterns: Elements of Reusable Object-Oriented Software*, Erich Gamma, Richard Helm, Ralph Johnson y John Vlissides, Addison-Wesley. Está disponible en: https://www.amazon.co.uk/Design-patterns-elements-reusable-object-oriented/dp/0201633612
- Para obtener más información sobre Nest.js, consulta la documentación oficial en: https://docs.nestjs.com/
- Para obtener más información sobre Bun.js, consulta la documentación oficial en: https://bun.sh/
