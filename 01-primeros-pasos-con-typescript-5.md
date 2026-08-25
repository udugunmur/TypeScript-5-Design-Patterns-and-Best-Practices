# Parte 1: Introducción a TypeScript 5

## Capítulo 1: Primeros pasos con TypeScript 5

¡Bienvenido a la edición actualizada de *TypeScript 4 Design Patterns and Best Practices*! Basándose en el éxito de la primera edición (publicada por primera vez en 2021), este libro incorpora los valiosos comentarios de los lectores y una gran cantidad de contenido nuevo. Ya sea que estés familiarizado con los patrones de diseño del libro de la Banda de los Cuatro (*Gang of Four* / GoF) o que seas completamente nuevo en el concepto, este libro es tu guía completa para aprovechar TypeScript 5 en la creación de aplicaciones robustas y escalables. Esto es lo que obtendrás:

- **Domina el arte de los patrones de diseño en TypeScript:** Profundizaremos tanto en la teoría como en la implementación práctica de patrones de diseño arquitectónicos específicamente en el contexto de TypeScript.
- **Aprovecha el poder de TypeScript 5:** Aprende a utilizar las últimas características de TypeScript 5 para crear estructuras de código eficientes y mantenibles para tus patrones de diseño.
- **Mejores prácticas modernizadas:** Explora principios de diseño efectivos y mejores prácticas adecuadas para el panorama moderno del desarrollo con TypeScript.

El estudio de patrones cumple múltiples propósitos importantes. En primer lugar, proporciona un lenguaje moderno y concreto para comprender los patrones de diseño, donde TypeScript actúa como un marco de trabajo potente y ampliamente utilizado. En segundo lugar, el énfasis radica en implementaciones frescas en lugar de depender de ejemplos obsoletos o genéricos. Por último, estudiar patrones capacita a las personas no solo para aplicar estos principios de diseño, sino también para adaptarlos y mejorarlos utilizando las mejores prácticas modernas. Esto garantiza que su código se mantenga eficiente y actualizado. El libro prioriza las aplicaciones prácticas que siguen las características y capacidades más recientes de TypeScript, asegurando relevancia y aplicabilidad en escenarios del mundo real.

Los dos primeros capítulos proporcionan una base sólida en TypeScript 5. Te proporcionarán los conocimientos esenciales para trabajar eficazmente con los ejemplos de código a lo largo del libro. Tras esta configuración fundamental, nos sumergiremos en los patrones de diseño, explorándolos uno a uno junto con su implementación práctica en TypeScript. Posteriormente, exploraremos patrones de diseño arquitectónicos y mejores prácticas que te ayudarán a construir aplicaciones robustas, escalables y mantenibles en TypeScript.

En este capítulo, cubriremos los siguientes temas principales:

- Introducción a TypeScript 5
- Explorando características útiles de TypeScript 5
- Comprendiendo la relación entre TypeScript y JavaScript
- Configuración del entorno de desarrollo
- Uso de VSCode con TypeScript
- Introducción al Lenguaje Unificado de Modelado (UML)

Al comprender estas potentes herramientas, podrás estructurar tu código de manera eficiente, promover la reutilización y garantizar el éxito del proyecto a largo plazo.

¡Empecemos!

> [!NOTE]
> Los enlaces a todas las fuentes mencionadas en este capítulo, así como a los materiales de lectura complementarios, se proporcionan en la sección *Lecturas complementarias*, hacia el final de este capítulo.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter01_Getting_Started_With_Typescript_5

En la sección *Configuración del entorno de desarrollo*, discutiremos cómo instalar y usar los ejemplos de código de este libro. Primero, refresquemos nuestros conocimientos sobre TypeScript, especialmente sobre su última versión.

---

### Introducción a TypeScript 5

TypeScript continúa la larga historia de seguridad de tipos del lenguaje, aportando una gran cantidad de mejoras a la experiencia del desarrollador. Aunque no es una versión revolucionaria en términos de funcionalidad central, TypeScript 5 se centra en el refinamiento y la optimización.

En esta sección, veremos un desglose rápido de algunas características clave de TypeScript introducidas en las versiones 4 y 5. Para TypeScript 4, exploramos algunas de esas características en la edición anterior del libro:

- **Tipos de literales de plantilla (*Template literal types*):** Seguridad de tipos mejorada para plantillas de cadenas literales, lo que permite un control más preciso sobre la manipulación de cadenas.
- **Tipos mapeados mejorados (*Enhanced mapped types*):** Manipulación más potente de tipos de objetos utilizando tipos mapeados.
- **Parámetros de tipo constantes (*Const type parameters*):** Seguridad de tipos mejorada para funciones genéricas al permitir valores constantes para los parámetros de tipo.
- **Palabra clave `override`:** Marca explícitamente los métodos que sobrescriben un método de la clase base, mejorando la claridad del código y detectando posibles errores.
- **Soporte mejorado para BigInt:** Mejor manejo de valores BigInt, una nueva primitiva de JavaScript para enteros grandes.

En cuanto a TypeScript 5, exploraremos las siguientes características más adelante en este capítulo:

- **Resolución simplificada de módulos:** Proceso de resolución de módulos simplificado para una experiencia de desarrollo más fluida.
- **Soporte de decoradores:** Mejor soporte para decoradores, una potente característica sintáctica para agregar metadatos al código.
- **Parámetros de tipo `const`:** Mejora de la inferencia de tipos para funciones al preservar los tipos literales de los objetos pasados como argumentos.
- **Manejo mejorado de enumeraciones (*enums*):** Todas las enumeraciones ahora se consideran enumeraciones de unión (*union enums*) de forma predeterminada, lo que mejora la seguridad y la flexibilidad de tipos.

Si bien TypeScript ofrece un rico conjunto de características, priorizaremos los conceptos que contribuyen directamente a comprender e implementar patrones de diseño de manera efectiva. Aunque usar la inferencia con parámetros de tipo `const` es conveniente, en términos prácticos solo sirve para mejorar la seguridad de tipos y no para diseñar programas. Por esta razón, estas características, aunque valiosas para el desarrollo general, pueden agregar complejidad cuando nuestro objetivo principal es dominar los patrones de diseño.

En su lugar, el libro utiliza ejemplos autocontenidos diseñados para el aprendizaje independiente y la consulta. Esto te permite captar la funcionalidad central de cada patrón de diseño en su forma más simple. Al centrarte en la implementación práctica sin características superfluas del lenguaje, obtendrás una base sólida en patrones de diseño que se puede aplicar fácilmente en tus proyectos de TypeScript.

#### Fundamentos de TypeScript

Una base sólida en los fundamentos de TypeScript es crucial para aprender y aplicar eficazmente los patrones de diseño. He aquí el porqué:

- **Calidad y mantenibilidad del código mejoradas:** TypeScript introduce el tipado estático, lo que te permite definir los tipos de datos que contendrán tus variables y funciones. Esto ayuda a detectar errores al principio del proceso de desarrollo, evitando problemas como asignar un valor de cadena a una variable que espera un número. Al comprender los tipos básicos y las anotaciones de tipos, puedes aprovechar el sistema de tipos de TypeScript para crear un código más robusto y mantenible, especialmente cuando trabajas con patrones de diseño complejos.
- **Refactorización efectiva:** La refactorización es el proceso de reestructurar el código existente para mejorar su diseño, legibilidad y mantenibilidad sin cambiar su comportamiento externo. La mayoría de las herramientas e IDEs cuentan con tareas integradas que nos permiten realizar tareas de refactorización. El sistema de tipos de TypeScript asiste en la refactorización del código, lo cual suele ser necesario al implementar patrones de diseño. A medida que modificas la estructura de tu código para implementar patrones de diseño, la seguridad de tipos ayuda a garantizar que los cambios sigan siendo consistentes y no introduzcan efectos secundarios no deseados. Esto protege tu base de código y simplifica futuras modificaciones.

Ahora, pasaremos a comprender los tipos básicos en la siguiente sección.

#### Bloques de construcción de TypeScript – Tipos básicos

Los programas de TypeScript se construyen mediante sentencias (*statements*) o expresiones (*expressions*). Ejemplos de sentencias incluyen declaraciones de variables, sentencias de control de flujo (como bucles `if` o `for`) y llamadas a funciones. Una expresión, por otro lado, es un fragmento de código que se evalúa como un valor.

> [!NOTE]
> Las sentencias realizan tareas como asignaciones, impresión de salidas o control de flujo (`if`/`else` y bucles). No producen valores directamente. Las expresiones, en cambio, evalúan un resultado (cálculos, referencias a variables y llamadas a funciones) que las sentencias pueden utilizar posteriormente. Piensa en las sentencias como dar órdenes, mientras que las expresiones suministran los datos para esas órdenes.

Las sentencias o expresiones suelen operar sobre datos, y ahí es donde entran los tipos básicos. Los tipos básicos definen los bloques fundamentales de construcción para representar datos en tu código TypeScript. Son como los tipos de datos básicos presentes en JavaScript, pero TypeScript añade el beneficio de la comprobación estática de tipos, lo que ayuda a detectar errores en etapas tempranas del desarrollo.

Antes de explorar los conceptos básicos de TypeScript, sería beneficioso presentar los símbolos y la terminología clave en los capítulos iniciales del libro. He aquí una introducción rápida:

- **Símbolo de tubería (`|`):** Utilizado para indicar tipos de unión (*union types*), permitiendo que una variable contenga múltiples tipos.
- **Símbolo et (`&`):** Utilizado para indicar tipos de intersección (*intersection types*), combinando múltiples tipos en uno.
- **Signo de interrogación (`?`):** Indica propiedades opcionales en los tipos.
- **Corchetes (`[]`):** Denota tipos de matriz (*array types*).

