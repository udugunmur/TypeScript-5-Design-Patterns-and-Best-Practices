# Parte 3: Conceptos avanzados de TypeScript y mejores prácticas

## Capítulo 9: Desarrollo de aplicaciones TypeScript modernas y robustas

Hasta ahora en este libro, hemos explorado patrones de diseño clásicos, programación funcional y metodologías reactivas. Al entrar en los últimos tres capítulos, nuestro enfoque cambiará hacia técnicas prácticas para construir aplicaciones TypeScript del mundo real, destacando las mejores prácticas y estrategias eficientes.

En este capítulo, examinaremos cómo combinar varios patrones de diseño para crear aplicaciones robustas mientras aprovechamos los potentes tipos y funciones de utilidad de TypeScript para hacer que tu código sea más limpio y mantenible. También introduciremos el Diseño Guiado por el Dominio (*Domain-Driven Design* o DDD), una metodología que enfatiza hacer coincidir la estructura de tu aplicación con las necesidades comerciales del negocio.

Luego, veremos la arquitectura Modelo-Vista-Controlador (MVC), un patrón ampliamente utilizado que mejora la organización del código mediante la separación de conceptos (*separation of concerns*). Además, cubriremos los principios SOLID, que proporcionan una guía clara para escribir código limpio, mantenible y escalable.

Cubriremos los siguientes temas principales en este capítulo:

- Combinación efectiva de patrones de diseño
- Aprovechamiento de los tipos de utilidad de TypeScript
- Implementación de DDD
- Adopción de los principios SOLID
- Aplicación de la arquitectura MVC

Al final de este capítulo, habrás adquirido conocimientos e ideas valiosas para desarrollar aplicaciones TypeScript del mundo real, integrando las mejores prácticas que funcionan bien en conjunto.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en el repositorio de GitHub de este libro:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter09_Best_practices

---

### Combinación efectiva de patrones de diseño

Los patrones de diseño son herramientas poderosas en el desarrollo de software, pero su verdadero potencial a menudo se hace realidad cuando se combinan estratégicamente.

Principios clave a tener en cuenta:
- **Propósito y ajuste (*Purpose and fit*):** Evalúa si la combinación de patrones de diseño se ajusta a los requisitos específicos de tu proyecto. No todas las combinaciones producen resultados beneficiosos; prioriza aquellas que aborden tus desafíos de manera efectiva.
- **Flexibilidad frente a complejidad:** Considera las implicaciones de la complejidad agregada frente a la flexibilidad deseada. Utiliza patrones de diseño que se complementen entre sí y agilicen la arquitectura del sistema en lugar de complicarla.
- **Capacidad de prueba (*Testability*):** Analiza cómo la combinación de patrones facilita la escritura y el mantenimiento de pruebas unitarias.

#### Combinación de Singleton con otros patrones

El patrón Singleton es muy flexible y se puede combinar con muchos otros patrones sin gran esfuerzo, siempre que se implemente correctamente.

##### Singleton con el patrón Builder

El objeto Builder suele ser una instancia única y solo debe usarse para crear un nuevo objeto. Sin embargo, antes de cada uso, el cliente debe restablecer el objeto Builder para limpiar cualquier estado interno de usos anteriores. La forma más sencilla de proporcionar un Builder Singleton es mediante una exportación predeterminada:

```typescript
export class PremiumWebsiteBuilder { 
    private state: State = initialState; 

    reset(): void { 
        // Reset internal state 
        this.state = initialState; 
    } 
    // Other builder methods... 
} 

export default new PremiumWebsiteBuilder()
```

Cualquier parte del código que importe esta exportación predeterminada recibirá una instancia Singleton vinculada a la versión del paquete.

##### Singleton con el patrón Façade

La combinación de los patrones Singleton y Façade es particularmente poderosa cuando necesitas proporcionar una interfaz simplificada y unificada para un subsistema complejo, asegurando al mismo tiempo que solo exista una instancia de esta interfaz en toda la aplicación.

