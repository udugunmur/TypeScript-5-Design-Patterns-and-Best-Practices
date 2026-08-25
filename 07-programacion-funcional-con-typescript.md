# Parte 3: Conceptos avanzados de TypeScript y mejores prácticas

## Capítulo 7: Programación funcional con TypeScript

En este capítulo, comenzaremos a explorar algunos paradigmas de programación disponibles en el lenguaje TypeScript, empezando por la programación funcional. A diferencia de los patrones de diseño, que son soluciones reutilizables para problemas comunes, los conceptos de programación funcional sirven como bloques de construcción fundamentales que se pueden combinar de varias maneras para crear patrones de programación robustos y flexibles.

La programación funcional ya es inherente a JavaScript, lo que permite a los desarrolladores aprovechar conceptos como funciones de orden superior (*Higher-Order Functions* o HOFs), clausuras (*closures*) y recursión. TypeScript se basa en estos principios agregando tipado estático, lo que proporciona mayor seguridad al construir aplicaciones más grandes y complejas.

La programación funcional se centra en conceptos clave como expresiones, composición de funciones, recursión, inmutabilidad, pureza y transparencia referencial. Al aprovechar estos conceptos, particularmente a través de las funciones de orden superior, puedes lograr una mayor flexibilidad y mantenibilidad en el diseño de tus aplicaciones.

A lo largo de este capítulo, descubrirás cómo estos conceptos cobran vida en TypeScript, permitiéndote construir estructuras avanzadas que mejoran tu capacidad para producir programas más grandes y complejos sin comprometer la seguridad de tipos.

Cubriremos los siguientes temas en este capítulo:

- Comprensión de los conceptos clave en programación funcional
- Exploración de estructuras funcionales prácticas
- Implementación de lentes funcionales (*Functional Lenses*)
- Comprensión y utilización de mónadas (*Monads*)

Al final de este capítulo, habrás adquirido las habilidades y técnicas necesarias para escribir software altamente componible utilizando potentes conceptos de programación funcional. Estarás equipado para aprovechar el sistema de tipos de TypeScript y garantizar la seguridad de tipos mientras adoptas el paradigma funcional.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter07_Functional_Programming_Concepts

---

### Comprensión de los conceptos clave en programación funcional

La programación funcional es un paradigma que utiliza funciones como los bloques de construcción principales para construir programas más grandes. Se basa en varios conceptos centrales que la distinguen de otros paradigmas.

Haremos una distinción entre lo que hemos aprendido hasta ahora sobre patrones de diseño y lo que aprenderemos ahora sobre conceptos de diseño, ya que tienen un significado diferente:

Los conceptos de diseño son los bloques de construcción de cualquier paradigma de programación. Por ejemplo, los conceptos básicos de la Programación Orientada a Objetos (POO) son la encapsulación, la abstracción, la herencia y el polimorfismo. Si no tienes encapsulación, no puedes proteger el acceso a los miembros privados de los objetos, lo que dificulta la aplicación de ciertos patrones de diseño.

Bajo el paradigma de programación funcional, existen conceptos clave que debes utilizar para obtener los mayores beneficios. Explicaremos los conceptos esenciales de la programación funcional uno por uno y luego exploraremos algunas abstracciones prácticas.

#### Programación declarativa frente a imperativa

Existen muchos paradigmas de programación que ofrecen diferentes enfoques para estructurar programas. En el contexto de la programación funcional, el método preferido es la programación declarativa, que enfatiza declarar qué debe lograr el programa en lugar de detallar cómo lograrlo. Este enfoque a menudo aprovecha los métodos de array de TypeScript, que son funcionales y encadenables, mejorando la legibilidad y la mantenibilidad.

Por el contrario, cuando especificas cómo realizar tareas paso a paso en un orden particular, el programa sigue el modelo de programación imperativa.

##### Programación imperativa

La programación imperativa es un paradigma que se enfoca en describir cómo realizar tareas, especificando los pasos exactos que la computadora debe seguir para lograr un objetivo. Con este enfoque, cada paso debe ejecutarse en una secuencia específica. Por ejemplo, considera la suma típica de números pares expresada de forma imperativa:

`paradigms.ts`:
```typescript
let evenSum = 0; 
for (let i = 1; i <= 10; i++) { 
    if (i % 2 === 0) { 
        evenSum += i; 
    } 
}
```

Aquí, hay una variable mutable `evenSum` que se actualiza dentro del bucle `for` solo cuando se cumple la condición `if`. Todo el proceso es sensible al orden. Si divides la declaración y asignación de la variable en dos partes y mueves la asignación dentro del bucle `for`, el resultado sería diferente, haciendo que esta sección sea más frágil si mueves piezas de código. Por otro lado, la programación imperativa es intuitiva cuando se trata de manipular el estado.

##### Programación declarativa

La programación declarativa es un paradigma que se enfoca en describir qué debe hacer el programa sin especificar cómo debe hacerse. Este enfoque abstrae los detalles de implementación, permitiendo a los desarrolladores declarar el resultado esperado en un flujo más natural e intuitivo. Enfatiza los resultados deseados en lugar de los pasos específicos para lograrlos.

Por ejemplo, considera cómo podemos expresar la suma de números pares utilizando un modelo de programación declarativa:

`paradigms.ts`:
```typescript
const numbers = Array.from({ length: 10 }, (_, i) => i + 1); 
const sum = numbers 
    .filter(n => n % 2 === 0) 
    .reduce((acc, n) => acc + n, 0); 

console.log(sum); // Output: 30
```

La primera línea declara que queremos generar un array de números del 1 al 10. Luego, la segunda línea crea una tubería (*pipeline*) de funciones: filtrar los números pares de una lista y luego reducirlos a una suma. El flujo de control y la gestión del estado se abstraen, haciendo que el código sea más legible y expresivo.

#### Funciones puras

Las funciones puras son la piedra angular de la programación funcional. Son funciones con dos características clave:

1. **Salida determinista (*Deterministic output*):** Producen la misma salida con los mismos argumentos. La función puede o no aceptar una entrada, pero para los mismos argumentos, siempre debe devolver la misma salida:

`pure-functions.ts`:
```typescript
function add(a: number, b: number): number { 
    return a + b 
} 

console.log(add(2, 3)) // 5 
console.log(add(2, 3)) // 5
```

Esta función `add` es pura porque siempre devuelve el mismo resultado para las mismas entradas y no modifica ningún estado externo ni produce efectos secundarios.

2. **Sin efectos secundarios (*No side effects*):** Una función se vuelve impura cuando realiza acciones que afectan o dependen del mundo exterior más allá de simplemente devolver un resultado. Estas acciones se conocen como efectos secundarios. Un efecto secundario es algo que interactúa con el sistema y no es parte del programa puro (por ejemplo, imprimir en la consola o abrir un archivo):

`impure-functions.ts`:
```typescript
let count = 0 

function incrementAndLog(value: number): number { 
    count++ // Modifies external state 
    console.log(`Count is now ${count}`) // Side effect: logging 
    return value + 1 
} 

console.log(incrementAndLog(5)) 
console.log(incrementAndLog(5))
```

Esta función `incrementAndLog` es impura porque modifica una variable externa (`count`), escribe en la consola y, para la misma entrada, produce salidas diferentes en las llamadas subsiguientes.

Puedes pensar en las funciones puras como funciones matemáticas porque las funciones matemáticas relacionan una entrada con una salida. La entrada es el dominio de la función (el argumento en programación). La salida es el codominio de la función (el tipo de retorno). También está el rango de la función, que es el valor de retorno real:

`pure-functions.ts`:
```typescript
function toZero(num: number): 0 { 
    return 0 
}
```

En el código anterior, el dominio de esta función son los números reales (por ejemplo, –1, -∞, +∞, 10, 5). El codominio es únicamente el número 0. Por lo tanto, independientemente del número que usemos, siempre devolverá cero.

#### Clausuras (*Closures*)

Una clausura (*closure*) es una función que tiene acceso a su propio ámbito, así como al ámbito de sus funciones externas. Esto significa que una clausura puede utilizar variables de su lista de definición local y de otras funciones envolventes.

Dado que tanto TypeScript como JavaScript permiten declarar funciones dentro de otra función y llamar a funciones anónimas, permite que esas funciones accedan al ámbito de la función contenedora:

`closure.ts`:
```typescript
function makeFunc() { 
    const name = "Alex" 
    function displayName() { 
        console.log(name) 
    } 
    return displayName 
} 

const myFunc = makeFunc() 
myFunc() // Alex
```

En este ejemplo, `makeFunc` devuelve `displayName`, que es una clausura. Aunque `makeFunc` ha terminado de ejecutarse, `displayName` todavía tiene acceso a la variable `name` del ámbito de la función externa.

Considera el siguiente fragmento de código con una función que utiliza clausuras para crear y devolver otra función:

`closure.ts`:
```typescript
let buttonProps = (borderRadius) => { 
    const createVariantButtonProps = (variant, color) => { 
        const newProps = { borderRadius, variant, color }; 
        return newProps; 
    }; 
    return createVariantButtonProps; 
}; 

let primaryButton = buttonProps("1rem"); 
const primaryButtonProps = primaryButton("primary", "red"); 
console.log(primaryButtonProps); // Output: { borderRadius: "1rem", variant: "primary", color: "red" }
```

En este ejemplo, `buttonProps` devuelve una clausura que captura la variable `borderRadius`.

#### Manejo de efectos secundarios – Acciones IO

En las aplicaciones del mundo real, los efectos secundarios a menudo son necesarios porque interactúan con el mundo exterior y producen resultados visibles. La programación funcional no elimina los efectos secundarios, sino que los gestiona cuidadosamente envolviéndolos en acciones IO (*Input-Output*):

`io-actions.ts`:
```typescript
interface IO<A> { 
    (): A; 
} 

const getCurrentTime: IO<string> = () => new Date().toISOString(); 
const logMessage = (message: string): IO<void> => () => console.log(message); 

const time = getCurrentTime(); 
console.log(time); 
logMessage("Hello, World!")();
```

`IO` actúa como un contenedor para las acciones de entrada/salida y denota que envuelve una función que produce efectos secundarios. De esta manera, el desarrollador es plenamente consciente de la situación y puede componer y manipular estas acciones sin ejecutar inmediatamente los efectos secundarios.