Ahora hagamos un desglose de algunos tipos básicos comunes en TypeScript:

- **Tipos primitivos:** Son los tipos de datos más fundamentales que representan valores básicos. Algunos ejemplos incluyen los siguientes:
  - `number`: Representa valores numéricos (por ejemplo, `10` o `3.14`).
  - `string`: Representa datos textuales entre comillas (por ejemplo, `"Hello, World!"`).
  - `boolean`: Representa valores `true` o `false`.
  - `void`: Representa la ausencia de un valor (por ejemplo, funciones que no devuelven nada).
- **Otros tipos básicos:** TypeScript ofrece tipos básicos adicionales más allá de los primitivos:
  - `null`: Representa la ausencia intencionada de un valor de objeto (por ejemplo, `let value: null = null;`).
  - `undefined`: Representa una variable que ha sido declarada pero a la que aún no se le ha asignado un valor (por ejemplo, `let value: string;`).
  - `unknown`: Representa un valor que tiene un tipo desconocido en tiempo de compilación.
  - `any`: Este tipo desactiva esencialmente la comprobación de tipos, ya que representa cualquier cosa que no se pueda tipar; nuestro objetivo es reducir el uso de este tipo tanto como sea posible.
  - `never`: Normalmente se utiliza para funciones que nunca retornan (o lanzan un error).

TypeScript ofrece una potente característica llamada seguridad de tipos (*type safety*). Esto significa que las variables y otros datos tienen tipos asociados que definen qué clase de valores pueden contener. Es como etiquetar una caja para describir su contenido. Para aprovechar la seguridad de tipos de TypeScript, puedes definir explícitamente el tipo de una variable al declararla. He aquí un ejemplo:

`intro.ts`:
```typescript
const one: string = "one" 
const two: boolean = false 
const three: number = 3 
const four: null = null 
const five: unknown = 5 
const six: any = 6 
const seven = Symbol("seven") 
function neverReturningFunction(): never { 
    throw new Error("This function never returns") 
} 
// let eight: never;
```

Este ejemplo de código muestra varios tipos básicos en TypeScript. Tenemos tipos primitivos como `string`, `number` y `boolean` para datos comunes. Además, existen tipos especiales como `null` para la ausencia intencionada de un valor y `unknown` para variables con tipos desconocidos. Aunque `any` permite asignar cualquier valor, elude la comprobación de tipos, por lo que debe usarse con moderación. Los símbolos son identificadores únicos y `never` se utiliza normalmente para funciones que nunca retornan o lanzan un error (demostrado en la función `neverReturningFunction`). Vale la pena señalar que a algunos tipos como `never` no se les pueden asignar valores directamente.

- **Enumeraciones (*Enums*):** Las enumeraciones definen conjuntos de constantes con nombre para mejorar la legibilidad del código, la mantenibilidad y la seguridad de tipos. De forma predeterminada, los enums crean constantes con valores numéricos que comienzan en `0` y se incrementan en `1` para cada miembro subsiguiente:

`enum.ts`:
```typescript
enum Direction { 
    Up = 0, 
    Down, // Implicitly assigned 1 
    Left, // Implicitly assigned 2 
    Right, // Implicitly assigned 3 
} 
let userDirection: Direction = Direction.Up
```

TypeScript 5 introdujo las enumeraciones de unión (*union enums*), una potente característica que eleva la seguridad de tipos al crear un tipo único para cada miembro dentro de un enum. El siguiente ejemplo muestra su uso:

`enum.ts`:
```typescript
const prefix = '/data'; 
const enum Routes { 
    Parts = `${prefix}/parts`, // "/data/parts" 
    Invoices = `${prefix}/invoices`, // "/data/invoices" 
}
```

Anteriormente, los enums se dividían entre tipos de enum numéricos y literales separados, lo que causaba confusión. Ahora, los enums son uniones de sus tipos miembro. Los miembros pueden tener valores calculados mediante expresiones (constantes o no constantes) y aquellos con valores constantes (como números o cadenas) obtienen tipos literales para una mayor seguridad de tipos. ¡Esto mejora la claridad del código y hace que los enums sean más potentes!

- **Matrices y tuplas (*Arrays and tuples*):** TypeScript ofrece dos formas de representar colecciones de datos: matrices (*arrays*) y tuplas (*tuples*). Entendámoslas mejor:
  - Los **arrays** representan una colección de elementos del mismo tipo (por ejemplo, `number[]`). Pueden tener un tamaño variable, lo que significa que puedes añadir o eliminar elementos tras su creación. Un ejemplo sería `const numbers: number[] = [1, 2, 3];`.
  - Las **tuplas** representan un array de longitud fija donde cada elemento tiene un tipo específico. Ofrecen seguridad de tipos al imponer el número de elementos y sus tipos individuales. Un ejemplo sería `const student: [string, number] = ["Alice", 25];`.

Elige arrays cuando necesites una colección de tamaño variable o cuando los elementos puedan cambiar dinámicamente. Por otro lado, usa tuplas cuando conozcas de antemano la estructura exacta y los tipos de elementos de tu colección. Las tuplas proporcionan una mejor seguridad de tipos y evitan discordancias accidentales.

- **Clases:** Las clases en TypeScript representan planos para crear objetos. Encapsulan datos (propiedades) y funcionalidad (métodos) dentro de una estructura bien definida. TypeScript admite dos tipos de clases:
  - **Clases concretas:** Son clases estándar que se pueden instanciar directamente para crear objetos. Definen propiedades y métodos que implementan el comportamiento deseado. He aquí un ejemplo de una clase concreta:

`classes.ts`:
```typescript
class User { 
    constructor(private readonly name: string) { 
        this.name = name 
    } 
    public getName(): string { 
        return this.name 
    } 
} 
const user = new User("Theo") 
console.log(user.getName()) // Output: "Theo"
```

Este código define una clase concreta llamada `User` en TypeScript. La clase representa un objeto de usuario con una propiedad privada `name` y un método público `getName`.

  - **Clases abstractas:** Sirven como plantillas para subclases y no se pueden instanciar directamente. Definen métodos abstractos (sin implementación) que las subclases deben implementar para proporcionar una funcionalidad específica. Esto impone una interfaz común en todas las clases derivadas. He aquí un ejemplo de una clase abstracta:

`classes.ts`:
```typescript
abstract class Animal { 
    abstract makeSound(): void; 
} 
class Dog extends Animal { 
    makeSound(): void { 
        console.log("Woof!"); 
    } 
} 
new Dog().makeSound();
```

Este código define una clase abstracta llamada `Animal` que sirve como modelo para crear tipos de animales. Tiene un método abstracto, `makeSound`, que debe ser implementado por cualquier clase que herede de `Animal`. Esto obliga a que las subclases definan cómo manejan este método.

- **Interfaces y tipos:** TypeScript proporciona dos herramientas potentes para definir estructuras de objetos: interfaces y tipos:
  - **Interfaces:** Son abstracciones que te permiten definir la forma de un objeto y sus propiedades, pero sin especificar una implementación. Por ejemplo, podemos definir una interfaz `Comparable` de esta manera:

`interfaces.ts`:
```typescript
export interface Comparable<T> { 
    compareTo(other: Comparable<T>): -1 | 0 | 1; 
}
```

Esta interfaz define que cualquier objeto que implemente `Comparable` debe tener un método `compareTo`. Este método toma otro objeto del mismo tipo (`T`) y devuelve un número que indica el resultado de la comparación (`-1` para menor que, `0` para igual, `1` para mayor que).

Las interfaces juegan un papel crucial en la promoción de la claridad y mantenibilidad del código. Imponen una estructura consistente a través de los objetos que las implementan, facilitando la comprensión y el razonamiento sobre tu código. Además, las interfaces permiten el bajo acoplamiento: las clases pueden implementar la interfaz sin depender de los detalles de implementación específicos.

  - **Tipos (*Types*):** Los tipos proporcionan otro enfoque para definir estructuras de objetos. Son como las interfaces pero ofrecen más flexibilidad. Los tipos pueden actuar como alias para tipos de datos complejos, mejorando la legibilidad del código. Además, permiten combinar tipos existentes mediante uniones (OR) e intersecciones (AND) para definiciones de tipos más específicas:
    - **Tipos de unión (OR):** Combinan múltiples tipos con el símbolo de tubería (`|`). El tipo resultante puede ser cualquiera de los tipos combinados. He aquí un ejemplo:

`types.ts`:
```typescript
type A = 'A'; 
type B = 'B'; 
type C = A | B; // C can be either 'A' or 'B'
```

> [!WARNING]
> **Evita el uso de tipos de unión con tipos incompatibles:**
> Al usar tipos de unión, asegúrate de que los tipos combinados sean compatibles. Por ejemplo, si combinas tipos que no tienen relación lógica, podría generar confusión y mayor complejidad en el código. He aquí un ejemplo:
> ```typescript
> type D = 'A' | 40; // D can be either 'A' or a number
> ```
> En este caso, `D` puede ser el carácter `'A'` o un número, lo que puede requerir comprobaciones y conversiones exhaustivas de tipos en tu código. Esta elección puede crear confusión y hacer que el código sea más difícil de mantener.

    - **Tipos de intersección (AND):** Combinan múltiples tipos con el símbolo et (`&`). El tipo resultante debe compartir todas las propiedades de ambos tipos originales. He aquí un ejemplo:

`types.ts`:
```typescript
type User = { 
    name: string 
} 
type ExtendedUser = User & { 
    age: number 
} 
let user: ExtendedUser = { 
    name: "Theo", 
    age: 20, 
}
```

**Interfaces frente a tipos:**
Al trabajar con tipos en TypeScript, prioriza el uso de interfaces para establecer contratos claros para los objetos. Las interfaces definen su estructura y propiedades, haciendo que el código sea más fácil de entender y mantener. Sin embargo, para la creación de tipos sobre la marcha o la combinación de tipos existentes con mayor flexibilidad, TypeScript también ofrece tipos (*types*). Esto permite un control dinámico sobre los tipos de datos cuando tus necesidades van más allá de lo que las interfaces pueden proporcionar.

- **Genéricos (*Generics*):** Los genéricos nos permiten definir funciones, clases e interfaces que pueden trabajar con cualquier tipo de datos manteniendo la seguridad de tipos. Esto significa que podemos escribir código adaptable a varios tipos sin perder los beneficios del tipado estático de TypeScript.

La sintaxis para definir un tipo genérico implica el uso de corchetes angulares (`< >`) para especificar un parámetro de tipo. He aquí una estructura básica:

`types.ts`:
```typescript
function callMe<T>(parameter: T): T { 
    return parameter; 
}
```

En este ejemplo, `T` es un marcador de posición para cualquier tipo. Cuando se llama a la función, `T` se reemplaza con el tipo real pasado como argumento.

Los genéricos también se pueden usar con interfaces para crear estructuras de datos reutilizables:

`types.ts`:
```typescript
interface Box<T> { 
    content: T; 
} 
const numberBox: Box<number> = { content: 10 };
```

En este ejemplo, la interfaz `Box` puede contener un valor de cualquier tipo especificado al crear una instancia de la interfaz.

Al comprender los roles distintos de los genéricos, interfaces y tipos, puedes diseñar de manera efectiva aplicaciones orientadas a objetos robustas y mantenibles en TypeScript. Ahora, continuemos y aprendamos qué hay de nuevo en TypeScript 5.

---

### Explorando características útiles de TypeScript 5

Como prometimos anteriormente, ahora exploraremos algunas de las características más destacadas introducidas en TypeScript 5.

> [!NOTE]
> En esta sección, mencionamos el concepto de errores del compilador. Si provienes de un entorno en lenguajes como C++ o Java, es importante entender una distinción particular. Un compilador es una herramienta que traduce el código fuente escrito en un lenguaje de programación a otra forma, típicamente código máquina o bytecode. El compilador de TypeScript, por otro lado, se enfoca en la verificación de tipos y en convertir TypeScript a JavaScript, en lugar de generar código máquina. Los desarrolladores experimentados pueden omitir esta sección si ya están familiarizados con estos conceptos.

- **Soporte de decoradores:** Los decoradores son una potente característica sintáctica que te permite añadir metadatos a clases, propiedades, métodos y otras partes de tu código. TypeScript 5 introduce un mejor soporte para decoradores, lo que te permite explorar esta funcionalidad para posibles casos de uso. Por ejemplo, el decorador `memoize` es un patrón común utilizado para la optimización del rendimiento. Almacena en caché los resultados de una llamada a una función según sus argumentos, evitando cálculos redundantes si se utilizan los mismos argumentos varias veces.

He aquí cómo puedes definir un decorador `log` en TypeScript 5:

`Decorators.ts`:
```typescript
function log(originalMethod: any, context: ClassMethodDecoratorContext) { 
    function replacementMethod(this: any, ...args: any[]) { 
        console.log(`Calling ${String(context.name)}`) 
        return originalMethod.call(this, ...args) 
    } 
    return replacementMethod 
} 

class Calculator { 
    @log 
    add(x: number, y: number): number { 
        return x + y 
    } 
} 

new Calculator().add(2, 3)
```

En este ejemplo, el decorador `@log` se aplica al método `add` dentro de la clase `Calculator`. Cada vez que llamamos al método `add`, el decorador `log` se llamará de antemano y registrará el nombre del contexto en la consola.

- **Manejo mejorado de enums:** De forma predeterminada, TypeScript 5 considera que todas las enumeraciones son enumeraciones de unión (*union enums*). Esto significa que cada valor de la enumeración se trata como un tipo distinto, mejorando la seguridad y la flexibilidad de tipos. Por ejemplo, considera un enum que representa opciones de color:

```typescript
enum Color { 
    Red, 
    Green, 
    Blue, 
} 
let myColor: Color = Color.Red
```

Ahora con TypeScript 5, la asignación de la variable a un valor diferente no está permitida:

```typescript
// myColor = 'orange';
```

Si descomentas la asignación de la variable, el compilador marcará esto con un error (*Figura 1.1: Error del compilador al asignar el tipo de enum incorrecto*). En este ejemplo, el enum `Color` es una enumeración de unión por defecto. No puedes asignar un valor que no esté explícitamente definido en el enum, mejorando la seguridad de tipos. Sin embargo, aún puedes asignar valores del enum como `Color.Red`.

- **Nuevo tipo de utilidad `NoInfer`:** TypeScript 5.4 introduce el tipo de utilidad `NoInfer` para mejorar la inferencia de tipos en funciones genéricas. Al llamar a funciones genéricas, TypeScript podría inferir tipos que no son ideales. Por ejemplo, una función que espera un color de una lista podría permitir que un color no válido no esté presente en la lista. `NoInfer<T>` evita que TypeScript considere el tipo interno `T` para una inferencia posterior. Esto permite una verificación de tipos más estricta basada en tipos definidos explícitamente. Considera un ejemplo simple donde tienes una función genérica que acepta un tipo `T`:

`NoInfer.ts`:
```typescript
class Animal { 
    sleep() {} 
} 

class Cat extends Animal { 
    miaw() {} 
} 

function petAnimal<T>(value: T, getDefault: () => T): T { 
    // ... function logic ... 
    return value || getDefault() 
} 

// This would compile without errors 
petAnimal(new Cat(), () => new Animal())
```

En el ejemplo mostrado aquí, esta función `petAnimal` toma dos argumentos: un valor de tipo `T` y `getDefault` como una función que debe devolver el mismo tipo que `value`. De forma predeterminada, la última línea permitirá que se infiera el tipo más amplio de la jerarquía, por lo que el tipo real sería `petAnimal(animal, () => animal)`. A veces, sin embargo, no deseas que el compilador infiera ciertos tipos.

Al usar `NoInfer<T>` para el tipo de retorno de `getDefault`, evitamos que el compilador infiera el tipo en función de cómo se usa `getDefault` dentro de `petAnimal`. Esto garantiza que `getDefault` deba devolver explícitamente un tipo compatible con `T`, manteniendo la seguridad de tipos para la función general.

En la siguiente sección, profundizaremos en la relación entre TypeScript y JavaScript.

---

### Comprendiendo la relación entre TypeScript y JavaScript

Habiendo establecido una comprensión sólida de los conceptos centrales de TypeScript, probablemente estés ansioso por aprender cómo migrar código JavaScript existente. Esto es especialmente valioso para aquellos con una sólida experiencia en JavaScript que buscan realizar la transición de proyectos a TypeScript. Para navegar eficazmente en este proceso, necesitamos comprender las diferencias fundamentales entre estos lenguajes.

En la siguiente sección, realizaremos una comparación de JavaScript y TypeScript, destacando las distinciones clave que guiarán tus esfuerzos de migración.

#### ¿Cómo se compara JavaScript con TypeScript?

Aunque tanto JavaScript como TypeScript comparten una sintaxis similar, existen diferencias clave que afectan la forma en que escribes y ejecutas el código. He aquí un desglose de algunas distinciones fundamentales:

- **Tipado frente a no tipado:** JavaScript es un lenguaje de tipado dinámico. Esto significa que el tipo de una variable se determina en tiempo de ejecución. TypeScript, por otro lado, es de tipado estático. Defines explícitamente los tipos de variables y funciones antes de la ejecución, lo que conduce a una comprobación de tipos más estricta y a menos errores en tiempo de ejecución.
- **Tipos implícitos frente a explícitos (con inferencia de tipos):** En JavaScript, los tipos suelen ser implícitos. Por ejemplo, `let x = 5;` implica que `x` es un número. TypeScript ofrece inferencia de tipos, lo que significa que a menudo puede deducir el tipo en función del valor inicial asignado. Así, `let x = 5` en TypeScript también infiere que `x` es un número. Sin embargo, TypeScript te permite anular explícitamente esta inferencia y asignar un tipo diferente cuando sea un tipo igualmente permitido.
- **Compilación frente a interpretación:** El código JavaScript normalmente es interpretado directamente por el navegador o el entorno del servidor. El código TypeScript pasa por un paso de compilación adicional que lo traduce a JavaScript mientras aplica comprobaciones de tipos. Este paso de compilación puede revelar potencialmente errores antes de que tu código llegue a ejecutarse.

Como ejemplo sencillo, el siguiente programa de JavaScript también es un programa de TypeScript válido de forma predeterminada cuando se establece la opción del compilador `noImplicitAny` en `false`, aunque no se declaren tipos en los nombres de los parámetros ni en el tipo de retorno:

`javascript-vs-typescript.ts`:
```typescript
function calculateArea(length, width) { 
    return length * width 
}
```

Esta es una función muy simple con dos argumentos, `length` y `width`, que los multiplica entre sí. No proporciona ningún tipo. En su lugar, asume que los argumentos se pueden multiplicar mediante el operador `*` (`mul`), que representa la multiplicación.

