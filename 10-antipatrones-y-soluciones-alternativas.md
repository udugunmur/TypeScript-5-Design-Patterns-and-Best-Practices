# Parte 3: Conceptos avanzados de TypeScript y mejores prácticas

## Capítulo 10: Antipatrones y soluciones alternativas

Así como existen buenos patrones de diseño y mejores prácticas al utilizar TypeScript, también existen antipatrones. Cuando se trabaja en aplicaciones a gran escala, inevitablemente te encontrarás con algunos patrones o partes de código que parecen problemáticas, son difíciles de leer y cambiar, o promueven comportamientos peligrosos. Esto sucede porque a medida que la aplicación crece, necesitas escribir más código que encaje en la base existente y, con bastante frecuencia, debes hacer ciertas concesiones.

Con el tiempo, a medida que más personas contribuyen al mismo espacio de código, verás muchas inconsistencias: objetos monolíticos (*God objects*), uso inconsistente de patrones o incluso un uso excesivo de tipos `any`. Los objetos monolíticos son clases u objetos que saben demasiado o hacen demasiado, violando el Principio de Responsabilidad Única (SRP) e introduciendo complejidad. En este capítulo, veremos enfoques para solucionar estos problemas.

Exploraremos varios aspectos importantes del sistema de tipos de TypeScript y las mejores prácticas para mantener la calidad del código. Analizaremos las dificultades del uso excesivo de clases y la herencia, y proporcionaremos estrategias para evitar estos problemas comunes. Luego, discutiremos los peligros de usar tipos permisivos o incorrectos que pueden provocar errores en tiempo de ejecución, así como los desafíos asociados con el uso de código idiomático de otros lenguajes.

Además, enumeraremos las trampas de inferencia de tipos, destacando sus limitaciones y cómo los malentendidos pueden generar tipos frágiles o comportamientos incorrectos en la verificación de tipos. Finalmente, identificaremos cómo los genéricos complejos pueden dificultar la lectura y el mantenimiento del código, junto con las mejores prácticas para su uso efectivo.

Cubriremos los siguientes temas en este capítulo:

- Uso excesivo de clases (*Class overuse*)
- Tipos permisivos o incorrectos
- Uso de código idiomático de otros lenguajes
- Trampas en la inferencia de tipos (*Type inference gotchas*)
- Trampas con genéricos (*Generic gotchas*)

Al final de este capítulo, podrás reconocer los antipatrones más importantes y proporcionar soluciones alternativas cuando sea necesario.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub aquí:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter10_Anti_patterns

---

### Uso excesivo de clases

Los principios de la Programación Orientada a Objetos (POO) y los patrones de diseño fomentan el modelado de entidades del mundo real mediante clases. Si bien los beneficios de la POO son bien reconocidos, a menudo conduce a una sobreabundancia de clases, creando complicaciones en la estructura del código.

Cuando intentas emular un sistema utilizando técnicas clásicas de POO como la herencia y la encapsulación, inevitablemente tienes que arrastrar toda la jerarquía de clases.

#### El problema de la jungla

El problema del "plátano, el mono y la jungla" (*the banana, monkey, jungle problem*) ejemplifica esto: si deseas un objeto `Banana`, es posible que debas importar un objeto `Jungle` que contiene una instancia de `Monkey`, la cual expone el método `getBanana()`:

```typescript
new Jungle().getAnimalByType("Monkey").getBanana();
```

Esto ilustra cómo la estructura de clases puede volverse enrevesada, generando dependencias innecesarias y complejidad.

Una solución adecuada es **favorecer la composición sobre la herencia**. El objeto `Jungle` puede representar un entorno o contexto donde coexisten varios animales y frutas, en lugar de estar estrechamente acoplado con comportamientos animales específicos:

```typescript
class Jungle { 
    constructor(private animal: Animal, private fruit: Fruit) {} 
    feedAnimals() { 
        this.animal.eat(this.fruit); 
    } 
} 

const jungle = new Jungle(); 
const monkey = new Monkey(); 
const banana = new Banana(); 
jungle.addAnimal(monkey); 
jungle.addFruit(banana); 
jungle.feedAnimals(); // Outputs: The monkey eats a banana.
```

La composición permite construir comportamientos complejos combinando componentes más simples que se pueden intercambiar o modificar fácilmente sin alterar toda la estructura.

#### Ejemplo de uso excesivo de clases

Consideremos una clase `CSV` que implementa dos interfaces: `Reader` para leer archivos y `Writer` para escribir en ellos:

`Class-overuse.ts`:
```typescript
interface Reader { 
    read(): string[] 
} 

interface Writer { 
    write(input: string[]): void 
} 

class CSV implements Reader, Writer { 
    constructor(private csvFilePath: string) {} 

    read(): string[] { 
        // Logic to read CSV 
        return ["data1", "data2"] 
    } 

    write(input: string[]): void { 
        // Logic to write to CSV 
    } 
}
```