| Enfoque | Descripción | Fortalezas | Limitaciones |
| :--- | :--- | :--- | :--- |
| **Acciones IO** | Encapsula los efectos secundarios | Indicación clara de efectos secundarios; componible | Puede introducir complejidad en la gestión de acciones |
| **Inyección de dependencias** | Invierte el control de las dependencias, facilitando las pruebas | Desacopla componentes, mejora la capacidad de prueba | Puede aumentar la complejidad; requiere un framework de DI |
| **Patrón Observer** | Permite que los objetos se suscriban y reaccionen a eventos | Promueve el bajo acoplamiento | Puede provocar fugas de memoria si no se gestiona adecuadamente; las cadenas de eventos complejas son difíciles de descifrar |

*Figura 7.1: Comparación de enfoques para gestionar efectos secundarios.*

#### Recursión

La recursión es un concepto fundamental en la programación funcional donde una función se llama a sí misma dentro de su propio cuerpo, a menudo con varios parámetros. En lugar de utilizar bucles imperativos, las funciones recursivas proporcionan una solución más elegante y a menudo más intuitiva:

`recursion.ts`:
```typescript
function factorial(n: number): number { 
    if (n <= 1) return 1 
    return n * factorial(n - 1) 
} 

console.log(factorial(5)) // Output: 120
```

Esta función demuestra dos elementos clave de la recursión:
- Un caso base (`n <= 1`) que detiene la recursión.
- Un caso recursivo que divide el problema en un subproblema más pequeño.

Pila de llamadas para `factorial(5)`:
```text
factorial(5) -> 5 * factorial(4) -> 5 * 4 * factorial(3) -> 5 * 4 * 3 * factorial(2) -> 5 * 4 * 3 * 2 * factorial(1) -> 5 * 4 * 3 * 2 * 1 -> 120
```

##### Uso de recursión en árboles

La recursión destaca al tratar con estructuras naturalmente recursivas como árboles:

`recursion.ts`:
```typescript
interface TreeNode { 
    value: number 
    left?: TreeNode 
    right?: TreeNode 
} 

function inOrder(node: TreeNode | undefined): number[] { 
    if (!node) { 
        return [] 
    } 
    return [...inOrder(node.left), node.value, ...inOrder(node.right)] 
} 

const tree: TreeNode = { 
    value: 1, 
    left: { value: 2, left: { value: 4 }, right: { value: 5 } }, 
    right: { value: 3, left: { value: 6 }, right: { value: 7 } }, 
} 

console.log(inOrder(tree))
```

Este ejemplo muestra cómo se puede utilizar la recursión para iterar sobre un árbol con un número desconocido de niveles de manera fluida mediante un recorrido en orden (*in-order*).

##### Recursión de cola y optimización (*Tail recursion*)

Para evitar errores de desbordamiento de pila (*stack overflow*) con entradas grandes, se utiliza la optimización de llamadas de cola (*tail call optimization*), que ocurre siempre que la última llamada devuelve directamente una llamada a la función en lugar de una expresión:

`recursion.ts`:
```typescript
function factorialTail(n: number, accumulator: number = 1): number { 
    if (n <= 1) return accumulator; 
    return factorialTail(n - 1, n * accumulator); 
} 

console.log(factorialTail(5)) // Output: 120
```

> [!IMPORTANT]
> Ten en cuenta que aunque conceptualmente el código presentado está formado correctamente para admitir llamadas de cola, los entornos de ejecución estándar de TypeScript/JavaScript desafortunadamente no siempre admiten optimización de llamada de cola (*TCO*) a bajo nivel.

#### Funciones como ciudadanos de primera clase

En TypeScript, las funciones son tratadas como **ciudadanos de primera clase** (*first-class citizens*), lo que significa que pueden usarse de forma nativa en diversas operaciones.

##### Asignadas a variables

`first-class-citizens.ts`:
```typescript
const greet = function(name: string): string { 
    return `Hello, ${name}!`; 
}; 

console.log(greet("Alice")); // Output: Hello, Alice!
```

##### Pasadas como argumentos a otras funciones

`first-class-citizens.ts`:
```typescript
function executeOperation(x: number, y: number, operation: (a: number, b: number) => number): number { 
    return operation(x, y); 
} 

const add = (a: number, b: number) => a + b; 
const multiply = (a: number, b: number) => a * b; 

console.log(executeOperation(5, 3, add)); // Output: 8 
console.log(executeOperation(5, 3, multiply)); // Output: 15
```

##### Devueltas como valores de otras funciones

`first-class-citizens.ts`:
```typescript
function createMultiplier(factor: number): (x: number) => number { 
    return function(x: number): number { 
        return x * factor; 
    }; 
} 

const double = createMultiplier(2); 
const triple = createMultiplier(3); 

console.log(double(5)); // Output: 10 
console.log(triple(5)); // Output: 15
```

##### Almacenadas en estructuras de datos

```typescript
const mathOperations = { 
    add: (a: number, b: number) => a + b, 
    subtract: (a: number, b: number) => a - b, 
    multiply: (a: number, b: number) => a * b, 
    divide: (a: number, b: number) => a / b 
}; 

console.log(mathOperations.add(10, 5)); 
console.log(mathOperations.multiply(3, 4));
```

Estas capacidades habilitan la creación de **Funciones de Orden Superior** (*Higher-Order Functions* o HOFs), que son funciones que toman otras funciones como argumentos o devuelven funciones como salida.