Sin embargo, el problema aquí es que cuando la opción `noImplicitAny` no está habilitada, el código se considerará menos seguro. He aquí un ejemplo de uso de esta función pasando argumentos válidos:

`javascript-vs-typescript.ts`:
```typescript
const area1 = calculateArea(5, 2); // Works fine 
const area2 = calculateArea("5", "2"); // No error but riskier
```

En este ejemplo, los parámetros `width` y `length` no tienen un tipo explícito. Como resultado, TypeScript los trata como `any`, permitiendo que se pasen tanto números como cadenas como argumentos. Esto podría crear problemas si pasáramos una cadena que no pueda ser coaccionada a un número, ya que el cálculo del área daría como resultado `NaN` (*Not a Number*).

> [!NOTE]
> En aras de la seguridad, debes habilitar la opción `noImplicitAny` de forma predeterminada. La opción `noImplicitAny` refuerza la seguridad de tipos en TypeScript. Sin ella, las funciones pueden adoptar de forma predeterminada el tipo `any`, eludiendo las comprobaciones de tipos y conduciendo potencialmente a errores. Imponer tipos explícitos para parámetros y valores de retorno, como se demostró en el ejemplo anterior de la función `calculateArea`, garantiza que la función opere sobre los tipos de datos previstos y previene comportamientos inesperados durante la ejecución.

Si tienes una base de código escrita en JavaScript, es posible que te pidan que la reescribas en TypeScript. La siguiente sección explica algunas técnicas que puedes utilizar para facilitar este esfuerzo.

#### Transición de JavaScript a TypeScript

Una pregunta razonable que podrías tener al intentar traducir código JavaScript existente a TypeScript es: ¿Cómo se puede hacer esto de manera eficiente y cómo escribir tipos correctos?

He aquí algunas estrategias efectivas:

- **Divide y vencerás:** Divide proyectos grandes de JavaScript en archivos y paquetes más pequeños y manejables. Esto te permite concentrarte en áreas específicas y evitar sentirte abrumado. Por ejemplo, comienza convirtiendo un módulo, como `auth.js` o `utils.js`, a TypeScript. Crea nuevos archivos llamados `auth.ts` y `utils.ts` y añade anotaciones de tipos.
- **Adopta los tipos `unknown` y `never`:** Considera usar `unknown` en lugar de `any` para mantener la flexibilidad y al mismo tiempo garantizar la seguridad de tipos. Del mismo modo, usa `never` para manejar explícitamente casos que no deberían ocurrir, haciendo que tu código sea más robusto y mantenible. El tipo `unknown` específicamente puede representar cualquier valor. Sin embargo, a diferencia de `any`, el tipo `unknown` no permite la asignación directa a otros tipos ni el acceso a propiedades sin comprobaciones de tipos previas. Estos tipos te ayudan a detectar errores a tiempo y hacen que tu código sea más predecible.
- **Conversión incremental de archivos:** Comienza cambiando el nombre de tus archivos JavaScript (`.js`) a archivos TypeScript (`.ts`). Dependiendo de la configuración de tu `tsconfig`, es probable que encuentres errores de compilación, lo cual es de esperar. Estos errores suelen indicar la falta de anotaciones de tipo.

Ahora, veamos un ejemplo de cómo solucionar la falta de tipos en los parámetros. Considera una función de JavaScript que comprueba si un parámetro es un objeto:

`isObject.js`:
```javascript
export const isObject = (o) => { 
    return o === Object(o) && !Array.isArray(o) && typeof o !== "function" 
}
```

He aquí cómo reescribirla en TypeScript:

`IsObject.ts`:
```typescript
export const isObject = (value: unknown): value is object => { 
    // Type guard using type assertion 
    return typeof value === "object" && value !== null && !Array.isArray(value) 
}
```

En este ejemplo, hemos agregado el parámetro `value: unknown`. Sin embargo, usamos un protector de tipo (*type guard*) con una aserción de tipo (`value is object`) para acotar el tipo a `object` dentro del cuerpo de la función, asegurando una representación más precisa.

Esto solo cubre una función. Seguir un enfoque incremental puede ayudarte a completar toda la tarea por partes mientras revisas los cambios de una manera más controlable.

A veces, deseas comenzar a usar TypeScript de inmediato y no esperar a una refactorización completa. La siguiente configuración del compilador te permite adoptar TypeScript de forma incremental.

#### Aprovechamiento del código JavaScript existente

Aunque recomendamos convertir progresivamente los archivos JavaScript a TypeScript, la opción del compilador `allowJs` ofrece un enfoque alternativo. Esta opción te permite importar archivos JavaScript normales (`.js`) directamente a tu proyecto TypeScript sin errores de compilación:

```typescript
import { isObject } from "./utilities.js"; // Notice the .js extension
```

En este ejemplo, el código importa la función `isObject` del archivo `utilities.js` (observa la extensión `.js`). Esto te permite utilizar el código JavaScript existente dentro de tu proyecto TypeScript con un simple cambio de línea.

> [!NOTE]
> Aunque `allowJs` proporciona flexibilidad, elude la comprobación de tipos para el código JavaScript importado. Esto puede introducir potencialmente errores relacionados con los tipos más adelante. Considera convertir estos archivos JavaScript a TypeScript para obtener una base de código más robusta y segura a largo plazo.

#### Manejo de librerías externas

Al importar desde librerías como Lodash o RxJS, TypeScript podría solicitarte que descargues definiciones de tipos. Estas definiciones proporcionan información de tipos para las funciones y objetos de la librería. Normalmente, el compilador sugerirá el comando de instalación adecuado. Por ejemplo, para instalar los tipos de Lodash, usarías lo siguiente:

```bash
$ npm install --save @types/lodash
```

La instalación de estas definiciones de tipos habilita la comprobación de tipos para las librerías importadas, mejorando la seguridad de tipos general de tu código.

#### Seguimiento de las sugerencias del compilador

En algunos casos, el compilador puede ofrecer sugerencias o mensajes de error que requieren una mayor investigación. Revisa atentamente estos mensajes y aprovecha los recursos en línea o la documentación de TypeScript ubicada en https://www.typescriptlang.org/docs/handbook/compiler-options.html para comprender cómo las diferentes opciones cambian el comportamiento del compilador.

#### Aprovechamiento de herramientas de linting

Considera el uso de herramientas como TSLint o ESLint con extensiones de TypeScript para identificar posibles problemas relacionados con los tipos y hacer cumplir las convenciones de codificación, mejorando la calidad y la mantenibilidad del código. En la sección *Lecturas complementarias* de este capítulo, proporcionamos un enlace al sitio web oficial de ESLint TypeScript que ofrece instrucciones sobre cómo habilitarlo en tu proyecto.

Al seguir estos consejos y abordar los tipos de librerías externas, puedes agilizar la migración de JavaScript a TypeScript y establecer una base de código más sólida y con mayor seguridad de tipos.

#### Patrones de diseño en JavaScript

Aunque se pueden implementar patrones de diseño tanto en JavaScript como en TypeScript, TypeScript ofrece ventajas significativas en términos de claridad, mantenibilidad y seguridad de tipos.

La naturaleza dinámica de JavaScript lo hace menos adecuado para patrones de diseño complejos. Faltan conceptos como las interfaces, lo que obliga a depender del tipado de pato (*duck typing*), comprobaciones de propiedades y aserciones en tiempo de ejecución.

Por ejemplo, al usar interfaces como parámetros, podemos cambiar la lógica de implementación en tiempo de ejecución, sin cambiar la firma de la función. Así es como funciona el patrón de diseño Strategy, como se explicará en el Capítulo 5.

Por ejemplo, veamos el tipado de pato frente al tipado estructural. Considera una función que registra mensajes y envía correos electrónicos:

`triggerNotification.js`:
```javascript
function triggerNotification(emailClient, logger) { 
    if (logger && typeof logger.log === "function") { 
        logger.log("Sending email") 
    } 
    if (emailClient && typeof emailClient .send === "function") { 
        emailClient.send("Message Sent") 
    } 
}
```

El ejemplo anterior aprovecha el tipado de pato (*duck typing*). Con el tipado de pato, la función confía en que los objetos tengan propiedades específicas en tiempo de ejecución. Esto puede provocar errores debido a formas incorrectas de los objetos o al orden de los argumentos. Mientras existan las propiedades `log` y `send` en esos objetos y sean funciones, esta operación tendrá éxito. Sin embargo, hay muchas formas en que esto puede salir mal. Observa la siguiente llamada a esta función:

```javascript
triggerNotification({ log: () => console.log("Logger call") }, { send: (msg) => console.log(msg) })
```

Cuando llamas a la función de esta manera, no sucede nada. Esto se debe a que el orden de los parámetros ha cambiado (se han intercambiado) y `log` o `send` no están disponibles como propiedades correspondientes.

Cuando proporcionas la forma correcta de los objetos, la llamada tiene éxito:

```javascript
triggerNotification({ send: (msg) => console.log(msg) }, { log: () => console.log("Logger call") })
```

Eso generaría el resultado correcto de esta función al ejecutarla en la línea de comandos:

```text
> Logger call 
> Message Sent
```

Con los argumentos correctos pasados a la función `triggerNotification`, verás la salida mencionada del comando `console.log`.

Aunque TypeScript ofrece interfaces para contratos claros, su principal fortaleza radica en el tipado estructural (*structural typing*). Esto significa que si un objeto tiene las propiedades y métodos requeridos, independientemente de su declaración, se puede utilizar en un contexto específico.

He aquí la misma función aprovechando el tipado estructural:

```typescript
function triggerNotification(emailClient: { send(message: string): void }, logger: { log(message: string): void }) { 
    logger.log('Sending email'); 
    emailClient.send("Message Sent"); 
} 
triggerNotification({ send: (msg) => console.log(msg) }, { log: (msg) => console.log(msg) });
```

En este ejemplo, definimos los parámetros de la función mediante tipos anónimos que especifican las propiedades y métodos requeridos (`send` y `log`). Siempre que los objetos pasados a la función tengan estas propiedades con funciones compatibles, se pueden utilizar, incluso sin declaraciones de interfaz formales.

En conclusión, aunque tanto JavaScript como TypeScript pueden implementar patrones de diseño, el tipado estático de TypeScript y características como las interfaces proporcionan una base más robusta y con mayor seguridad de tipos, especialmente para patrones de diseño complejos.

Habiendo explorado los fundamentos de los patrones de diseño y cómo TypeScript mejora su implementación, dirijamos ahora nuestra atención a configurar tu entorno de desarrollo.

---

### Configuración del entorno de desarrollo

El código fuente complementario de este libro está estructurado como un proyecto estándar de TypeScript. Se incluyen todas las librerías y configuraciones necesarias para que puedas ejecutar los ejemplos directamente desde la línea de comandos o dentro de Visual Studio Code. Esta sección te proporcionará los conocimientos necesarios para:

- Identificar las librerías utilizadas y sus propósitos dentro de los ejemplos.
- Comprender los parámetros de `tsconfig` que rigen el comportamiento del compilador de TypeScript.
- Ejecutar y depurar pruebas unitarias utilizando Vitest.

¡Empecemos!

#### Librerías y herramientas esenciales

El código utiliza varias librerías externas para mostrar patrones de diseño en contextos prácticos. Nuestro objetivo es ayudarte a revisar varios de los patrones de diseño dentro de un caso de uso específico. He aquí un desglose de sus roles:

- **React:** Esta popular librería de interfaz de usuario promueve patrones como la composición, factorías de componentes y componentes de orden superior (*Higher-Order Components*). Explicaremos cómo usar TypeScript con React en el Capítulo 2.
- **Vitest:** Este es un framework de pruebas rápido y con muchas funciones creado específicamente para proyectos TypeScript. Aprovecha las capacidades de Jest con funcionalidades adicionales como soporte integrado para TypeScript, mayor velocidad de ejecución de pruebas y una API moderna.
- **Express.js:** Perfecto para construir servicios web Node.js con TypeScript, Express ofrece un framework mínimo y estable, promoviendo la modularidad y el rendimiento. Aprenderás más sobre cómo usar TypeScript en el servidor en el Capítulo 2.
- **Immutable.js:** Esta librería es responsable del trabajo con estructuras de datos que involucran inmutabilidad. La inmutabilidad es un concepto que usamos con bastante frecuencia en programación funcional, mediante el cual no permitimos que los objetos sean modificados o alterados una vez creados. Aprenderemos más sobre la inmutabilidad en el Capítulo 7.
- **fp-ts:** Esta librería expone abstracciones de programación funcional como Mónadas (*Monads*), Opciones (*Options*) y Lentes (*Lens*). Profundizaremos en la programación funcional en el Capítulo 7.
- **RxJS:** Esta librería ofrece conceptos de programación reactiva como Observables a través de una API fácil de usar. El uso de Observables ayuda a crear aplicaciones escalables y resistentes. Aprenderás más sobre Observables en el Capítulo 8.

Cada capítulo explorará cómo estas herramientas te capacitan para implementar patrones de diseño con TypeScript de manera efectiva.

#### Comprensión del archivo tsconfig.json

Al trabajar con código fuente de TypeScript, el compilador necesita orientación sobre cómo ubicar y compilar tus archivos con configuraciones específicas. Esta configuración se logra a través de un archivo `tsconfig.json` o `jsconfig.json`. Por lo general, un solo archivo es suficiente para la mayoría de los proyectos. Sin embargo, para este libro, aprovecharemos un enfoque más flexible:

- **`tsconfig.json` base:** Define las opciones del compilador comunes aplicables en todos los capítulos.
- **`tsconfig.json` específico de cada capítulo:** Cada capítulo tendrá su propia configuración, heredando del archivo base.

Ahora, exploremos algunas opciones clave del compilador:

- **`module`:** Define cómo funcionan las importaciones y exportaciones. Usaremos CommonJS (formato de Node.js), generando sentencias `require` en el código compilado. Otras opciones incluyen `es2015`, `es2020`, `es2022` y `esnext`, que apuntan a versiones de ECMAScript Modules (ESM). Puedes inspeccionar el código generado en la carpeta `dist` para ver esto en acción.
- **`strict`:** Esta opción es una combinación de las opciones del compilador `noImplicitAny`, `strictNullChecks`, `strictFunctionTypes` y `strictBindCallApply`.
- **`baseUrl`:** Especifica el directorio base para resolver nombres de módulos no relativos. Esto puede simplificar tus sentencias de importación al proporcionar una ruta raíz para tus módulos. Por ejemplo, considera la siguiente configuración:
  ```json
  "baseUrl": "./src"
  ```
  Dentro de la carpeta `src`, cualquier componente puede importar otro componente utilizando esta declaración de importación:
  ```typescript
  import { MyComponent } from 'components/MyComponent';
  ```
  La ruta de importación resolverá esto como `src/components/MyComponent`.
- **`paths`:** Especifica alias para directorios que deben usarse junto con la opción `baseUrl`. He aquí un ejemplo:
  ```json
  "baseUrl": "./src", 
  "paths": { 
      "@components/*": ["components/*"], 
      "@utils/*": ["utils/*"] 
  }
  ```
  Con esta configuración, puedes importar módulos usando los alias definidos:
  ```typescript
  import { MyComponent } from '@components/MyComponent'; 
  import { myUtility } from '@utils/myUtility';
  ```
- **`target`:** Especifica la versión de generación de código de destino, como ES6, ES2017, etc. Esta opción garantiza la compatibilidad con el entorno elegido (por ejemplo, la versión de Node.js o el soporte del navegador). En este proyecto, usaremos ES5 para una amplia compatibilidad con navegadores.
- **`noImplicitAny`:** No permite compilar cuando TypeScript infiere un tipo como `any`. Esto ocurre a menudo con funciones que carecen de especificaciones de tipo en sus parámetros. He aquí un ejemplo que arroja un error con esta opción:

`degToRad.ts`:
```typescript
const degToRad = (degree): number => (degree * Math.PI) / 180;
```

Declara la función `degToRad` sin especificar el tipo del parámetro `degree`. Con `noImplicitAny`, el compilador arrojaría un error, solicitándote que definas el tipo de número esperado para el parámetro.

- **`strictNullChecks`:** Aplica comprobaciones más estrictas para `undefined` y `null`. El compilador identificará y generará errores para cualquier código que pueda dejar valores nulos sin verificar. He aquí un ejemplo:

`maybeNumber.ts`:
```typescript
// With strictNullChecks: error 
let maybeNumber: number | null = null 
let value = maybeNumber * 10 
let value2 
if (maybeNumber !== null) { 
    value2 = maybeNumber * 10 
} else { 
    value2 = 0 
} 
let value3 = maybeNumber ?? 0
```

Este código define una variable `maybeNumber` que puede ser de tipo `number` o `null`. El compilador arrojaría errores en la segunda línea al intentar asignar el cálculo a la variable `value`, ya que potencialmente podría ser `null`.

- **`experimentalDecorators` y `emitDecoratorMetadata`:** Son opciones obligatorias para utilizar decoradores (especialmente con Inversify.js). Los decoradores son un concepto potente para mejorar el comportamiento de las clases.
- **`sourceMap`:** Habilita los mapas de origen (*source maps*) para depurar código TypeScript. Esto permite a los depuradores (como VSCode o las herramientas de desarrollo del navegador) mostrar el código TypeScript original al pausar en los puntos de interrupción (*breakpoints*).
- **`lib`:** Especifica un conjunto de declaraciones de entorno para librerías externas. Esto puede ayudar al compilador a comprender funciones y objetos comunes utilizados en tu proyecto sin necesidad de instalar definiciones de tipos por separado. Por ejemplo, establecer `lib: ["dom"]` proporciona declaraciones para las APIs DOM del navegador.
- **`noUnusedLocals`:** Evita compilar código con variables locales no utilizadas. Esto ayuda a identificar y eliminar variables innecesarias, mejorando la claridad del código. He aquí un ejemplo:

`noUnusedLocals.ts`:
```typescript
function greet() { 
    const name = "Alice" // Used 
    let message // Unused (errors with noUnusedLocals) 
    message = "Hello, " + name + "!" 
    return message 
}
```

Con esta opción habilitada, el compilador arrojaría un error para la variable no utilizada `message`. Esto marca posibles problemas donde las variables podrían haber sido declaradas accidentalmente pero no utilizadas.

- **`noUnusedParameters`:** Evita compilar código con parámetros de función no utilizados. Esto ayuda a identificar y eliminar parámetros innecesarios, promoviendo definiciones de funciones más limpias.

También hay muchas más opciones del compilador disponibles que pueden ajustar diferentes aspectos del sistema. Estas opciones suelen ajustar aspectos más específicos del compilador personalizando la rigurosidad de las comprobaciones de tipos. Antes de habilitar opciones adicionales, llega a un consenso con tus colegas para evitar confusiones.

Por supuesto, al escribir software, contar con pruebas automatizadas añade otra capa de seguridad. Veamos cómo ejecutar las pruebas unitarias proporcionadas en este libro.