A medida que definimos nuevas clases que solo reutilizan parcialmente la funcionalidad, introducimos un acoplamiento estrecho:

`Class-overuse.ts`:
```typescript
class ExcelToCSV extends CSV { 
    constructor( 
        csvFilePath: string, 
        private excelFilePath: string, 
    ) { 
        super(csvFilePath) 
    } 

    read(): string[] { 
        // Logic to read from Excel file 
        return ["excelData1", "excelData2"] 
    } 
} 

class ExcelToPDF extends ExcelToCSV { 
    constructor( 
        csvFilePath: string, 
        excelFilePath: string, 
        private pdfFilePath: string, 
    ) { 
        super(csvFilePath, excelFilePath) 
    } 

    write(input: string[]): void { 
        // Logic to write to PDF 
    } 
}
```

Esta dependencia parcial crea un acoplamiento estrecho entre todas las clases y rompe el Principio de Responsabilidad Única (SRP).

Para mejorar esto mediante composición sobre herencia, separamos las funcionalidades en clases dedicadas que implementan interfaces individuales:

`Class-overuse.ts`:
```typescript
class CSVReader implements Reader { 
    constructor(private csvFilePath: string) {} 
    read(): string[] { 
        // Logic to read CSV 
        return ["data1", "data2"] 
    } 
} 

class CSVWriter implements Writer { 
    write(input: string[]): void { 
        // Logic to write to CSV 
    } 
} 

class ExcelReader implements Reader { 
    constructor(private excelFilePath: string) {} 
    read(): string[] { 
        // Logic to read from Excel file 
        return ["excelData1", "excelData2"] 
    } 
} 

class PDFWriter implements Writer { 
    write(input: string[]): void { 
        // Logic to write to PDF 
    } 
}
```

Ahora combinamos lectores y escritores de forma desacoplada:

`Class-overuse.ts`:
```typescript
class ReaderToWriters { 
    constructor(private reader: Reader, private writers: Writer[]) {} 

    perform() { 
        const lines = this.reader.read(); 
        this.writers.forEach(writer => writer.write(lines)); 
    } 
}
```

Este estilo de reutilización se conoce como **reutilización de caja negra (*black-box reuse*)**, cumple con los principios SOLID y hace que el código sea más fácil de entender y extender.

#### Uso de interfaces para modelos

Al definir modelos o entidades en TypeScript, considera usar interfaces en lugar de clases completas cuando solo representen datos:

`Class-overuse.ts`:
```typescript
interface Configuration { 
    paths: { 
        apiBase: string 
        login: string 
    } 
} 

const applicationConfig: Configuration = { 
    paths: { 
        apiBase: "/v1", 
        login: "/login", 
    }, 
} 

function updateEmployee(employee: Employee, updates: Partial<Employee>): Employee { 
    return { ...employee, ...updates }; 
}
```

Aprovechando tipos de utilidad como `Readonly` y `Partial`:

`Class-overuse.ts`:
```typescript
interface Project { 
    id: number; 
    name: string; 
    description?: string; 
} 

type ReadonlyProject = Readonly<Project>; 
type PartialProject = Partial<Project>; 

const initialProject: ReadonlyProject = { id: 1, name: "TypeScript Guide" }; 

const updatedProject: Project = { 
    ...initialProject, 
    description: "An updated TypeScript guide" 
};
```

---

### Tipos excesivamente permisivos o incorrectos

#### Errores comunes

##### Uso del tipo `any`

El tipo `any` desactiva completamente la verificación de tipos del compilador, trasladando los errores a tiempo de ejecución:

`Permissive-types.ts`:
```typescript
function processValue(value) { 
    console.log(value.toUpperCase()) // This will throw an error if value is not a string 
} 

processValue("hello") // Works fine 
processValue(123) // Runtime error: value.toUpperCase is not a function
```

> [!TIP]
> Como alternativa segura a `any`, utiliza `unknown`. Con `unknown`, TypeScript obliga a realizar comprobaciones de tipo (estrechamiento de tipos) antes de operar sobre el valor.

##### Uso del tipo `Function`

El tipo `Function` es un contenedor genérico permisivo que no define firmas de parámetros ni tipos de retorno específicos:

`Permissive-types.ts`:
```typescript
interface Callback { 
    onEvent: Function; // Permissive type 
} 

const callback1: Callback = { 
    onEvent: (a: string) => a.toUpperCase(), 
}; 

const callback2: Callback = { 
    onEvent: () => "Hello", 
}; 

const callback3: Callback = { 
    onEvent: () => 1, 
};
```