`Singleton-facade.ts`:
```typescript
interface ServiceA {} 
interface ServiceB {} 

class SystemFacade { 
    private static instance: SystemFacade; 

    private constructor( 
        private serviceA: ServiceA, 
        private serviceB: ServiceB 
    ) {} 

    static getInstance(serviceA: ServiceA, serviceB: ServiceB): SystemFacade { 
        if (!SystemFacade.instance) { 
            SystemFacade.instance = new SystemFacade(serviceA, serviceB); 
        } 
        return SystemFacade.instance; 
    } 

    performComplexOperation(): void { 
    } 
} 

class ConcreteServiceA implements ServiceA {} 
class ConcreteServiceB implements ServiceB {} 

// Usage 
const facade = SystemFacade.getInstance( 
    new ConcreteServiceA(), 
    new ConcreteServiceB()
); 
facade.performComplexOperation();
```

> [!CAUTION]
> Centralizar demasiada complejidad en un Singleton-Façade puede crear un cuello de botella. A medida que el subsistema crece, esto puede generar problemas de rendimiento y dificultar la escalabilidad y el mantenimiento del sistema.

##### Singleton con los patrones Factory Method y Abstract Factory

En el contexto del patrón Abstract Factory, los métodos de fábrica no mantienen ningún estado interno, lo que los hace adecuados para implementarse como Singletons, evitando la creación innecesaria de múltiples instancias de fábrica.

Sin embargo, cuando las fábricas se implementan como Singletons, pueden limitar la flexibilidad en las pruebas porque el patrón Singleton no permite proporcionar simulaciones (*mocks*) o sustitutos (*stubs*) fácilmente, compartiendo la misma instancia entre todas las pruebas unitarias.

##### Singleton con el patrón State

Dado que puedes crear un objeto State una vez y compartirlo con el objeto Originator/Context, puedes convertirlo en un Singleton para que todos los objetos de contexto compartan una única instancia del estado al mismo tiempo.

#### Combinación de Iterator con otros patrones

##### Iterator con el patrón Composite

La combinación de los patrones Iterator y Composite ofrece un enfoque excelente para gestionar estructuras jerárquicas complejas. El patrón Composite permite la creación de estructuras de árbol que representan jerarquías de parte-todo, mientras que el patrón Iterator ofrece una forma de acceder a los elementos de un objeto agregado en secuencia sin exponer su representación interna.

`Iterator-composite.ts`:
```typescript
const root = new Composite("Root") 
root.add(new Composite("Child1")) 
root.add(new Composite("Child2")) 

const iterator = root.createIterator(); 
while (iterator.hasNext()) { 
    const component = iterator.next(); 
    if (component) { 
        console.log(component.getName()); 
    } 
}
```

> [!TIP]
> Al implementar esta combinación en estructuras profundas, es beneficioso considerar estrategias de almacenamiento en caché (*caching*) o memorización (*memoization*) para reducir la necesidad de recorrer repetidamente las mismas rutas en la jerarquía.

##### Iterator con el patrón Visitor

Permite realizar operaciones sobre los elementos de una colección sin tener que modificar las clases de la colección en sí:

`Iterator-visitor.ts`:
```typescript
const collection = new ElementCollection(); 
collection.add(new ElementA("Element A1")); 
collection.add(new ElementB("Element B1")); 
collection.add(new ElementA("Element A2")); 

const visitor = new ConcreteVisitor(); 
const iterator = collection.createIterator(); 

while (iterator.hasNext()) { 
    const element = iterator.next(); 
    if (element) { 
        element.accept(visitor); 
    } 
}
```

---

### Aprovechamiento de los tipos de utilidad de TypeScript

#### Composición de tipos de utilidad