#### Composición de funciones

La composición de funciones es un concepto matemático mediante el cual se aplica una función a los resultados de otra:

`function-composition.ts`:
```typescript
function double(x: number): number { 
    return x * 2 
} 

function increment(x: number): number { 
    return x + 1 
} 

const doubleAndIncrement = (x: number): number => increment(double(x)) 

console.log(doubleAndIncrement(3)) // Output: 7 
// Explanation: (3 * 2) + 1 = 7
```

Inferencia de tipos en la composición:

`Function-composition.ts`:
```typescript
interface Person { 
    name: string; 
    age: number; 
} 

function getDisplayName(p: Person): string { 
    return p.name.toLowerCase(); 
} 

function getLength(s: string): number { 
    return s.length; 
} 

const getDisplayNameLength = compose(getDisplayName, getLength);
```

El caso más simple de composición es cuando tienes dos funciones, $f$ y $g$, que aceptan un solo parámetro y forman la expresión:
$$f(g(x)) \quad \text{o} \quad f \circ g$$

##### Composición de funciones con múltiples argumentos

`function-composition.ts`:
```typescript
function capitalizeFirstLetter(str: string): string { 
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase(); 
} 

function removeSpaces(str: string): string { 
    return str.replace(/\s+/g, ''); 
} 

function truncate(str: string, length: number): string { 
    return str.length > length ? str.slice(0, length) + '...' : str; 
} 

const formatUserInput = (input: string): string => { 
    return truncate(removeSpaces(capitalizeFirstLetter(input)), 10); 
}; 

console.log(formatUserInput(" john doe ")); // Output: "Johndoe..." 
console.log(formatUserInput("ALICE IN WONDERLAND")); // Output: "Aliceinwo..."
```

##### Uso de una utilidad de composición

`function-composition.ts`:
```typescript
function compose<T>(...fns: Array<(arg: T) => T>) { 
    return (x: T) => fns.reduceRight((acc, fn) => fn(acc), x) 
} 

const formatName = compose( 
    (s: string) => truncate(s, 10), 
    removeSpaces, 
    capitalizeFirstLetter 
); 

console.log(formatName(" john doe ")); // Output: "Johndoe..." 
console.log(formatName("ALICE IN WONDERLAND")); // Output: "Aliceinwo..."
```

##### Currificación (*Currying*) para una mejor composición

La currificación transforma una función que toma múltiples argumentos en una secuencia de funciones que toman un solo argumento cada una:

`function-composition.ts`:
```typescript
function curry<T, U, V>(fn: (a: T, b: U) => V): (a: T) => (b: U) => V { 
    return (a: T) => (b: U) => fn(a, b) 
} 

const curriedTruncate = curry(truncate) 
const formatAndTruncate = compose((s: string) => curriedTruncate(s)(7), removeSpaces, capitalizeFirstLetter) 

console.log(formatAndTruncate(" john doe ")) // Output: "Johndoe..." 
console.log(formatAndTruncate("ALICE IN WONDERLAND")) // Output: "Alicein..."
```

#### Transparencia referencial

La transparencia referencial significa que una expresión o llamada a una función puede reemplazarse por su valor resultante sin cambiar el comportamiento del programa.

Ejemplo de función **sin** transparencia referencial (muta la entrada):

`referential.ts`:
```typescript
function sortList(list: number[]): number[] { 
    return list.sort((a, b) => a - b) 
} 

let originalList = [3, 1, 4, 1, 5, 9] 
let sortedList = sortList(originalList) 
console.log(sortedList) // [1, 1, 3, 4, 5, 9] 
console.log(originalList) // [1, 1, 3, 4, 5, 9] - Original list is mutated!
```

Ejemplo de función **con** transparencia referencial:

`referential.ts`:
```typescript
function pureSort(list: number[]): number[] { 
    return [...list].sort((a, b) => a - b) 
} 

let numbers = [3, 1, 4, 1, 5, 9] 
let sorted1 = pureSort(numbers) 
let sorted2 = pureSort(numbers) 
console.log(sorted1) // [1, 1, 3, 4, 5, 9] 
console.log(sorted2) // [1, 1, 3, 4, 5, 9] 
console.log(numbers) // [3, 1, 4, 1, 5, 9] - Original list remains unchanged
```

Composición funcional con Ramda:

`referential.ts`:
```typescript
import * as R from "ramda" 

export interface IO<A> { 
    (): A 
} 

const log = (s: unknown): IO<void> => () => console.log(s) 

function main(): IO<void> { 
    return R.compose(log, sumList, getArgs)(11, 4) 
} 

function sumList(numbers: number[]): number { 
    return numbers.reduce((prev, curr) => prev + curr, 0) 
} 

function getArgs(a: number, b: number): number[] { 
    return [a, b] 
} 

console.log(main()()) // 15
```

Reemplazando `sumList` directamente con su valor (15):

`referential.ts`:
```typescript
function main(): IO<(a, b) => void> { 
    return R.compose(log, 15, getArgs)(11, 4); 
} 

console.log(main()()); // 15
```

#### Inmutabilidad

La inmutabilidad es el principio de no permitir que una variable o un objeto cambie una vez definido e inicializado.

##### Inmutabilidad básica con `const`