Definición de firmas de funciones precisas con genéricos:

`Permissive-types.ts`:
```typescript
interface Callback<T> { 
    onEvent: (arg: T) => void; 
} 

const stringCallback: Callback<string> = { 
    onEvent: (a) => console.log(a.toUpperCase()), 
}; 

const numberCallback: Callback<number> = { 
    onEvent: (n) => console.log(n * 2), 
}; 

stringCallback.onEvent("hello"); // Works fine 
numberCallback.onEvent(5); // Works fine 
// stringCallback.onEvent(5); // Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```

---

### Uso de código idiomático de otros lenguajes

Al hacer la transición a TypeScript desde otros lenguajes, los desarrolladores a menudo trasladan patrones que no son óptimos en el ecosistema TypeScript.

#### Desde el lenguaje Java

Los desarrolladores de Java suelen recurrir a clases de estilo POJO (*Plain Old Java Objects*) o JavaBeans con *getters* y *setters* repetitivos:

`Idiomatic-code.ts`:
```typescript
class Employee { 
    constructor(private id: string, private name: string) {} 

    getName(): string { 
        return this.name 
    } 

    setName(name: string) { 
        this.name = name 
    } 

    getId(): string { 
        return this.id 
    } 

    setId(id: string) { 
        this.id = id 
    } 
}
```

En TypeScript no se pueden declarar múltiples constructores sobrecargados con cuerpo independiente como en Java. Además, los *getters* y *setters* mecánicos añaden verbosidad innecesaria.

Serialización simple:
```typescript
console.log(JSON.stringify(new Employee("Theo", "1"))); //{"id":"Theo","name":"1"}
```

Inmutabilidad manual:
```typescript
const theo = new Employee("Theo", "1"); 
new Employee(theo.getName(), "2");
```

Enfoque idiomático en TypeScript usando interfaces, tipos inmutables y funciones de fábrica:

`Idiomatic-code.ts`:
```typescript
interface Employee { 
    readonly id: string; 
    readonly name: string; 
    readonly department: string; 
} 

function createEmployee(id: string, name: string, department: string): Employee { 
    return { id, name, department }; 
} 

function updateEmployee(employee: Employee, updates: Partial<Employee>): Employee { 
    return { ...employee, ...updates }; 
} 

const emp = createEmployee('1', 'John Doe', 'IT'); 
console.log(emp.name); // John Doe 
const updatedEmp = updateEmployee(emp, { department: 'HR' }); 
console.log(updatedEmp.department); // HR
```

#### Desde el lenguaje Go

En Go, el manejo de errores se realiza devolviendo tuplas de valores `[resultado, error]`:

`Idiomatic-code.ts`:
```typescript
function divideNumbers(a: number, b: number): [number | null, Error | null] { 
    if (b === 0) { 
        return [null, new Error("Division by zero")]; 
    } 
    return [a / b, null]; 
} 

const [result, err] = divideNumbers(10, 2); 
if (err !== null) { 
    console.error("Error:", err.message); 
} else { 
    console.log("Result:", result); 
}
```

En TypeScript, el enfoque idiomático estándar para el flujo de control sincrónico aprovecha excepciones con bloques `try/catch` o Promises/Async-Await:

`Idiomatic-code.ts`:
```typescript
function divideNumbers(a: number, b: number): number { 
    if (b === 0) { 
        throw new Error("Division by zero"); 
    } 
    return a / b; 
} 

try { 
    const result = divideNumbers(10, 2); 
    console.log("Result:", result); 
} catch (error) { 
    console.error("Error:", error.message); 
}
```

---

### Trampas en la inferencia de tipos

#### Antipatrón: Confiar excesivamente en el tipado implícito

Tipado explícito:
```typescript
const arr: number[] = [1,2,3]
```

Tipado implícito:
```typescript
const arr = [1,2,3]// type of arr inferred as number[]
```

Declaración sin inicialización (infección con `any`):
```typescript
let x; // fails with noImplicitAny flag enabled 
x = 2;
```

#### No utilizar aserciones `const` para tipos literales

Sin aserción `const`, TypeScript amplía los tipos literales a tipos primitivos generales (`string`):

`Type-inference.ts`:
```typescript
const colors = { 
    red: "#FF0000", 
    green: "#00FF00", 
    blue: "#0000FF" 
}; 

function getColor(color: "red" | "green" | "blue") { 
    return colors[color]; 
}
```

Con aserción `as const` para preservar los tipos literales exactos:

`Type-inference.ts`:
```typescript
const colors = { 
    red: "#FF0000", 
    green: "#00FF00", 
    blue: "#0000FF" 
} as const; 

function getColor(color: keyof typeof colors) { 
    return colors[color]; 
} 
// return type is of type "#FF0000" | "#00FF00" | "#0000FF"
```