#### Ejecución de las pruebas unitarias

Como se mencionó anteriormente, utilizaremos Vitest, un popular framework de pruebas para proyectos JavaScript y TypeScript, para ejecutar pruebas unitarias. Vitest ofrece una configuración intuitiva y se integra bien con los principales frameworks. Este libro proporciona configuraciones para ejecutar pruebas directamente dentro de VSCode.

Aquí, hemos proporcionado opciones de configuración para ejecutar las pruebas unitarias en la línea de comandos. Para ejecutar las pruebas, tendrás que ejecutar el siguiente comando en la consola:

```bash
$ npm run test
```

Por ejemplo, hay un archivo llamado `mul.ts` en el Capítulo 1 que incluye una función para multiplicar dos números:

`mul.ts`:
```typescript
function mul(a: number, b: number): number { 
    return a * b 
} 
export default mul
```

Luego, también tenemos el archivo de prueba para esta función, que tiene el mismo nombre de archivo pero con la extensión `test.ts`:

`mul.test.ts`:
```typescript
import mul from "./mul" 
import { test, expect } from "vitest" 

test("multiplies 2 and 3 to give 6", () => { 
    expect(mul(2, 3)).toBe(6) 
})
```

Al ejecutar estos casos de prueba, verás los resultados del ejecutor de pruebas:

```text
$ npm run test 
> typescript-5-design-patterns-and-best-practices@1.0.0 test 
✓ |chapter-1_Getting_Started_With_Typescript_5| src/mul.test.ts (1) 
  ✓ multiplies 2 and 3 to give 6 

Test Files  1 passed (1) 
     Tests  1 passed (1) 
  Start at  12:46:37 
  Duration  5ms
```

Aprovechando el framework Vitest a lo largo de este libro, validaremos aspectos clave de los patrones de diseño. Esto incluye asegurar que el patrón Singleton, que exploraremos en el Capítulo 3, por ejemplo, cumpla con su principio de instancia única, y que el patrón Factory construya de manera confiable objetos del tipo correcto. Dado que las pruebas unitarias robustas a menudo preceden a los despliegues en producción, probar de forma consistente tus abstracciones de código se vuelve primordial.

---

### Uso de VSCode con TypeScript

Habiendo explorado las librerías incluidas en el libro y cómo ejecutar los ejemplos, dirijamos nuestra atención a dominar tu entorno de desarrollo. Un buen Entorno de Desarrollo Integrado (IDE) como VSCode puede mejorar significativamente tu productividad al depurar, refactorizar y trabajar con código TypeScript.

Utilizamos VSCode para desarrollar la base de código de este libro. Aprenderás a aprovechar las herramientas de inspección de VSCode para visualizar los tipos inferidos de las variables, obteniendo información valiosa sobre el comportamiento de tu código. Finalmente, cubriremos técnicas esenciales de refactorización para mejorar la legibilidad y reutilización del código.

Al utilizar eficazmente VSCode, obtendrás una experiencia de desarrollo más fluida y una comprensión más profunda de los conceptos de TypeScript a medida que trabajas con los ejemplos y exploras el material más a fondo.

#### Uso de VSCode para el código de este libro

VSCode es un editor integrado y ligero lanzado en 2015 por Microsoft. Ofrece una impresionante variedad de características que nos ayudan al escribir código. Actualmente admite varios lenguajes de programación importantes, incluidos TypeScript, Java, Go y Python. Podemos utilizar la integración nativa de TypeScript de VSCode para escribir y depurar código, inspeccionar tipos y automatizar tareas comunes de desarrollo.

Empecemos:

1. Para instalarlo, visita la página oficial de descargas en https://code.visualstudio.com/Download y elige el ejecutable adecuado para tu sistema operativo. En este libro, estamos utilizando la versión 1.88.1 de VSCode.
2. Una vez instalado, querrás abrir la carpeta de proyectos de este libro haciendo clic en **File | Open | (Project)**. Dado que estamos trabajando en el primer capítulo, puedes expandir la carpeta del Capítulo 1 e inspeccionar el código ubicado allí.
3. A continuación, querrás compilar todo el código de ejemplo de cada capítulo utilizando el siguiente comando desde la carpeta raíz:
   ```bash
   $ npm run build
   ```
   Hemos configurado el proyecto para utilizar *npm workspaces*, por lo que el script se encargará de ejecutar el paso de compilación en todas las carpetas relacionadas.
4. Al final, cada capítulo contendrá el código transpilado en su respectiva carpeta `dist`. Estos son archivos JavaScript normales que puedes ejecutar usando Node.js con la siguiente estructura de comandos:
   ```bash
   $ node ruta-absoluta-del-archivo/ejemplo.js
   ```
   Por ejemplo, he aquí cómo ejecutarías el archivo `decorators.js`:
   ```bash
   $ node chapters/chapter1_Getting_Started_With_Typescript_5/dist/decorators.js
   ```
   Salida:
   ```text
   Calculating 2 + 3 
   5 
   5
   ```

> [!NOTE]
> Algunos ejemplos pueden no producir ninguna salida en la consola. Además, algunos ejemplos contienen código comentado que de otro modo no compilaría si no estuviera comentado.

Esta sección ha explorado los beneficios de la seguridad de tipos y cómo aprovechar las características de VSCode para navegar por los ejemplos de código incluidos en el libro. La siguiente sección, *Inspección de tipos en acción*, te dotará de las herramientas y técnicas para una inspección de tipos efectiva dentro de VSCode.

---

### Inspección de tipos en acción

Ahora que sabes cómo ejecutar y depurar programas usando VSCode, probablemente quieras saber cómo inspeccionar tipos y aplicar sugerencias para mejorar la consistencia. VSCode te ayuda a inspeccionar tipos y mejorar la consistencia del código a medida que escribes TypeScript. El servidor de lenguaje de TypeScript integrado proporciona sugerencias e información de tipos.

#### Inspección de tipos

La inspección de tipos te permite verificar los tipos de datos asociados con variables, parámetros de funciones y valores de retorno. Esto garantiza que tu código se adhiera a las definiciones de tipos y evita posibles errores en tiempo de ejecución causados por discordancias de tipos de datos. He aquí cómo inspeccionar tipos usando VSCode:

1. Abre el archivo `removeDuplicateChars.ts` en el editor. Este contiene una función que acepta una cadena de entrada y elimina los caracteres duplicados. Siéntete libre de ejecutarlo e inspeccionar cómo funciona.
2. Si colocas el cursor del ratón sobre las variables en el cuerpo de la función, puedes inspeccionar sus tipos (*Figura 1.5: Inspección del valor de retorno inferido*).

Esto es obvio cuando declaramos su tipo explícitamente. Sin embargo, podemos inspeccionar tipos que han sido inferidos por el compilador y averiguar cuándo o por qué necesitamos agregar tipos explícitos.

El uso de los tipos correctos y la confianza en la inferencia de tipos siempre que sea posible es muy importante al trabajar con TypeScript. VSCode ofrece buenas utilidades de inspección para hacer esto, pero a menudo necesitamos ayudar al compilador a lograrlo. Aprenderás a trabajar con tipos y a comprender la inferencia de tipos en el Capítulo 2, en la sección *Trabajo con tipos avanzados*.

#### Refactorización con VSCode

La refactorización es una técnica potente para mejorar tu base de código. Implica reestructurar el código existente para mejorar su legibilidad, mantenibilidad y capacidad de adaptarse a cambios futuros. Más importante aún, la refactorización no debe alterar la funcionalidad central del código.

**¿Por qué refactorizamos?**
La refactorización te ayuda a escribir código más limpio y mantenible. El código limpio es más fácil de entender para ti y para los demás, lo que hace que sea más sencillo de modificar y ampliar en el futuro.

> [!NOTE]
> Cuando realices una refactorización, es recomendable contar con pruebas unitarias antes de cambiar cualquier código existente. Esto es para garantizar que no introduzcas cambios disruptivos ni dejes de capturar casos extremos.

#### Extracción de alias de tipo

Como ejemplo, podemos considerar extraer la firma de tipo de una función en un alias de tipo para una mejor legibilidad y reutilización. Sin embargo, antes de hacerlo, es importante comprender cuándo definir tipos explícitamente y cuándo confiar en la inferencia de tipos de TypeScript.

**Confiar en la inferencia de tipos:**
En muchos casos, permitir que TypeScript infiera los tipos puede generar un código más limpio y conciso. He aquí algunos escenarios donde la inferencia de tipos es preferible:
- Para funciones simples donde los tipos son claros por el contexto, puedes dejar que TypeScript infiera los tipos.
- Para variables locales, especialmente dentro de ámbitos pequeños, la inferencia de tipos puede ser más legible que las anotaciones de tipos explícitas.

**Uso de tipos explícitos:**
Por otro lado, hay situaciones en las que definir tipos explícitamente es ventajoso:
- Para funciones complejas con múltiples parámetros o un tipo de retorno complejo, definir tipos explícitamente puede mejorar la legibilidad y aclarar el propósito de la función.
- Para APIs públicas, donde necesitamos tipos que se utilizarán en diferentes módulos y los tipos explícitos proporcionan una mejor documentación.

Considera ambos enfoques antes de comenzar el proceso de refactorización para asegurarte de que el resultado final conduzca a un código más mantenible. Ahora, recorramos el proceso paso a paso de refactorización de una función.

#### Refactorización en acción con VSCode

Usando VSCode, podemos refactorizar el código con el que estamos trabajando utilizando algunas acciones de ayuda para refactorización. Veámoslas en acción con un ejemplo:

1. Abre el archivo `refactoring.ts` en el código fuente del Capítulo 1 en GitHub. Contiene funciones relacionadas con la búsqueda de elementos en un array:

`refactoring.ts`:
```typescript
function find<T>(arr: T[], predicate: (item: T) => boolean) { 
    for (let item of arr) { 
        if (predicate(item)) { 
            return item 
        } 
    } 
    return undefined 
}
```

Podemos mejorar la función `find` creando un alias de tipo reutilizable para el parámetro `predicate`. Este parámetro define una función que toma un elemento de tipo `T` y devuelve un tipo booleano.

2. Resalta todo el cuerpo de la función del parámetro `predicate`: `(item: T) => boolean`.
3. Haz clic derecho y selecciona **Refactor**, luego elige **Extract to type alias** (*Figura 1.6: Refactorización mediante extracción a alias de tipo*).
4. Nombra el alias de tipo como `Predicate`. Esto crea una definición de tipo reutilizable separada de la función:

```typescript
type Predicate<T> = (item: T) => boolean;
```

Ahora, la función `find` refactorizada puede aprovechar el tipo `Predicate` recién creado:

```typescript
type Predicate<T> = (item: T) => boolean 

function find<T>(arr: T[], predicate: Predicate<T>) { 
    for (let item of arr) { 
        if (predicate(item)) { 
            return item 
        } 
    } 
    return undefined 
}
```

VSCode ofrece varias otras funciones de refactorización que pueden ahorrarte tiempo y esfuerzo:
- **Extract Method (Extraer método):** Aísla bloques de código reutilizables en funciones dedicadas.
- **Extract Variable (Extraer variable):** Crea variables para almacenar expresiones utilizadas con frecuencia.
- **Rename Symbols (Renombrar símbolos):** Renombra variables de forma coherente en toda la base de código.

Estas herramientas pueden mejorar significativamente tu flujo de trabajo de desarrollo al reducir el riesgo de errores durante las modificaciones del código.

Ahora exploraremos el mundo del Lenguaje Unificado de Modelado (UML) para descubrir cómo representa visualmente los sistemas de software.

---

### Introducción al Lenguaje Unificado de Modelado (UML)

El diseño y la arquitectura de software eficaces requieren la necesidad de comunicar ideas complejas con claridad y precisión. En el ámbito de los patrones de diseño —soluciones reutilizables a desafíos de programación comunes— esta claridad se vuelve crucial. UML es un lenguaje visual creado específicamente para representar sistemas de software y sus intrincadas relaciones.

Desarrollado a finales de la década de 1980, UML se ha convertido en una herramienta de comunicación común para que arquitectos y desarrolladores tracen los planos de sus creaciones. Este capítulo se centra en los aspectos esenciales de UML, especialmente en los diagramas de clases como herramientas fundamentales para describir patrones de diseño. Los diagramas de clases ofrecen una representación visual de las clases y objetos, así como de sus interacciones dentro de un patrón de diseño. A medida que aprendamos más sobre ellos, exploraremos cómo UML permite a los desarrolladores no solo aprovechar los patrones de diseño de manera efectiva, sino también documentarlos y compartirlos en un formato fácilmente comprensible.

#### ¿Qué es UML?

Como se mencionó anteriormente, los patrones de diseño son soluciones predefinidas a problemas de software recurrentes. Su eficacia depende de una comunicación y comprensión claras entre los equipos de desarrollo. Aquí es donde entra en juego UML. UML, con su notación estandarizada y su enfoque en la representación visual, ofrece una forma independiente del lenguaje para representar patrones de diseño. A través de diagramas de clases, los desarrolladores pueden modelar los elementos centrales y las interacciones dentro de un patrón, independientemente de cualquier lenguaje de programación específico. Es como un diagrama que describe la sintaxis.

> [!NOTE]
> Aunque los diagramas UML tienen una larga historia en la ingeniería de software, debes usarlos con cuidado. Por lo general, solo deben utilizarse para demostrar un caso de uso o subsistema específico, junto con una breve explicación de las decisiones de arquitectura. UML no es muy adecuado para capturar los requisitos dinámicos de sistemas muy complejos porque, como lenguaje visual, solo es adecuado para representar visiones generales de alto nivel.

#### Aprendizaje de diagramas de clases UML

Los diagramas de clases UML proporcionan una instantánea estática de las clases y objetos dentro de un sistema de software. TypeScript, con su soporte para clases, interfaces y modificadores de visibilidad (`public`, `protected` y `private`), ofrece un lenguaje perfecto para traducir estos conceptos a código.

Examinemos los bloques de construcción fundamentales de los diagramas de clases:

- **Clases:** Las clases representan planos para crear objetos. Definen la estructura (propiedades) y el comportamiento (métodos) que compartirán todos los objetos de esa clase. Piensa en ellas como plantillas para crear entidades similares. En TypeScript, una definición de clase simple (*Figura 1.7: Diagrama de clases para la clase Product*) representa un producto con un nombre y un precio, con métodos para recuperar el nombre (`getName()`) y el precio (`getPrice()`), y para aplicar un descuento (`discount(discountPercentage)`) basado en un porcentaje proporcionado.
- **Objetos:** Los objetos son instancias individuales de una clase. Contienen valores específicos para las propiedades definidas dentro de la clase. Imagina cada objeto como una variación única construida a partir de la plantilla de la clase, que contiene tanto datos como comportamientos relacionados con esos datos.
- **Interfaces:** Definen un conjunto de funcionalidades (métodos) que una clase debe implementar. Especifican el *qué* (funcionalidades) sin dictar el *cómo* (detalles de implementación). Esto promueve el bajo acoplamiento y un mantenimiento del código más sencillo. He aquí una interfaz de muestra que una clase puede implementar (*Figura 1.8: Diagrama de clases para Interfaces*):

```typescript
interface Identifiable<T extends string | number> { 
    id: T; 
} 

class Product implements Identifiable<string> { 
    id: string; 
    constructor(id: string) { 
        this.id = id; 
    } 
}
```

`Identifiable` define un tipo genérico `T` que puede ser una cadena o un número, representando el formato del identificador (ID). Impone que cualquier clase que implemente `Identifiable` debe tener una propiedad `id` de tipo `T`. `Product` implementa `Identifiable` con `string` como el tipo específico para su propiedad `id`.

- **Clases abstractas:** Estas clases actúan como modelos que no se pueden instanciar directamente (lo que significa que no puedes crear objetos directamente a partir de ellas). Proporcionan una base para construir clases más especializadas que heredan sus propiedades y funcionalidades. Un diagrama de clase abstracta (*Figura 1.9: Diagrama de clases para clases abstractas*) muestra cómo una clase abstracta, `Shape`, y una subclase, `Square`, definen funcionalidades comunes (como una propiedad pública `color` y un método abstracto `getArea()`) mientras permiten que las subclases concretas implementen detalles específicos y proporcionen implementaciones concretas para calcular el área en función de la longitud del lado.
- **Asociaciones:** Las asociaciones son los bloques de construcción para representar relaciones entre clases, interfaces u otros elementos dentro de un diagrama UML. Piensa en ellas como puentes que conectan diferentes partes de tu sistema. Estas asociaciones pueden ser directas o indirectas, ofreciendo flexibilidad para representar diversas interacciones. Una asociación directa ocurre cuando una clase mantiene una referencia directa a otra clase como un objeto o propiedad. Una asociación indirecta ocurre cuando una clase hace referencia a otra clase a través de un identificador (como un ID) pero no mantiene una referencia directa al objeto. Por ejemplo, tenemos los siguientes modelos para `Blog` y `Author` (*Figura 1.10: Diagrama de clases para asociaciones*):

`associations.ts`:
```typescript
class Author { 
    constructor(private id: string, private name: string) {} 
} 

class Blog implements Identifiable<string> { 
    constructor(private id: string, private author: Author) {} 
}
```

Observa que la elección entre asociaciones directas e indirectas depende de tus necesidades de diseño específicas. Las asociaciones directas ofrecen un acoplamiento más estrecho entre clases, mientras que las asociaciones indirectas proporcionan un acoplamiento más flexible y pueden ser más convenientes para ciertos escenarios.

- **Agregaciones:** La agregación, una forma especializada de asociación en UML, representa una relación de parte-todo entre clases. Indica que una clase (el todo) puede existir de forma independiente, mientras que la otra clase (la parte) tiene una dependencia más fuerte del todo para su existencia. Por ejemplo, supongamos que tenemos una clase `SearchService` que acepta un parámetro `QueryBuilder` y realiza solicitudes de API en un sistema diferente (*Figura 1.11: Diagrama de clases para agregaciones*):

`aggregations.ts`:
```typescript
class QueryBuilder {} 
class EmptyQueryBuilder extends QueryBuilder {} 

interface SearchParams { 
    qb?: QueryBuilder; 
    path: string; 
} 

class SearchService { 
    queryBuilder?: QueryBuilder; 
    path: string; 
    constructor({ private qb = new EmptyQueryBuilder(), private path: string }: SearchParams) { 
        this.queryBuilder = qb; 
        this.path = path; 
    } 
}
```

En este caso, cuando no tenemos una clase `QueryBuilder` o la clase en sí no tiene consultas para realizar, `SearchService` seguirá existiendo, aunque en realidad no realizará ninguna solicitud. `QueryBuilder` también puede existir sin `SearchService`.

- **Composiciones:** La composición, un concepto potente en UML, representa una forma más estricta de agregación. Representa una relación de "tiene-un" (*has-a*) donde la clase padre (el todo) controla el ciclo de vida de sus hijos (las partes). Esto significa que si el objeto padre se destruye, todos sus objetos secundarios también se destruyen. He aquí un ejemplo con las clases `House` y `Room` (*Figura 1.12: Diagrama de clases para composiciones*):