`immutability.ts`:
```typescript
const name = "Alice"; 
name = "Bob"; // Error: Cannot assign to 'name' because it is a constant. 

const numbers = [1, 2, 3]; 
numbers.push(4); // This is allowed and modifies the array 
numbers = [5, 6, 7]; // Error: Cannot assign to 'numbers' because it is a constant.
```

##### Tipos de solo lectura (`Readonly`)

`immutability.ts`:
```typescript
interface User { 
    name: string; 
    age: number; 
} 

const user: Readonly<User> = { 
    name: "Alice", 
    age: 30 
}; 

user.age = 31; // Error: Cannot assign to 'age' because it is a read-only property.
```

##### Inmutabilidad profunda (*Deep immutability*)

`immutability.ts`:
```typescript
type DeepReadonly<T> = T extends (infer R)[] 
    ? ReadonlyArray<DeepReadonly<R>> 
    : T extends Function 
    ? T 
    : T extends object 
    ? {readonly [K in keyof T]: DeepReadonly<T[K]>} 
    : T; 

interface Department { 
    name: string; 
    employees: {id: number, name: string}[]; 
} 

const dept: DeepReadonly<Department> = { 
    name: "Engineering", 
    employees: [{id: 1, name: "Alice"}, {id: 2, name: "Bob"}] 
}; 

dept.name = "Sales"; // Error 
dept.employees.push({id: 3, name: "Charlie"}); // Error 
dept.employees[0].name = "Alicia"; // Error
```

Inmutabilidad en clases:

`immutability.ts`:
```typescript
class ImmutablePerson { 
    readonly #name: string; 
    readonly #age: number; 

    constructor(name: string, age: number) { 
        this.#name = name; 
        this.#age = age; 
    } 

    get name(): string { 
        return this.#name; 
    } 

    get age(): number { 
        return this.#age; 
    } 

    withAge(newAge: number): ImmutablePerson { 
        return new ImmutablePerson(this.#name, newAge); 
    } 
} 

const person1 = new ImmutablePerson("Alice", 30); 
const person2 = person1.withAge(31); 

console.log(person1.age); // 30 
console.log(person2.age); // 31
```

##### Uso de Immutable.js

`immutability.ts`:
```typescript
import { List, Map } from 'immutable'; 

const list1 = List([1, 2, 3]); 
const list2 = list1.push(4); 
console.log(list1.toArray()); // [1, 2, 3] 
console.log(list2.toArray()); // [1, 2, 3, 4] 

const map1 = Map({ a: 1, b: 2 }); 
const map2 = map1.set('b', 3); 
console.log(map1.toObject()); // { a: 1, b: 2 } 
console.log(map2.toObject()); // { a: 1, b: 3 } 

(list1 as any).push(5); 
console.log(list1.toArray()); // [ 1, 2, 3 ]
```

---

### Comprensión de las lentes funcionales (*Functional Lenses*)

Una lente funcional es un par ordenado de funciones *getter* y *setter* que permite acceder y actualizar de forma inmutable una parte de una estructura de datos compleja (*Figura 7.2: Visualización de un patrón de lente que enfoca los datos de dirección anidados del usuario a través de UserLens y AddressLens*).

#### Cuándo usar lentes funcionales

- **Modificar objetos de datos complejos:** Al interactuar con estructuras profundamente anidadas sin necesidad de realizar recorridos manuales engorrosos.
- **Mantener la inmutabilidad:** Para crear copias modificadas de objetos anidados sin mutar los originales.
- **Transformaciones de datos más limpias:** Para enfocar y transformar partes específicas de una estructura de datos de forma componible.

#### Implementación de lentes

`lenses.ts`:
```typescript
export interface Lens<T, A> { 
    get: (obj: T) => A 
    set: (obj: T) => (newValue: A) => T 
}
```

Función auxiliar `lensProp`:

`lens.ts`:
```typescript
function lensProp<T, K extends keyof T>(key: K): Lens<T, T[K]> { 
    return { 
        get: (obj: T): T[K] => obj[key], 
        set: (obj: T) => (value: T[K]): T => ({ ...obj, [key]: value }), 
    } 
}
```

Uso básico:

`lens.ts`:
```typescript
interface Person { 
    name: string 
    age: number 
    email: string 
} 

const person: Person = { 
    name: "John", 
    age: 30, 
    email: "john@example.com", 
} 

const ageLens = lensProp<Person, "age">("age") 
const currentAge = ageLens.get(person); 
console.log(currentAge); // Output: 30 
const updatedPerson = ageLens.set(person)(35); 
console.log(updatedPerson);
```

Funciones de utilidad `view`, `set` y `over`:

`lens.ts`:
```typescript
function view<T, A>(lens: Lens<T, A>, obj: T): A { 
    return lens.get(obj) 
} 

function set<T, A>(lens: Lens<T, A>, obj: T, value: A): T { 
    return lens.set(obj)(value) 
} 

function over<T, A, B>(lens: Lens<T, A>, f: (x: A) => A, obj: T) { 
    return lens.set(obj)(f(lens.get(obj))) 
}
```

Uso de `over`:

`lens.ts`:
```typescript
const increaseAge = over(ageLens, (val: number) => val + 1, person) 
console.log(view(ageLens, increaseAge)) // 31
```