Puedes combinar múltiples tipos de utilidad para crear transformaciones de tipos más complejas. Por ejemplo, eliminar el modificador `readonly` de las propiedades de un objeto:

`Utilities.ts`:
```typescript
type Mutable<T> = { 
    -readonly [K in keyof T]: Mutable<T[K]>; 
}; 

interface UserState { 
    readonly name: string; 
    readonly age: number; 
    readonly address: { 
        readonly street: string; 
        readonly city: string; 
    }; 
} 

const mutableUserState: Mutable<UserState> = { 
    name: "Alice", 
    age: 24, 
    address: { 
        street: "123 Main St", 
        city: "Wonderland" 
    } 
}; 

mutableUserState.age = 31; 
mutableUserProfile.address.city = "New Wonderland"; 

const newUserState: UserState = { ...mutableUserState };
```

#### Creación de tipos de utilidad personalizados

Uso avanzado de tipos mapeados condicionales para extraer claves opcionales y obligatorias:

`Utilities.ts`:
```typescript
type OptionalKeys<T> = { 
    [K in keyof T]-?: {} extends Pick<T, K> ? K : never 
}[keyof T]; 

type RequiredKeys<T> = { 
    [K in keyof T]-?: {} extends Pick<T, K> ? never : K 
}[keyof T]; 

interface User { 
    id: number; 
    name: string; 
    email?: string; 
} 

type UserOptionalKeys = OptionalKeys<User>; // "email" 
type UserRequiredKeys = RequiredKeys<User>; // "id" | "name"
```

> [!NOTE]
> El uso de tipos mapeados recursivos complejos con interfaces muy grandes puede ralentizar los tiempos de compilación de TypeScript debido a la verificación de tipos recursiva exhaustiva.

#### Aprovechamiento de tipos mapeados con tipos de utilidad

`Utilities.ts`:
```typescript
type Mutable<T> = { 
    -readonly [P in keyof T]: T[P] 
} 

type FunctionPropertyNames<T> = { 
    [K in keyof T]: T[K] extends Function ? K : never 
}[keyof T] 

type FunctionProperties<T> = Pick<T, FunctionPropertyNames<T>> 

interface Calculator { 
    readonly value: number 
    add: (n: number) => void 
    subtract: (n: number) => void 
} 

// Using FunctionPropertyNames to extract method names 
type Names = FunctionPropertyNames<Calculator>; // "subtract" | "add" 
type MutableCalculator = Mutable<Calculator> 
type CalculatorMethods = FunctionProperties<Calculator> 

const calc: MutableCalculator = { 
    value: 0, 
    add(n) { 
        this.value += n 
    }, 
    subtract(n) { 
        this.value -= n 
    }, 
} 

calc.value = 10 // This is now allowed
```

---

### Implementación de DDD (Domain-Driven Design)

El Diseño Guiado por el Dominio (DDD) representa un enfoque de desarrollo de software que permite traducir la lógica de negocio compleja en componentes de software que coincidan con su significado y propósito.

#### Los bloques de construcción de DDD

##### Contextos delimitados (*Bounded contexts*)

Definen límites lógicos claros dentro de los cuales es aplicable un modelo de dominio específico. En una plataforma de comercio electrónico, el subdominio de gestión de pedidos se puede dividir en:
- **Contexto de carrito de compras (*Shopping cart context*):** Gestiona los artículos que el usuario pretende comprar.
- **Contexto de procesamiento de pagos (*Payment processing context*):** Gestiona pasarelas de pago y transacciones.
- **Contexto de envío (*Shipping context*):** Supervisa la logística, direcciones y seguimiento de entregas.

En arquitecturas de microservicios, cada servicio puede actuar como un contexto delimitado independiente.

##### Lenguaje común o ubicuo (*Common language*)

Establece un vocabulario compartido entre desarrolladores, expertos en el dominio y analistas de negocio para evitar ambigüedades.

##### Entidades (*Entities*)