---

### Trampas con genéricos

#### Nombres confusos de múltiples tipos genéricos

Uso de identificadores genéricos de una sola letra difíciles de leer:
```typescript
interface KeyValuePair<T, K> { 
    key: T; 
    value: K; 
}
```

Mejora con nombres descriptivos:
```typescript
interface KeyValuePair<TKey, TValue> { 
    key: TKey; 
    value: TValue; 
}
```

```typescript
interface ApiResponse<TData, TError> { 
    data: TData; 
    error?: TError; 
}
```

Declaración de tipos explícita en APIs públicas complejas:
```typescript
const items: number[] = [1, 2, 3]; // Explicit type for clarity
```

#### Tipos genéricos predeterminados excesivamente permisivos

`Generic-types.ts`:
```typescript
type Config<T = {}, U = {}> = { 
    ctx?: T 
    data?: U 
} 

const t: Config = { 
    ctx: {color: 'red'}, 
    data: {} 
}
```

Al asignar valores a `ctx`, TypeScript infiere `ctx` como `{}` debido a los valores predeterminados, arrojando errores al intentar acceder a `t.ctx.color`.

Solución con restricciones explícitas (`extends`):

`Generic-types.ts`:
```typescript
type WithColor = { color: string }; 

type Config<T extends WithColor, U = {}> = { 
    ctx?: T 
    data?: U 
} 

const t: Config<WithColor> = { 
    ctx: {color: 'red'}, 
    data: {} 
} 

if (t.ctx) { 
    t.ctx.color = 'blue' 
}
```

Restricciones con tipos de opciones:

`Generic-types.ts`:
```typescript
type FetchOptions<T extends { url: string }> = { 
    params: T; 
}; 

const options: FetchOptions<{ url: string; queryParams?: string }> = { 
    params: { 
        url: "/api/data", 
        queryParams: "id=123" 
    } 
};
```

#### Ignorar las características más recientes de TypeScript (`NoInfer`)

Función sin `NoInfer`:

`Generic-types.ts`:
```typescript
function find<T extends string>(heyStack: T[], needle: T): number { 
    return heyStack.indexOf(needle); 
}; 

console.log(find(["a","b","c"],"d"))
```

El compilador infiere `T` ampliándolo a `"a" | "b" | "c" | "d"`:
```typescript
function find<"a" | "b" | "c" | "d">(heyStack: ("a" | "b" | "c" | "d")[], needle: "a" | "b" | "c" | "d"): number
```

Uso de `NoInfer` (introducido en TypeScript 5.4) para bloquear la inferencia no deseada en parámetros específicos:

`Generic-types.ts`:
```typescript
function find<T extends string>(heyStack: T[], needle: NoInfer<T>): number { 
    return heyStack.indexOf(needle) 
} 

// Argument of type '"d"' is not assignable to parameter of type '"a" | "b" | "c"' 
// Incorrect usage: 
const invalidResult = find(["a", "b", "c"], "d"); // Type error due to NoInfer constraint
```

---

### Resumen

En este capítulo exploramos los antipatrones y trampas más importantes en TypeScript:
- **Uso excesivo de clases:** Preferir la composición sobre la herencia jerárquica rígida y utilizar interfaces ligeras para modelar datos.
- **Tipos permisivos:** Evitar `any` y `Function`, utilizando `unknown` y firmas genéricas estrictas.
- **Idiomas foráneos:** Evitar patrones POJO de Java o retornos múltiples de Go, adoptando los mecanismos nativos de TypeScript y JavaScript.
- **Inferencia y genéricos:** Usar aserciones `as const`, nombres descriptivos en genéricos (`TKey`, `TValue`), restricciones claras (`extends`) y utilidades modernas como `NoInfer`.

En el último capítulo del libro, realizaremos una exploración detallada del uso de patrones de diseño en dos frameworks de código abierto ampliamente utilizados: **tRPC** y **Apollo Client**.

---

### Preguntas y respuestas

1. **¿Cuál es el propósito del tipo de utilidad `NoInfer` en TypeScript?**
   - **Respuesta:** El tipo de utilidad `NoInfer`, introducido en TypeScript 5.4, se utiliza para bloquear la inferencia de tipos en puntos específicos de una función o tipo, garantizando comprobaciones de tipos más estrictas y precisas basadas exclusivamente en los argumentos deseados.

2. **¿Cómo se define la reutilización de caja negra (*black-box reuse*) en el contexto de la composición de objetos?**
   - **Respuesta:** La reutilización de caja negra consiste en utilizar un componente basándose exclusivamente en su interfaz pública sin depender ni conocer los detalles de su implementación interna. Facilita las pruebas y el mantenimiento, y sigue de cerca el Principio de Sustitución de Liskov.