#### Casos de uso de lentes

Gestión del estado de una lista de tareas (*To-Do list*):

`lens.ts`:
```typescript
interface TodoItem { 
    id: string 
    title: string 
    completed: boolean 
} 

interface TodoListState { 
    allItemIds: string[] 
    byItemId: { id: TodoItem } 
}
```

Actualización inmutable del estado usando lentes:

`lens.ts`:
```typescript
interface UpdateTodoItemCompletedAction { 
    type: "UPDATE_TODO_ITEM_COMPLETED" 
    id: string 
    completed: boolean 
} 

const byItemIdLens = lensProp<TodoListState, 'byItemId'>('byItemId'); 

function todoItemLens(id: string): Lens<{ [key: string]: TodoItem; }, TodoItem> { 
    return lensProp<{ [key: string]: TodoItem }, string | number>(id as keyof { [key: string]: TodoItem }); 
} 

const completedLens = lensProp<TodoItem, 'completed'>('completed'); 

function reduceState(currentState: TodoListState, action: UpdateTodoItemCompletedAction): TodoListState { 
    switch (action.type) { 
        case "UPDATE_TODO_ITEM_COMPLETED": 
            const itemLens = todoItemLens(action.id); 
            const currentTodoItem = view(itemLens, currentState.byItemId); 
            const updatedTodoItem = over(completedLens, () => action.completed, currentTodoItem); 
            const updatedByItemId = { 
                ...currentState.byItemId, 
                [action.id]: updatedTodoItem, 
            }; 
            // Return the new state with updated byItemId 
            return set(byItemIdLens, currentState, updatedByItemId); 
    } 
    return currentState; 
}
```

Ejecución:

`lens.ts`:
```typescript
const initialState: TodoListState = { 
    byItemId: { 
        '1': { id: '1', title: 'Learn TypeScript', completed: false }, 
        '2': { id: '2', title: 'Build a project', completed: false }, 
    }, 
}; 

const action: UpdateTodoItemCompletedAction = { 
    type: "UPDATE_TODO_ITEM_COMPLETED", 
    id: '1', 
    completed: true, 
}; 

const newState = reduceState(initialState, action); 
console.log(newState);
```

---

### Exploración de estructuras funcionales prácticas

#### Funtores (*Functors*)

Un funtor es una estructura que contiene un valor y proporciona una función `map` para transformar ese valor sin alterar la estructura del contenedor.

`functional-structures.ts`:
```typescript
class Box<T> { 
    constructor(private value: T) {} 

    map<U>(f: (value: T) => U): Box<U> { 
        return new Box(f(this.value)); 
    } 

    toString(): string { 
        return `Box(${this.value})`; 
    } 
} 

const box = new Box(5); 
const result = box.map(x => x * 2).map(x => x + 1); 
console.log(result.toString()); // Box(11)
```

Funtores en `fp-ts`:

`functional-structures.ts`:
```typescript
import { pipe } from "fp-ts/function" 
import * as A from 'fp-ts/Array' 

const numbers = [1, 2, 3, 4] 
const doubleArray = pipe( 
    numbers, 
    A.map(n => n * 2) 
) // Output: [2, 4, 6, 8]
```

#### Aplicativos (*Applicatives*)

Un aplicativo extiende a un funtor permitiendo aplicar una función envuelta en un contenedor a un valor envuelto en otro contenedor a través del método `ap`:

`functional-structures.ts`:
```typescript
interface Applicative<T> { 
    map<U>(f: (value: T) => U): Applicative<U> 
    ap<U>(f: Applicative<(value: T) => U>): Applicative<U> 
} 

class Maybe<T> implements Applicative<T> { 
    private constructor(private value: T | null) {} 

    static just<T>(value: T): Maybe<T> { 
        return new Maybe(value) 
    } 

    static nothing<T>(): Maybe<T> { 
        return new Maybe<T>(null) 
    } 

    map<U>(f: (value: T) => U): Maybe<U> { 
        return this.value === null ? Maybe.nothing() : Maybe.just(f(this.value)) 
    } 

    ap<U>(f: Maybe<(value: T) => U>): Maybe<U> { 
        if (this.value === null || f.value === null) { 
            return Maybe.nothing<U>() 
        } 
        return Maybe.just(f.value(this.value)) 
    } 

    getOrElse(defaultValue: T): T { 
        return this.value !== null ? this.value : defaultValue 
    } 
}
```

Uso con `fp-ts`:

`functional-structures.ts`:
```typescript
import { pipe } from 'fp-ts/function'; 
import * as O from 'fp-ts/Option'; 
import { sequenceT } from 'fp-ts/Apply'; 

const add = (a: number) => (b: number) => a + b; 
const maybeNumber1 = O.some(5); 
const maybeNumber2 = O.some(10); 
const maybeAdd = O.some(add); 

const result2 = pipe( 
    sequenceT(O.option)(maybeAdd, maybeNumber1, maybeNumber2), 
    O.map(([fn, a, b]) => fn(a)(b)) 
); 

console.log(O.getOrElse(() => 0)(result2)); // Should output 15
```

#### Semigrupos (*Semigroups*)

Un semigrupo es una estructura algebraica que consiste en un conjunto de valores y una operación binaria asociativa (`concat`).