Objetos del dominio que poseen una identidad única persistente y son típicamente mutables (`id`, `created_at`, `updated_at`).

##### El patrón Repository (*The Repository pattern*)

Actúa como capa de abstracción entre la lógica del dominio y la capa de acceso a datos, proporcionando una interfaz similar a una colección.

##### Objetos de valor (*Value objects*)

Representan aspectos del dominio sin identidad conceptual (sin campo `id`). Son inmutables y dos instancias se consideran iguales si todos sus atributos coinciden:

`Ddd.ts`:
```typescript
class Money { 
    private readonly amount: number 
    private readonly currency: string 

    constructor(amount: number, currency: string) { 
        this.amount = amount 
        this.currency = currency 
        this.validate() 
    } 

    private validate(): void { 
        if (this.amount < 0) { 
            throw new Error("Amount cannot be negative") 
        } 
        if (this.currency.length !== 3) { 
            throw new Error("Currency must be a 3-letter ISO code") 
        } 
    } 

    private ensureSameCurrency(other: Money): void { 
        if (this.currency !== other.currency) { 
            throw new Error("Cannot perform operations on different currencies"); 
        } 
    } 

    public equals(other: Money): boolean { 
        return this.amount === other.amount && this.currency === other.currency; 
    } 

    public add(other: Money): Money { 
        this.ensureSameCurrency(other); 
        return new Money(this.amount + other.amount, this.currency); 
    } 

    public subtract(other: Money): Money { 
        this.ensureSameCurrency(other); 
        const resultAmount = this.amount - other.amount; 
        if (resultAmount < 0) { 
            throw new Error("Resulting amount cannot be negative"); 
        } 
        return new Money(resultAmount, this.currency); 
    } 
}
```

##### Eventos de dominio (*Domain events*)

Indican que algo significativo ha sucedido en el dominio para que otras partes del sistema reaccionen:

`ddd.ts`:
```typescript
const dispatcher = new EventDispatcher() 
const userService = new UserService(dispatcher) 

async function sendWelcomeEmail(event: UserRegisteredEvent) { 
    const maxRetries = 3; 
    for (let attempt = 1; attempt <= maxRetries; attempt++) { 
        try { 
            // Simulating sending an email that can fail 
            await sendEmail(event.email); 
            console.log(`Successfully sent welcome email to ${event.email}`); 
            return; 
        } catch (error) { 
            console.error(`Attempt ${attempt} failed to send email to ${event.email}:`, error); 
            if (attempt === maxRetries) { 
                console.error(`Failed to send welcome email after ${maxRetries} attempts.`); 
            } 
        } 
    } 
} 

async function notifyAdminOfNewUser(event: UserRegisteredEvent) { 
    console.log(`Notifying admin of new user: ${event.userId}`); 
} 

dispatcher.addListener(sendWelcomeEmail) 
dispatcher.addListener(notifyAdminOfNewUser) 

userService.registerUser("user123", "user@example.com")
```

#### Desventajas actuales de DDD

- **Inversión considerable de tiempo y recursos:** Requiere un esfuerzo importante para definir el lenguaje ubicuo y refinar los modelos.
- **No es una solución universal:** En aplicaciones simples, puede introducir complejidad innecesaria.
- **Alta dependencia de expertos en el dominio:** Si no hay comunicación fluida con expertos del negocio, el modelado puede resultar defectuoso.

---

### Adopción de los principios SOLID

SOLID es el acrónimo de los cinco principios fundamentales de diseño orientado a objetos definidos por Robert C. Martin:

#### Principio de Responsabilidad Única (*Single Responsibility Principle* - SRP)

Una clase debe tener solo una razón para cambiar.

Violación del principio en `User`:

`Solid.ts`:
```typescript
class User { 
    constructor( 
        private name: string, 
        private email: string, 
        private password: string, 
    ) {} 

    generateSlug(): string { 
        return kebabCase(this.name) 
    } 

    login(email: string, password: string) {} 
    sendEmail(email: string, template: string) {} 
}
```