`compositions.ts`:
```typescript
class Room { 
    constructor(private name: string) {} 
    getName(): string { 
        return this.name; 
    } 
} 

class House { 
    constructor(private rooms?: Room[]) { 
        this.rooms = rooms || []; 
    } 
    addRoom(room: Room): void { 
        this.rooms.push(room); 
    } 
    removeRoom(room: Room): void { 
    } 
    getRooms(): Room[] { 
        return this.rooms; 
    } 
}
```

El diagrama de clases representa dos clases: `Room` y `House`. Una clase `Room` tiene un nombre y métodos para acceder y configurarlo. Una clase `House` tiene un array privado de objetos `Room` y métodos para administrarlos, incluida la adición de objetos de habitación y la recuperación de la lista completa. La relación de composición significa que una clase `House` puede contener múltiples objetos de habitación, y el ciclo de vida de los objetos `Room` está ligado al de `House`.

- **Herencia:** La herencia, un concepto fundamental en la programación orientada a objetos, permite que las clases hereden propiedades y funcionalidades de otras clases. Establece una relación jerárquica donde una subclase (clase hija) hereda de una clase base (clase padre). Esto promueve la reutilización del código y reduce la redundancia. Revisemos el ejemplo de `BaseClient` y `UsersApiClient` para ilustrar la herencia (*Figura 1.13: Diagrama de clases para herencia*):

`inheritance.ts`:
```typescript
class BaseClient { 
    constructor(protected baseUrl: string) { 
        this.baseUrl = baseUrl; 
    } 
    protected getBaseUrl(): string { 
        return this.baseUrl; 
    } 
} 

class UsersApiClient extends BaseClient { 
    constructor(baseUrl: string) { 
        super(baseUrl); 
    } 
    getUsers(): void { 
        console.log(`Fetching users from ${this.getBaseUrl()}/users`); 
    } 
}
```

- **Visibilidad:** La visibilidad juega un papel importante en el diseño orientado a objetos al controlar el acceso a los elementos internos de una clase (atributos y métodos). En los diagramas UML, símbolos específicos representan los niveles de visibilidad, lo que garantiza una comunicación clara dentro de tu equipo:
  - **Público (`+`):** Los elementos marcados con un más (`+`) son accesibles desde cualquier lugar dentro del sistema.
  - **Privado (`-`):** Los elementos marcados con un menos (`-`) solo son accesibles dentro de la propia clase.
  - **Protegido (`#`):** Los elementos marcados con una almohadilla (`#`) son accesibles desde la propia clase y sus subclases.
  - **Paquete (`~`):** Los elementos marcados con una virgulilla (`~`) son accesibles dentro del mismo paquete. Este nivel de visibilidad se usa con menos frecuencia en el diseño orientado a objetos moderno.

Por ejemplo, tenemos una clase `SSHUser` que acepta una clave privada y una clave pública (*Figura 1.14: Diagrama de clases para modificadores de visibilidad*):

`visibility.ts`:
```typescript
class SSHUser { 
    constructor( 
        private privateKey: string, 
        public publicKey: string, 
    ) { 
        this.privateKey = privateKey 
        this.publicKey = publicKey 
    } 

    public getBase64(): string { 
        return Buffer.from(this.publicKey) 
            .toString("base64") 
    } 
}
```

Aquí, podemos ver que los métodos están separados por una barra horizontal para mayor visibilidad.

Si bien dibujar diagramas de clases en papel puede parecer sencillo, el verdadero desafío radica en modelar con precisión los conceptos y relaciones subyacentes dentro de tu dominio del problema. Este proceso suele ser iterativo y requiere la colaboración de expertos en el dominio o partes interesadas bien informadas. El Capítulo 9, centrado en el desarrollo de aplicaciones TypeScript modernas y robustas, profundizará en cómo el Diseño Guiado por el Dominio (*Domain-Driven Design* / DDD) puede ser una herramienta valiosa para traducir eficazmente las reglas de negocio en una representación UML bien estructurada.

---

### Resumen

Eso concluye nuestro primer capítulo introductorio. Este capítulo sirvió como tu punto de partida en el mundo de TypeScript. Comenzamos revelando los tipos fundamentales y las características del lenguaje que definen TypeScript, junto con su conexión con JavaScript. Luego, recorrimos el proceso de conversión de un programa simple de JavaScript a su equivalente en TypeScript.

A continuación, exploramos las librerías esenciales que encontrarás a lo largo de este libro, destacando su papel en la construcción de aplicaciones escalables. Exploramos el archivo `tsconfig` y sus diversas opciones, lo que te permite personalizar tu experiencia de desarrollo con TypeScript.

Continuando, te capacitamos con las habilidades necesarias para ejecutar, depurar y perfeccionar eficazmente tu código utilizando el potente editor VSCode. Exploramos sus capacidades integradas para la refactorización, lo que te permite mejorar aún más la estructura y mantenibilidad de tu base de código.

Finalmente, presentamos UML y los diagramas de clases, que sirven como un método tradicional para visualizar patrones de diseño y abstracciones. Comprender UML te proporciona un enfoque estandarizado para documentar los sistemas de software y sus modelos.

Al combinar el conocimiento adquirido en este capítulo, estás bien posicionado para embarcarte en la creación de proyectos básicos de TypeScript. Esta experiencia práctica consolidará tu comprensión del lenguaje. Además, aprender a aprovechar las tareas y configuraciones de lanzamiento de VSCode puede mejorar significativamente tu flujo de trabajo de desarrollo y productividad.

En el próximo capítulo, consideraremos las complejidades del sistema de tipos de TypeScript, explorando sus características más avanzadas.

---

### Preguntas y respuestas

Siéntete libre de revisar las siguientes preguntas y sus respuestas correspondientes para resolver cualquier duda u obtener información adicional:

1. **Más allá de los errores en tiempo de ejecución, ¿existen otras ventajas en la seguridad de tipos de TypeScript?**
   - **Respuesta:** Sí, la comprobación estática de tipos en TypeScript ofrece varios beneficios:
     - **Claridad del código mejorada:** Los tipos explícitos mejoran la legibilidad y la mantenibilidad del código para ti y otros desarrolladores.
     - **Detección temprana de errores:** Los errores de tipo se detectan durante el desarrollo, evitando errores inesperados en tiempo de ejecución.
     - **Mejor soporte del IDE:** La información de tipos permite que los IDEs ofrezcan funciones como autocompletado y refactorización que son más precisas y útiles.

2. **¿Se puede utilizar TypeScript con bases de código JavaScript existentes?**
   - **Respuesta:** ¡Sí! TypeScript se integra perfectamente con JavaScript. Puedes migrar gradualmente el código JavaScript existente a TypeScript pieza por pieza, lo que garantiza una transición suave.

3. **¿Cuándo podría ser más desafiante refactorizar con TypeScript?**
   - **Respuesta:** Si bien TypeScript generalmente simplifica la refactorización, puede haber situaciones en las que presente más desafíos. Por ejemplo, bases de código extensas sin anotaciones de tipos existentes pueden requerir más esfuerzo para refactorizarse eficazmente. La falta de opciones del compilador como `noImplicitAny` o el modo estricto (`strict`) puede reducir la seguridad de tipos. Además, incluso con TypeScript, una lógica de código muy compleja aún puede requerir pruebas durante la refactorización para garantizar que los cambios no introduzcan consecuencias no deseadas.

4. **¿Existen limitaciones en el uso de diagramas UML?**
   - **Respuesta:** Si bien los diagramas UML son una herramienta valiosa para visualizar el diseño de software, tienen algunas limitaciones. A medida que los sistemas se vuelven más grandes y complejos, los diagramas UML pueden volverse engorrosos y difíciles de mantener. Además, los diagramas UML se centran principalmente en la estructura estática de un sistema, representando las relaciones entre clases y componentes. Es posible que no capturen los aspectos dinámicos del comportamiento de un sistema, como la forma en que los objetos interactúan y cómo fluyen los datos a través del sistema.

---

### Lecturas complementarias

- Para una buena introducción a TypeScript, lee *Learning TypeScript*, Remo H. Jansen, Packt Publishing. Disponible en: https://www.packtpub.com/product/learning-typescript-2x-second-edition/9781788391474
- Puedes configurar TypeScript con ESLint siguiendo la guía de inicio disponible en: https://typescript-eslint.io/getting-started
- La refactorización se explica en detalle en *Refactoring: Improving the Design of Existing Code, 2nd Edition*, Martin Fowler. Disponible en: https://martinfowler.com/books/refactoring.html
- UML, según lo explicado por sus creadores, se detalla en *The Unified Modeling Language User Guide*, Booch and James Rumbaugh. Disponible en: https://www.researchgate.net/publication/234785986_Unified_Modeling_Language_User_Guide_The_2nd_Edition_Addison-Wesley_Object_Technology_Series
- Para obtener información completa sobre npm (*Node Package Manager*), visita la documentación oficial de npm en: https://docs.npmjs.com/
- Para obtener más información sobre React, incluidas sus características y cómo comenzar, consulta el sitio web oficial de React en: https://react.dev/
- Para obtener información detallada sobre Express.js, incluidas guías y documentación de la API, visita el sitio web oficial de Express.js en: https://expressjs.com/
- Para comprender el motor JavaScript V8, que impulsa Node.js y Chrome, puedes visitar la página oficial del proyecto V8 en: https://v8.dev/