$$(a \times b) \times c = a \times (b \times c)$$

`functional-structures.ts`:
```typescript
interface Semigroup<T> { 
    concat(other: T): T 
} 

class Sum implements Semigroup<Sum> { 
    constructor(public value: number) {} 
    concat(other: Sum): Sum { 
        return new Sum(this.value + other.value) 
    } 
} 

class Str implements Semigroup<Str> { 
    constructor(public value: string) {} 
    concat(other: Str): Str { 
        return new Str(this.value + other.value) 
    } 
} 

// Generic function to combine a list of semigroups 
function concatAll<T extends Semigroup<T>>(xs: T[]): T { 
    return xs.reduce((acc, x) => acc.concat(x)) 
} 

// Usage examples 
const sums = [new Sum(1), new Sum(2), new Sum(3)] 
console.log(concatAll(sums).value) // Output: 6 

const strings = [new Str("Hello, "), new Str("functional "), new Str("programming!")] 
console.log(concatAll(strings).value) // Output: "Hello, functional programming!"
```

Semigrupos en `fp-ts`:

`functional-structures.ts`:
```typescript
const numberSum = N.SemigroupSum 
const result1 = numberSum.concat(1, 2) 

const stringConcat = S.Semigroup 
const result2 = stringConcat.concat('Hello ', 'World')
```

#### Monoides (*Monoids*)

Un monoide es un semigrupo que incluye un elemento de identidad (`identity` o valor neutro):

$$(a \times 1) = a \quad \text{y} \quad (1 \times a) = a$$

`functional-structures.ts`:
```typescript
interface Monoid<T> extends Semigroup<T> { 
    identity(): T; 
} 

class Product implements Semigroup<Product>, Monoid<Product> { 
    constructor(public value: number) {} 

    concat(other: Product): Product { 
        return new Product(this.value * other.value); 
    } 

    identity(): Product { 
        return new Product(1); // 1 is the identity element for multiplication 
    } 
} 

// Example usage: 
const a = new Product(2); 
const b = new Product(3); 
const c = new Product(4); 
const result = a.concat(b).concat(c); // (2 * 3) * 4 
console.log(result.value); // Output: 24
```

Monoides en `fp-ts`:

`functional-structures.ts`:
```typescript
import { pipe } from "fp-ts/function" 
import * as A from 'fp-ts/Array' 
import * as N from 'fp-ts/number' 

const numberMonoid = N.MonoidSum; 
const numbers2 = [1, 2, 3, 4] 
const sum = pipe( 
    numbers2, 
    A.foldMap(numberMonoid)(n => n) 
)
```

#### Recorribles (*Traversables*)

Un recorrible (*traversable*) es una estructura que aplica una función a cada elemento y recopila los resultados en una nueva estructura aplanada.

`functional-structures.ts`:
```typescript
interface Traversable<T> { 
    traverse<A>(fn: (item: T) => A[]): A[]; 
} 

class TraversableList<T> implements Traversable<T> { 
    constructor(private items: T[]) {} 

    // Traverse method applies the function to each element and collects the results in an array 
    traverse<A>(fn: (item: T) => A[]): A[] { 
        return this.items.flatMap(fn) 
    } 
} 

// Example usage: 
const list = new TraversableList<number>([1, 2, 3]) 

// Function to apply 
const expand = (num: number): number[] => [num, num * 2] 

// Applying the function using traverse 
const result4 = list.traverse(expand) 
console.log(result) // Output: [1, 2, 2, 4, 3, 6]
```

Traversables en `fp-ts`:

`functional-structures.ts`:
```typescript
const maybeNumbers = [O.some(1), O.some(2), O.none, O.some(4)] 
const traverseResult = pipe( 
    maybeNumbers, 
    A.traverse(O.Applicative)((n) => n) 
)
```

---

### Comprensión y utilización de mónadas (*Monads*)

Una mónada es una estructura abstracta que encapsula cálculos y permite su composición de forma consistente, manejando efectos secundarios, valores opcionales y secuencias de operaciones (*Figura 7.4: Flujo de trabajo de la mónada Maybe que muestra cómo maneja valores opcionales con transformaciones seguras*).

Funciones básicas:

`monads.ts`:
```typescript
function add2(x: number): number { 
    return x + 2; 
} 

function mul3(x: number): number { 
    return x * 3; 
} 

console.log(mul3(add2(2))); // (2 + 2) * 3 = 12 
console.log(add2(mul3(2))); // (2 * 3) + 2 = 8
```

Funciones con valores opcionales:

`monads.ts`:
```typescript
function safeDivide(x: number, y: number): number | null { 
    return y !== 0 ? x / y : null; 
} 

function safeSquareRoot(x: number): number | null { 
    return x >= 0 ? Math.sqrt(x) : null; 
}
```

Componer estas funciones sin mónadas conduce a comprobaciones de `null` anidadas y código engorroso:

`monads.ts`:
```typescript
const result = safeDivide(16, 4); 
if (result !== null) { 
    const sqrtResult = safeSquareRoot(result); 
    if (sqrtResult !== null) { 
        console.log(sqrtResult); 
    } 
}
```

#### Introducción a la mónada Maybe