Refactorización separando responsabilidades en servicios especializados:

`solid.ts`:
```typescript
class UserAccountService { 
    login(user: User, password: string) {} 
} 

class EmailService { 
    sendEmailToUser(user: User, template: string) {} 
}
```

#### Principio de Abierto/Cerrado (*Open-Closed Principle* - OCP)

Las entidades de software deben estar abiertas a la extensión, pero cerradas a la modificación.

Implementación inicial modificable:

`Solid.ts`:
```typescript
type AccountType = "Normal" | "Premium" 

class User { 
    constructor( 
        private name: string, 
        private email: string, 
        private password: string, 
        private accountType: AccountType = "Normal", 
    ) {} 

    isPremium(): boolean { 
        return this.accountType === "Premium" 
    } 
} 

class VoucherService { 
    getVoucher(user: User): string { 
        if (user.isPremium()) { 
            return "15% discount" 
        } else { 
            return "10% discount" 
        } 
    } 
}
```

Refactorización mediante mapeo extensible:

`Solid.ts`:
```typescript
type Voucher = string 

const userTypeToVoucherMap: Record<AccountType, Voucher> = { 
    Normal: "10% discount", 
    Premium: "15% discount", 
    Ultimate: "20% discount", 
} 

class VoucherService { 
    getVoucher(user: User): string { 
        return userTypeToVoucherMap[user.getAccountType()]; 
    } 
}
```

#### Principio de Sustitución de Liskov (*Liskov Substitution Principle* - LSP)

Los objetos de un programa deben poder ser reemplazados por instancias de sus subtipos sin alterar el funcionamiento correcto del programa.

`Solid.ts`:
```typescript
interface Bag<T> { 
    push(item: T): void 
    pop(): T | undefined 
    isEmpty(): boolean 
} 

class Stack<T> implements Bag<T> { 
    constructor(private items = []) {} 

    push<T>(item: T) {} 

    pop(): T | undefined { 
        if (this.items.length > 0) { 
            return this.items.pop() 
        } 
        return undefined 
    } 

    isEmpty(): boolean { 
        return this.items.length === 0 
    } 
}
```

Violación de LSP con efectos secundarios inesperados:

`Solid.ts`:
```typescript
class NonEmptyStack<T> implements Bag<T> { 
    private tag: any = Symbol() 

    constructor(private items: T[] = []) { 
        if (this.items.length == 0) { 
            this.items.push(this.tag) 
        } 
    } 

    push(item: T) { 
        this.items.push(item) 
    } 

    pop(): T | undefined { 
        if (this.items.length === 1) { 
            const item = this.items.pop() 
            this.items.push(this.tag) 
            return item 
        } 
        if (this.items.length > 1) { 
            return this.items.pop() 
        } 
        return undefined 
    } 

    isEmpty(): boolean { 
        return this.items.length === 0 
    } 
}
```

#### Principio de Segregación de Interfaces (*Interface Segregation Principle* - ISP)

Los clientes no deben verse obligados a depender de interfaces con métodos que no utilizan. Es preferible tener interfaces pequeñas y específicas.

Interfaz demasiado amplia y acoplada:

`Solid.ts`:
```typescript
interface Collection<T> { 
    pushBack(item: T): void 
    popBack(): T 
    pushFront(item: T): void 
    popFront(): T 
    isEmpty(): boolean 
    insertAt(item: T, index: number): void 
    deleteAt(index: number): T | undefined 
}
```

Segregación en interfaces delgadas y modulares:

`Solid.ts`:
```typescript
interface Collection<T> { 
    isEmpty(): boolean 
} 

interface Array<T> extends Collection<T> { 
    insertAt(item: T, index: number): void 
    deleteAt(index: number): T | undefined 
} 

interface Stack<T> extends Collection<T> { 
    pushFront(item: T): void 
    popFront(): T 
} 

interface Queue<T> extends Collection<T> { 
    pushBack(item: T): void 
    popFront(): T 
}
```