`monads.ts`:
```typescript
class Maybe<T> { 
    private constructor(private value: T | null) {} 

    static just<T>(value: T): Maybe<T> { 
        return new Maybe(value); 
    } 

    static nothing<T>(): Maybe<T> { 
        return new Maybe<T>(null); 
    } 

    map<U>(fn: (value: T) => U): Maybe<U> { 
        return this.value === null ? Maybe.nothing() : Maybe.just(fn(this.value)); 
    } 

    flatMap<U>(fn: (value: T) => Maybe<U>): Maybe<U> { 
        return this.value === null ? Maybe.nothing() : fn(this.value); 
    } 
}
```

Reescritura de funciones con `Maybe`:

`monads.ts`:
```typescript
function safeDivide(x: number, y: number): Maybe<number> { 
    return y !== 0 ? Maybe.just(x / y) : Maybe.nothing(); 
} 

function safeSquareRoot(x: number): Maybe<number> { 
    return x >= 0 ? Maybe.just(Math.sqrt(x)) : Maybe.nothing(); 
} 

const result = safeDivide(16, 4).flatMap(safeSquareRoot); 
console.log(result); // Maybe { value: 2 } 

const invalidResult = safeDivide(16, 0).flatMap(safeSquareRoot); 
console.log(invalidResult); // Maybe { value: null }
```

#### Cuándo usar mónadas

##### Manejo de errores (Mónada Either)

`monads.ts`:
```typescript
class Either<L, R> { 
    private constructor(private left: L | null, private right: R | null) {} 

    static left<L, R>(value: L): Either<L, R> { 
        return new Either<L, R>(value, null); 
    } 

    static right<L, R>(value: R): Either<L, R> { 
        return new Either<L, R>(null, value); 
    } 
    // more methods here 
} 

function divide(a: number, b: number): Either<string, number> { 
    return b === 0 ? Either.left("Division by zero") : Either.right(a / b); 
} 

function squareRoot(n: number): Either<string, number> { 
    return n < 0 ? Either.left("Cannot calculate square root of negative number") : Either.right(Math.sqrt(n)); 
} 

const result = divide(10, 2).flatMap(squareRoot);
```

##### Gestión de estado (Mónada State)

`monads.ts`:
```typescript
class State<S, A> { 
    constructor(public run: (s: S) => [A, S]) {} 

    static of<S, A>(a: A): State<S, A> { 
        return new State(s => [a, s]); 
    } 

    map<B>(f: (a: A) => B): State<S, B> { 
        return new State(s => { 
            const [a, s1] = this.run(s); 
            return [f(a), s1]; 
        }); 
    } 

    flatMap<B>(f: (a: A) => State<S, B>): State<S, B> { 
        return new State(s => { 
            const [a, s1] = this.run(s); 
            return f(a).run(s1); 
        }); 
    } 
} 

const increment = new State<number, void>(s => [undefined, s + 1]); 
const getCount = new State<number, number>(s => [s, s]); 

const program = increment.flatMap(() => increment) 
    .flatMap(() => getCount); 

const [count, finalState] = program.run(0); 
console.log(count, finalState); // 2, 2
```

#### Comprensión de las leyes de las mónadas (*Monad laws*)

Una estructura monádica debe satisfacer tres leyes fundamentales:

1. **Identidad por la izquierda (*Left identity*):**
   ```text
   M.of(a).flatMap(f) === f(a)
   ```
2. **Identidad por la derecha (*Right identity*):**
   ```text
   m.flatMap(M.of) === m
   ```
3. **Asociatividad (*Associativity*):**
   ```text
   m.flatMap(f).flatMap(g) === m.flatMap(x => f(x).flatMap(g))
   ```

Comprobación en código con la mónada `Maybe`:

`monads.ts`:
```typescript
// Left Identity 
const a = 5 
const f = (x: number) => Maybe.just(x * 2) 
console.log(Maybe.just(a).flatMap(f).equals(f(a))) // true 

// Right Identity 
const m = Maybe.just(3) 
console.log(m.flatMap(Maybe.just).equals(m)) // true 

// Associativity 
const g = (x: number) => Maybe.just(x + 1) 
const h = (x: number) => Maybe.just(x * 3) 
const m1 = Maybe.just(2).flatMap(g).flatMap(h) 
const m2 = Maybe.just(2).flatMap((x) => g(x).flatMap(h)) 
console.log(m1.equals(m2)) // true
```

---

### Resumen

En este capítulo exploramos los conceptos fundamentales de la programación funcional en TypeScript:
- **Pureza, inmutabilidad y transparencia referencial:** Reducen los efectos secundarios y facilitan el razonamiento matemático sobre el código.
- **Funciones como ciudadanos de primera clase y composición:** Permiten construir tuberías modulares y reutilizables mediante currificación y funciones de orden superior.
- **Lentes funcionales (*Lenses*):** Abstraen *getters* y *setters* para leer y actualizar estructuras profundamente anidadas sin romper la inmutabilidad.
- **Estructuras algebraicas y Mónadas:** Los funtores, aplicativos, semigrupos, monoides, recorribles y mónadas (`Maybe`, `Either`, `State`) proporcionan abstracciones formales y seguras para el manejo de colecciones, efectos secundarios y control de errores.

En el siguiente capítulo, exploraremos cómo la programación reactiva y asíncrona nos ayuda a construir sistemas escalables, guiados por eventos y tolerantes a fallos.