#### Principio de Inversión de Dependencias (*Dependency Inversion Principle* - DIP)

Los módulos de alto nivel no deben depender de módulos de bajo nivel; ambos deben depender de abstracciones. Las abstracciones no deben depender de los detalles; los detalles deben depender de las abstracciones.

Violación con acoplamiento directo:

`Solid.ts`:
```typescript
type User = { 
    name: string 
    email: string 
} 

class UserService { 
    constructor() {} 

    findByEmail(email: string): User | undefined { 
        const userRepo = UserRepositoryFactory.getInstance() 
        return userRepo.findByEmail(email) 
    } 
} 

class UserRepositoryFactory { 
    static getInstance(): UserRepository { 
        return new UserRepository() 
    } 
} 

class UserRepository { 
    users: User[] = [{ name: "Theo", email: "theo@example.com" }] 

    findByEmail(email: string): User | undefined { 
        const user = this.users.find((u) => u.email === email) 
        return user 
    } 
}
```

Inversión de dependencias mediante inyección de interfaz y mocks de prueba:

`Solid.ts`:
```typescript
class UserService { 
    constructor(private userQuery: UserQuery = UserRepositoryFactory.getInstance()) {} 

    findByEmail(email: string): User | undefined { 
        return this.userQuery.findByEmail(email) 
    } 
} 

class UserRepository implements UserQuery { 
    users: User[] = [{ name: "Theo", email: "theo@example.com" }] 

    findByEmail(email: string): User | undefined { 
        return this.users.find((u) => u.email === email); 
    } 
} 

class MockUserQuery implements UserQuery { 
    private users: User[] = [ 
        { name: "Alice", email: "alice@example.com" }, 
        { name: "Bob", email: "bob@example.com" }, 
    ]; 

    findByEmail(email: string): User | undefined { 
        return this.users.find((u) => u.email === email); 
    } 
} 

// Unit test for UserService 
describe("UserService", () => { 
    let userService: UserService; 
    let mockUserQuery: MockUserQuery; 

    beforeEach(() => { 
        mockUserQuery = new MockUserQuery(); 
        userService = new UserService(mockUserQuery); // Injecting the mock 
    }); 
    // Test cases 
});
```

#### ¿Es el uso de SOLID siempre la mejor práctica?

Los principios SOLID son guías excelentes, pero deben equilibrarse con principios complementarios como **DRY** (*Don't Repeat Yourself*) y **KISS** (*Keep It Simple, Stupid*). La sobreingeniería por adherirse ciegamente a SOLID puede generar una proliferación excesiva de clases y niveles de indirección innecesarios para casos simples.

---

### Aplicación de la arquitectura MVC

La arquitectura Modelo-Vista-Controlador (MVC) organiza los módulos de software en tres componentes interconectados:

#### Modelo (*Model*)

Encapsula los datos de la entidad y la lógica de negocio.

`Mvc.ts`:
```typescript
interface TodoModel { 
    id: number 
    title: string 
    completed: boolean 
    toggleCompletion(): void 
} 

class Todo implements TodoModel { 
    constructor( 
        public readonly id: number, 
        public title: string, 
        public completed: boolean = false, 
    ) {} 

    toggleCompletion(): void { 
        this.completed = !this.completed 
    } 
}
```

#### Vista (*View*)

Define la capa de presentación y captura la interacción del usuario.

`Mvc.ts`:
```typescript
class TodoList { 
    private todos: TodoModel[] = [] 
    private nextId: number = 1 

    addTodo(title: string): void { 
        const newTodo = new Todo(this.nextId++, title) 
        this.todos.push(newTodo) 
    } 

    getTodos(): TodoModel[] { 
        return this.todos 
    } 
} 

class TodoView { 
    constructor(private model: TodoList) {} 

    displayTodos() { 
        console.log("Todo List:") 
        this.model.getTodos().forEach((todo, index) => { 
            console.log(`${index + 1}. ${todo}`) 
        }) 
    } 

    promptAddTodo() { 
        const readline = require("readline").createInterface({ 
            input: process.stdin, 
            output: process.stdout, 
        }) 
        readline.question("Enter a new todo: ", (todo: string) => { 
            console.log("Todo added successfully!") 
            readline.close() 
        }) 
    } 
}
```

#### Controlador (*Controller*)

Coordina la comunicación entre el modelo y la vista:

`Mvc.ts`:
```typescript
class TodoController { 
    constructor(private model: TodoList, private view: TodoView) {} 

    addTodo(title: string): void { 
        this.model.addTodo(title); 
        console.log("Todo added successfully!"); 
        this.view.displayTodos(); 
    } 

    promptAddTodo(): void { 
        this.view.promptAddTodo(); 
    } 
} 

const todoList = new TodoList(); 
const todoView = new TodoView(todoList); 
const todoController = new TodoController(todoList, todoView); 

todoController.promptAddTodo();
```

---

### Resumen

En este capítulo analizamos recomendaciones clave y mejores prácticas para el desarrollo en TypeScript a escala:
- **Combinación estratégica de patrones de diseño:** Singleton con Builder, Façade o State; e Iterator con Composite o Visitor.
- **Tipos de utilidad y tipos mapeados:** Construcción de transformaciones avanzadas como `Mutable<T>`, `OptionalKeys<T>` y `FunctionProperties<T>`.
- **Diseño Guiado por el Dominio (DDD):** Organización modular mediante contextos delimitados, entidades, objetos de valor y eventos de dominio.
- **Principios SOLID:** Creación de arquitecturas desacopladas, mantenibles y fáciles de probar.
- **Arquitectura MVC:** Separación limpia de datos (Model), interfaz (View) y orquestación (Controller).

En el siguiente capítulo, exploraremos los antipatrones y trampas más comunes al desarrollar en TypeScript y cómo evitarlos.

---

### Preguntas y respuestas

1. **¿Cuáles son los beneficios de combinar patrones de diseño?**
   - **Respuesta:** Al combinar patrones de diseño, se aprovechan las mejores cualidades de cada uno (por ejemplo, combinar Singleton con Façade para asegurar una única puerta de enlace a un subsistema complejo, o Iterator con Composite para recorrer jerarquías uniformemente).

2. **¿Cuál es la diferencia entre los tipos de utilidad `Omit` y `Pick`?**
   - **Respuesta:** `Pick<T, K>` construye un tipo seleccionando únicamente las propiedades especificadas `K` del tipo `T`. Por el contrario, `Omit<T, K>` construye un tipo seleccionando todas las propiedades de `T` y eliminando las claves especificadas `K`.

3. **¿En qué se diferencia DRY de SOLID?**
   - **Respuesta:** DRY (*Don't Repeat Yourself*) se enfoca en evitar la duplicación de código extrayendo lógica repetida en funciones o clases reutilizables. SOLID se enfoca en la mantenibilidad, el bajo acoplamiento y la flexibilidad de extensión. En ocasiones, la aplicación estricta de SOLID puede requerir cierta duplicación o más clases, por lo que debe buscarse un equilibrio pragmático entre ambos.

---

### Lecturas adicionales

- *Mastering TypeScript, Fourth Edition*, de Nathan Rozentals: https://www.packtpub.com/product/mastering-typescript-fourth-edition/9781800564732
- Introducción al patrón Repository: https://www.umlboard.com/design-patterns/repository.html
- *Full-Stack React, TypeScript, and Node*, de David Choi: https://www.packtpub.com/product/full-stack-react-typescript-and-node/9781839219931
