# Parte 2: Patrones de diseño fundamentales en TypeScript

## Capítulo 6: Patrones de diseño de comportamiento para la gestión del estado y el comportamiento

En el capítulo anterior, revisamos los patrones de diseño de comportamiento que facilitan la comunicación y las interacciones entre objetos.

En este capítulo, nos centraremos en los patrones de diseño de comportamiento que ayudan a gestionar el estado y el comportamiento de los objetos a lo largo del tiempo. Estos patrones proporcionan opciones para controlar cómo cambian, se comportan o se ejecutan los objetos en función de diferentes estados o procesos, desacoplando el estado interno de un objeto de su comportamiento. Introducen formas de hacer que las interacciones entre objetos sean más flexibles mediante la definición de estructuras de control para el comportamiento de los objetos.

Examinaremos los siguientes patrones de diseño de comportamiento en este capítulo:

- El patrón Iterator
- El patrón Memento
- El patrón State
- El patrón Template Method
- El patrón Visitor

Nuevamente, para cada patrón, explicaremos su concepto central y propósito, beneficios y posibles inconvenientes, junto con detalles de implementación y mejores prácticas.

Al final de este capítulo, habrás adquirido una comprensión integral de cómo gestionar transiciones de estado complejas y comportamientos de objetos. Estarás bien informado para implementar estos patrones en escenarios del mundo real donde la gestión del estado de los objetos o la organización de comportamientos complejos sea importante.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter06_Behavioral_Design_Patterns_State

---

### El patrón Iterator

El patrón Iterator es un patrón de diseño de comportamiento que proporciona una forma de iterar sobre una colección de objetos sin conocer la estructura interna de dichos objetos. Este patrón es fundamental en muchos lenguajes de programación y es un concepto central en el diseño de mecanismos eficientes y flexibles para el recorrido de colecciones.

Algunos de sus conceptos clave son los siguientes:

- **Abstracción del recorrido:** El patrón Iterator abstrae el proceso de recorrer una colección, separando el algoritmo de recorrido de la estructura de la colección. Esta separación permite diferentes métodos de recorrido sin modificar la colección.
- **Interfaz uniforme:** Proporciona una interfaz estándar para recorrer diferentes tipos de colecciones, lo que facilita la escritura de código genérico que funciona con varias estructuras de datos.
- **Encapsulación de los detalles de la colección:** El patrón encapsula la estructura interna de la colección, lo que permite al desarrollador cambiar la parte de implementación sin cambiar el código que utiliza el patrón.

Una analogía para este patrón es cuando tienes una lista guardada de programas favoritos en tu disco duro. Cada uno de estos videos se guarda en una carpeta diferente, pero puedes iterar sobre ellos uno por uno desde tu vista de interfaz de usuario sin conocer los detalles de su ubicación en el disco.

A continuación explicaremos en detalle cuándo utilizar este patrón.

#### Cuándo usar el patrón Iterator

Querrás considerar el uso de un Iterator para los siguientes casos de uso:

- **Estructuras de datos complejas:** Al tratar con estructuras de datos complejas como árboles, grafos o colecciones personalizadas donde la lógica de recorrido puede ser complicada.
- **Múltiples algoritmos de recorrido:** Cuando necesitas proporcionar múltiples formas de recorrer una colección (por ejemplo, en orden, preorden o postorden para un árbol).
- **Desacoplamiento de clientes y colecciones:** Para permitir que el código del cliente acceda a los elementos de la colección sin conocer su estructura interna.
- **Iteración paralela:** Cuando necesitas mantener múltiples posiciones de recorrido en la misma colección simultáneamente.
- **Evaluación perezosa (*Lazy evaluation*):** Para implementar la carga diferida o la generación de elementos bajo demanda, donde los elementos se calculan solo cuando se solicitan.
- **Provisión de una interfaz uniforme:** Cuando deseas proporcionar una forma estándar de recorrer diferentes tipos de colecciones en tu sistema.

En estos casos, puedes utilizar un objeto Iterator que encapsulará la operación de recorrido de la estructura de datos subyacente o un objeto agregado. El principal beneficio para los clientes que utilizarán este patrón es que podrán usarlo en bucles sin saber cómo están estructurados los objetos entre bastidores.

#### Diagrama de clases UML

La estructura del patrón Iterator se puede representar claramente mediante un diagrama de clases UML (*Figura 6.1: El patrón Iterator*):

- **Iterator:** Esta interfaz declara los métodos para acceder y recorrer elementos. Por lo general, incluye dos métodos principales: `hasNext()` para verificar si hay más elementos sobre los que iterar, y `next()` para recuperar el siguiente elemento en la secuencia.
- **Aggregate:** Esta interfaz declara un método para crear un objeto Iterator. Es implementada por colecciones concretas que desean ser iterables.
- **ConcreteIterator:** Esta clase implementa la interfaz `Iterator`. Almacena la posición actual de la iteración e implementa la lógica para moverse a través de la colección. La clase `ConcreteIterator` tiene una referencia a la colección sobre la que está iterando.
- **ConcreteAggregate:** Esta clase implementa la interfaz `Aggregate`. Representa la colección que se está recorriendo e implementa el método para crear una clase `ConcreteIterator` para sí misma. También suele incluir métodos para gestionar la colección, como agregar o eliminar elementos.
- **Client:** Representa el código que utiliza la interfaz `Iterator`. La clase `Client` trabaja tanto con la interfaz `Aggregate` para obtener un iterador como con la interfaz `Iterator` para recorrer los elementos.

Esta estructura permite una gran flexibilidad. Se pueden agregar nuevos tipos de colecciones implementando la interfaz `Aggregate`, y se pueden agregar nuevas formas de recorrido implementando nuevas clases `Iterator`. El código cliente permanece sin cambios, trabajando solo con las interfaces abstractas.

#### Implementación clásica

Comencemos a implementar el patrón Iterator basándonos en el diagrama de clases anterior. Usaremos una lista de reproducción de música como ejemplo:

`iterator.ts`:
```typescript
class Song { 
    constructor(public title: string, public artist: string, public duration: number) {} 
    toString(): string { 
        return `${this.title} by ${this.artist} (${this.duration} seconds)`; 
    } 
} 

interface Iterator<T> { 
    hasNext(): boolean; 
    next(): T | null; 
} 

interface Playlist { 
    createIterator(): Iterator<Song>; 
}
```

Aquí, la clase `Song` representa una sola canción con propiedades como título, artista y duración. La interfaz `Iterator` define el contrato para los iteradores con dos métodos: `hasNext()` para comprobar si hay más elementos, y `next()` para recuperar el siguiente elemento. Luego, la interfaz `Playlist` es la interfaz `Aggregate`, que declara el método `createIterator()` que devuelve una interfaz `Iterator<Song>`.

A continuación, implementamos el resto de las clases:

`iterator.ts`:
```typescript
class PlaylistIterator implements Iterator<Song> { 
    private currentIndex: number = 0; 
    constructor(private playlist: Song[]) {} 

    hasNext(): boolean { 
        return this.currentIndex < this.playlist.length; 
    } 

    next(): Song | null { 
        if (this.hasNext()) { 
            if (!this.playlist[this.currentIndex]) { 
                // Lazy load more songs from an external source 
                this.loadMoreSongs(); 
            } 
            return this.playlist[this.currentIndex++] || null; 
        } 
        return null; 
    } 

    // Load more songs from an external source 
    private loadMoreSongs(): void {//} 
} 

class MusicPlaylist implements Playlist { 
    private songs: Song[] = []; 

    addSong(song: Song): void { 
        this.songs.push(song); 
    } 

    createIterator(): Iterator<Song> { 
        return new PlaylistIterator(this.songs); 
    } 
} 

const myPlaylist = new MusicPlaylist(); 
myPlaylist.addSong(new Song("Bohemian Rhapsody", "Queen", 354)); 
myPlaylist.addSong(new Song("Stairway to Heaven", "Led Zeppelin", 482)); 
myPlaylist.addSong(new Song("Imagine", "John Lennon", 183)); 

const iterator = myPlaylist.createIterator(); 
console.log("My Playlist:"); 
while (iterator.hasNext()) { 
    const song = iterator.next(); 
    if (song) { 
        console.log(song.toString()); 
    } 
}
```

Aquí, la clase `PlaylistIterator` actúa como el iterador concreto, responsable de rastrear la posición actual en la lista de reproducción. Admite carga diferida (*lazy loading*), lo que significa que puede obtener canciones adicionales de una fuente externa cuando sea necesario, en lugar de cargarlas todas de antemano. La clase `MusicPlaylist` actúa como nuestra clase `ConcreteAggregate`, administrando una colección de canciones. El código cliente al final demuestra cómo usar el patrón Iterator.

Como ejemplo más práctico, considera el uso de `async/await` en la clase `AsyncSongIterator`. Esta implementación permite la iteración asíncrona sobre una colección de canciones que se pueden obtener de una fuente externa, como una API:

`iterator.ts`:
```typescript
class AsyncSongIterator { 
    private currentIndex: number = 0 
    private songs: string[] = [] 

    constructor() {} 

    // Simulate fetching songs from an API 
    private async fetchSongs(): Promise<string[]> { 
        await new Promise((resolve) => setTimeout(resolve, 1000)) 
        return ["Song 1", "Song 2", "Song 3"] 
    } 

    async next(): Promise<{ value: string | null; done: boolean }> { 
        if (this.currentIndex === 0) { 
            this.songs = await this.fetchSongs() 
        } 
        if (this.currentIndex < this.songs.length) { 
            return { value: this.songs[this.currentIndex++], done: false } 
        } else { 
            return { value: null, done: true } 
        } 
    } 

    [Symbol.asyncIterator]() { 
        return this 
    } 
} 

const songIterator = new AsyncSongIterator() 
for await (const song of songIterator) { 
    console.log(song) 
}
```

La clase `AsyncSongIterator` implementa un iterador asíncrono que obtiene una lista de canciones de una fuente externa, simulando una llamada a la API. La clase implementa el método `[Symbol.asyncIterator]()`, lo que le permite ser utilizada en un bucle `for await...of`.

#### Pruebas (*Testing*)

Al probar el patrón Iterator, debes verificar que la implementación del iterador sea sólida:
- Comprueba que al llamar a `next()` y `hasNext()` sobre la colección, los métodos recuperen el siguiente elemento y devuelvan `true` o `false` respectivamente según la existencia de elementos adicionales.
- En `Aggregate`, verifica que la llamada al método `createIterator()` devuelva la instancia correcta de `ConcreteIterator`.

Para ejecutar los casos de prueba provistos en `iterator.test.ts`:

```bash
$ npm run test -- iterator
```

#### Críticas

- **Complejidad para colecciones simples:** Puede introducir una complejidad innecesaria para listas o arrays básicos, donde las abstracciones adicionales del patrón Iterator podrían ser excesivas.
- **Sobrecarga de mantenimiento:** Introduce más clases e interfaces para mantener.
- **Excesivo para colecciones de solo lectura:** Para colecciones que solo se leen y nunca se modifican, un método simple que devuelva un array o lista puede ser suficiente y más intuitivo.

#### Caso de uso del mundo real

El patrón Iterator encuentra una aplicación significativa en bibliotecas de programación reactiva como RxJS, que aprovecha este patrón para manejar flujos de datos asíncronos de manera eficiente:

```typescript
const observable = new Observable<number>(observer => { 
    observer.next(1); 
    observer.next(2); 
    observer.next(3); 
    observer.complete(); 
}); 

observable 
    .map(x => x * 10) 
    .filter(x => x > 10) 
    .subscribe(value => console.log(value));
```

La clase `Observable` utiliza iteradores de ES6, lo que permite operaciones componibles en los flujos de datos.

A continuación, examinaremos el patrón Memento.

---

### El patrón Memento

El patrón Memento (Recuerdo) es una poderosa solución de diseño para gestionar el estado de un objeto a lo largo de una aplicación sin comprometer la encapsulación. Este patrón utiliza un mecanismo para almacenar un estado interno, creando efectivamente una instantánea (*snapshot*) en un momento particular en el tiempo. Luego expone operaciones que manipulan este estado para realizar ciertas tareas de manera segura.

En su núcleo, el patrón Memento consta de tres componentes clave:

- **Originator (Originador):** Este es el objeto cuyo estado debe guardarse y restaurarse. Contiene el estado que deseamos gestionar.
- **Memento (Recuerdo):** Es un objeto simple que almacena el estado real del Originator. Proporciona una interfaz directa para almacenar y recuperar datos, actuando como una instantánea del estado del Originator en un momento específico.
- **Caretaker (Conserje / Cuidador):** Responsable de realizar el seguimiento de múltiples objetos Memento, mantiene un historial de estados pero nunca modifica el contenido de un Memento.

La belleza de este patrón radica en su capacidad para externalizar el estado interno de un objeto sin violar la encapsulación. Las clases Originator y Caretaker interactúan con el Memento para guardar y restaurar estados, pero ninguna tiene acceso directo al funcionamiento interno de la otra.

#### Cuándo usar el patrón Memento

- **Preservación y restauración del estado del objeto:** Cuando necesitas crear instantáneas del estado de un objeto (incluidas las propiedades privadas), almacenar estas instantáneas en un formato reproducible y restaurar el objeto a un estado anterior a petición.
- **Mantenimiento de la encapsulación:** Permite almacenar o recuperar el objeto de estado a través de una abstracción que está oculta para los clientes y proporciona una interfaz limpia para las operaciones de gestión de estado.
- **Simplificación de la gestión de estados complejos:** Especialmente útil al tratar con objetos que tienen estados internos complejos, implementar funciones de deshacer/rehacer (*undo/redo*) multinivel y gestionar puntos de control (*checkpoints*) en procesos o simulaciones de larga duración.

#### Diagrama de clases UML

En la *Figura 6.2: El patrón Memento*, tenemos cuatro elementos principales: la interfaz `AppState`, y las clases `Originator`, `Memento` y `Caretaker`:

- La clase `Originator` crea objetos `Memento` y la clase `Caretaker` almacena múltiples objetos `Memento`.
- Tanto `Originator` como `Memento` tienen una relación con la interfaz `AppState`, que representa el objeto de estado que se desea guardar o restaurar.
- `Originator` mantiene este estado y lo actualiza cuando es necesario; luego llama al método `save()`, que devolverá un objeto `Memento`.
- La clase `Caretaker` agrega todos esos objetos memento. Cuando el usuario desea restaurar un memento anterior, utiliza el método `restore()`.

#### Implementación clásica

Veamos un ejemplo que implementa una función de deshacer (*undo*) en un editor de texto:

`memento.ts`:
```typescript
interface TextEditorState { 
    content: string; 
    cursorPosition: number; 
} 

class EditorMemento { 
    constructor(private readonly state: TextEditorState) {} 
    getState(): TextEditorState { 
        return this.state; 
    } 
}
```

Aquí, `TextEditorState` representa el estado del editor de texto, incluyendo el contenido y la posición del cursor. `EditorMemento` encapsula la clase `TextEditorState`, proporcionando una forma de guardar y recuperar el estado del editor.

Uso por parte del cliente:

`memento.ts`:
```typescript
const editor = new TextEditor(); 
const caretaker = new EditorCaretaker(editor); 

editor.type("Hello, "); 
caretaker.save(); 

editor.type("world!"); 
caretaker.save(); 

console.log(editor.getContent()); // Output: Hello, world! 

caretaker.undo(); 
console.log(editor.getContent()); // Output: Hello,  

caretaker.redo(); 
console.log(editor.getContent()); // Output: Hello, world!
```

El ejemplo demuestra cómo utilizar el patrón Memento para implementar la funcionalidad de deshacer y rehacer en un editor de texto.

#### Pruebas (*Testing*)

Al probar este patrón, debes garantizar la corrección del guardado y la restauración de estados, las operaciones del Originator y la funcionalidad del Caretaker, cubriendo pruebas de integración y casos límite.

Para ejecutar los casos de prueba en `memento.test.ts`:

```bash
$ npm run test -- memento
```

#### Críticas

- **Complejidad:** Introduce clases y relaciones adicionales, lo que puede resultar innecesario para necesidades de gestión de estado simples.
- **Consumo de memoria:** Almacenar múltiples estados puede generar un uso significativo de memoria, especialmente con objetos grandes o numerosos. Esto se puede mitigar implementando técnicas de compresión de estado o limitando el número de estados almacenados.
- **Impacto en el rendimiento:** Crear y restaurar Mementos con frecuencia puede afectar el rendimiento si los estados son complejos.

#### Caso de uso del mundo real

**Visual Studio Code** implementa un sofisticado sistema de deshacer/rehacer basado en los principios del patrón Memento:
- `ITextSnapshot` (Memento): Representa una instantánea inmutable del texto del editor en un punto específico en el tiempo.
- `TextModel` (Originator): Gestiona el estado actual del texto y crea instantáneas.
- `EditStack` (Caretaker): Mantiene una pila de ediciones y proporciona la funcionalidad de deshacer/rehacer.

Al almacenar operaciones de edición en lugar de instantáneas de texto completo, logra eficiencia manteniendo los beneficios centrales del patrón.

A continuación, examinaremos el patrón State.

---

### El patrón State

El patrón State (Estado) es un patrón de diseño de comportamiento que permite a un objeto alterar su comportamiento cuando cambia su estado interno. Este patrón es particularmente útil en escenarios donde el comportamiento de un objeto debe cambiar dinámicamente según su estado, sin recurrir a sentencias condicionales manuales.

Sus conceptos clave son:
- **Context:** El objeto que controla todas las instancias de estado y expone métodos para interactuar con esos estados.
- **State:** Una interfaz o clase abstracta que define los comportamientos específicos de cada estado.
- **Concrete states:** Implementaciones de la interfaz `State`, cada una de las cuales encapsula el comportamiento asociado con un estado particular.

En lugar de que el objeto gestione internamente su comportamiento dependiente del estado mediante condicionales complejos, el patrón State externaliza estos comportamientos en objetos de estado separados. El contexto delega el trabajo específico del estado a estos objetos, cambiando entre ellos a medida que cambia su estado.

#### Cuándo usar el patrón State

- **Comportamiento complejo dependiente del estado:** Cuando el comportamiento de un objeto varía significativamente según su estado y deseas evitar grandes sentencias condicionales.
- **Transiciones de estado frecuentes o complejas:** Cuando las transiciones de estado son frecuentes o complejas y deseas gestionarlas de forma más limpia.
- **Reducción de duplicación:** Cuando diferentes estados tienen un comportamiento similar con ligeras variaciones, ayuda a eliminar la duplicación de código.
- **Cambios de estado en tiempo de ejecución:** Cuando el estado de un objeto puede cambiar dinámicamente en tiempo de ejecución.
- **Mejora de la flexibilidad:** Permite cambiar partes del comportamiento agregando nuevos estados de objetos sin modificar la clase de contexto.

#### Diagrama de clases UML

En la *Figura 6.3: El patrón State*, la clase `Context` tiene una relación de composición con `State`. Esto permite que `Context` trabaje con cualquier estado concreto que implemente la interfaz `State`. La clase `Context` puede cambiar su estado actual llamando al método `changeState()`.

#### Implementación clásica

`state.ts`:
```typescript
interface State { 
    handle(): void 
} 

class Context { 
    private state: State 

    constructor(initialState: State) { 
        this.state = initialState 
    } 

    public request(): void { 
        this.state.handle() 
    } 

    public changeState(newState: State): void { 
        if (this.state instanceof ConcreteStateA && !(newState instanceof ConcreteStateB)) { 
            throw new Error("Invalid state transition"); 
        } 
        this.state = newState 
    } 
}
```

Implementación de estados concretos y código del cliente:

`state.ts`:
```typescript
class ConcreteStateA implements State { 
    public handle(): void { 
        console.log("Handling request in ConcreteStateA"); 
    } 
} 

class ConcreteStateB implements State { 
    public handle(): void { 
        console.log("Handling request in ConcreteStateB"); 
    } 
} 

const context = new Context(new ConcreteStateA()); 
context.request(); // Output: Handling request in ConcreteStateA 

context.changeState(new ConcreteStateB()); 
context.request(); // Output: Handling request in ConcreteStateB
```

`ConcreteStateA` y `ConcreteStateB` implementan la interfaz `State`, proporcionando sus propias implementaciones para el método `handle()`. `Context` cambia entre diferentes estados concretos, modificando su comportamiento en consecuencia.

#### Pruebas (*Testing*)

Al escribir pruebas para este patrón:
- Verifica que cada tipo de estado capture los parámetros de estado correctos y que los datos que contienen sean precisos.
- Comprueba que la lógica de transición de estados sea correcta al llamar al método `changeState()`.

Para ejecutar las pruebas en `state.test.ts`:

```bash
$ npm run test -- state
```

#### Críticas

- **Complejidad frente a beneficio:** Si tu sistema solo tiene unos pocos estados con variaciones mínimas, la sobrecarga de implementar el patrón completo puede superar sus beneficios frente a soluciones más simples como enumeraciones (*enums*) o sentencias `if-else`.
- **Riesgo de sobreingeniería:** Los desarrolladores pueden verse tentados a crear clases de estado separadas para variaciones menores.
- **Dificultad para definir estados significativos:** Puede resultar difícil definir estados de una manera que proporcione un valor real sin introducir repetición o código repetitivo (*boilerplate*).
- **Cobertura de casos límite:** Asegurar una cobertura completa de todas las transiciones posibles y estados de error puede volverse complejo.

#### Caso de uso del mundo real

**React Router** utiliza un patrón similar al de estados para gestionar diferentes comportamientos de enrutamiento:

```typescript
interface RouteState { 
    match(path: string): boolean; 
    render(component: React.ComponentType): React.ReactElement; 
} 

class Route { 
    private state: RouteState; 

    constructor(path: string | RegExp) { 
        // Initialize state based on path type 
    } 

    matches(path: string): boolean { 
        return this.state.match(path); 
    } 
    // Other methods... 
}
```

El comportamiento se encapsula en los objetos de estado, lo que facilita agregar nuevos tipos de coincidencia de rutas en el futuro sin modificar el código existente.

A continuación, examinaremos el patrón Template Method.

---

### El patrón Template Method

El patrón Template Method (Método Plantilla) es un patrón de diseño de comportamiento que define el esqueleto de un algoritmo en una operación de una superclase, delegando algunos pasos a las subclases. Permite a las subclases redefinir ciertos pasos de un algoritmo sin cambiar su estructura general.

Sus conceptos clave son:
- **Clase base abstracta (*Abstract base class*):** Define el método plantilla y las operaciones abstractas.
- **Método plantilla (*Template Method*):** Es el esqueleto del algoritmo, que llama tanto a operaciones concretas como abstractas.
- **Operaciones concretas (*Concrete operations*):** Implementadas directamente en la clase base abstracta.
- **Operaciones abstractas (*Abstract operations*):** Marcadores de posición para pasos que deben ser implementados obligatoriamente por las subclases.
- **Operaciones de enlace / gancho (*Hook operations*):** Pasos opcionales con implementaciones predeterminadas que las subclases pueden sobrescribir.

#### Cuándo usar el patrón Template Method

- **Variaciones algorítmicas con estructura común:** Tienes un algoritmo o proceso con una estructura fija, pero ciertos pasos deben variar (por ejemplo, un sistema de procesamiento de documentos donde PDF, Word y HTML comparten el mismo flujo pero requieren análisis o formato específico).
- **Reducción de la duplicación de código:** Cuando múltiples clases tienen implementaciones similares con solo ligeras variaciones.
- **Desarrollo de frameworks:** Estás desarrollando un framework donde ciertas partes de un algoritmo deben ser personalizables por los usuarios del framework.

#### Diagrama de clases UML

En la *Figura 6.4: El patrón Template Method*:
- `DocumentProcessor` es la clase abstracta que define `processDocument()` como método plantilla. `openDocument()` y `saveDocument()` son métodos concretos, mientras que `extractContent()` y `analyzeContent()` son métodos abstractos.
- `PDFProcessor` y `WordProcessor` heredan de `DocumentProcessor` y proporcionan implementaciones específicas para cada tipo de documento.

#### Implementación clásica

`template-method.ts`:
```typescript
abstract class DocumentProcessor { 
    public processDocument(): void { 
        this.openDocument() 
        this.extractContent() 
        this.analyzeContent() 
        this.saveDocument() 
    } 

    protected openDocument(): void { 
        console.log("Opening document") 
    } 

    protected abstract extractContent(): void 
    protected abstract analyzeContent(): void 

    protected saveDocument(): void { 
        console.log("Saving processed document") 
    } 
}
```

Implementación de subclases concretas:

`template-method.ts`:
```typescript
class PDFProcessor extends DocumentProcessor { 
    protected extractContent(): void { 
        console.log("Extracting content from PDF"); 
    } 

    protected analyzeContent(): void { 
        console.log("Analyzing PDF content"); 
    } 
}
```

Uso en tiempo de ejecución:

```typescript
const pdfDoc: DocumentProcessor = new PDFProcessor(); 
const wordDoc: DocumentProcessor = new WordProcessor(); 
pdfDoc.processDocument(); 
wordDoc.processDocument();
```

El cliente utiliza las instancias de `DocumentProcessor` y el polimorfismo despacha los métodos de plantilla adecuados en tiempo de ejecución.

#### Pruebas (*Testing*)

Al probar el patrón Template Method, nos centramos en verificar que las subclases entreguen los resultados esperados y que los pasos del algoritmo se ejecuten en el orden correcto.

Para ejecutar los casos de prueba en `template-method.test.ts`:

```bash
$ npm run test -- template-method
```

#### Críticas

- **Dificultad en la desaprobación (*Deprecation*):** Si los pasos de la plantilla deben quedar obsoletos después de que el código se haya distribuido a los clientes, puede ser difícil eliminar o alterar estos pasos sin romper el código del cliente que depende de ellos.
- **Flexibilidad limitada:** Para mantener la flexibilidad, es aconsejable definir menos pasos obligatorios y más ganchos (*hooks*) opcionales.

#### Casos de uso del mundo real

- **Componentes de clase de React:** El método `render()` es obligatorio, mientras que los métodos de ciclo de vida como `componentDidMount()`, `componentWillUnmount()` y `shouldComponentUpdate()` son ganchos opcionales:
  ```typescript
  class WelcomeHome extends React.Component<{ name: string},{}> { 
      componentDidMount() { 
          console.log("Just loaded"); 
      } 
      componentWillUnmount() { 
          console.log("Goodbye!"); 
      } 
      shouldComponentUpdate() { 
          return false; 
      } 
      render() { 
          return <h1>Hello, {this.props.name}</h1>; 
      } 
  }
  ```
- **Ganchos de ciclo de vida de Angular:** Métodos como `ngOnInit()` en componentes de Angular:
  ```typescript
  import { Component, OnInit } from '@angular/core'; 

  @Component({ 
      selector: 'app-user-profile', 
      template: `<h1>User Profile</h1><div>{{ userData }}</div>`, 
  }) 
  export class UserProfileComponent implements OnInit { 
      userData: string; 

      // Template Method 
      ngOnInit(): void { 
          this.initialize(); 
          this.loadData(); 
      } 

      private initialize(): void { 
          console.log('Initializing User Profile'); 
      } 

      private loadData(): void { 
          // Simulate data loading 
          this.userData = 'User data loaded from API'; 
      } 
  }
  ```

A continuación, examinaremos el último patrón de este capítulo: el patrón Visitor.

---

### El patrón Visitor

El patrón Visitor (Visitante) es un patrón de diseño de comportamiento que te permite separar algoritmos de los objetos sobre los que operan. Permite agregar operaciones adicionales a una colección de objetos sin modificar sus clases originales.

En la práctica, esto implica agregar un método a tus componentes que acepte una referencia a un objeto Visitor y pase su propia instancia (`this`) como parámetro a este visitante (*Double Dispatch*). El objeto Visitor obtiene acceso a los métodos públicos de cada objeto visitado, lo que le permite acumular o realizar operaciones sobre su estado.

#### ¿Cuándo usar el patrón Visitor?

- **Abstracción de funcionalidad para recolectar estado público:** Cuando tienes una jerarquía de objetos compuestos y necesitas recorrerlos para recopilar ciertos parámetros o variables de estado.
- **Aplicación de nuevas operaciones a grupos de objetos con interfaces comunes:** Cuando tienes un grupo de objetos que implementan una interfaz común pero tienen métodos específicos en sus subclases concretas que deseas utilizar sin alterar los objetos mismos.

#### Diagrama de clases UML

- En la *Figura 6.5: El objeto Visitor*, la interfaz `DocumentVisitor` declara una operación de visita para cada clase de elemento concreto (`visitPdfDocument`, `visitWordDocument`, `visitCompositeDocument`), y `ConcreteDocumentVisitor` la implementa.
- En la *Figura 6.6: El patrón Visitor*, la interfaz `AcceptsVisitor` declara la operación `accept(visitor: DocumentVisitor)`. Los elementos concretos (`PdfDocument`, `WordDocument`, `CompositeDocument`) implementan `AcceptsVisitor` y pasan su propia instancia al método de visita correspondiente del visitante.

#### Implementación clásica

`visitor.ts`:
```typescript
export interface DocumentVisitor { 
    visitPdfDocument(pdfDocument: PdfDocument): void 
    visitWordDocument(wordDocument: WordDocument): void 
    visitCompositeDocument(compositeDocument: CompositeDocument): void 
} 

export interface AcceptsVisitor { 
    accept(visitor: DocumentVisitor): void 
}
```

Implementación de clases de documentos concretas:

`visitor.ts`:
```typescript
export class PdfDocument implements AcceptsVisitor { 
    accept(visitor: DocumentVisitor): void { 
        visitor.visitPdfDocument(this); 
    } 
    // PDF-specific methods... 
} 

export class WordDocument implements AcceptsVisitor { 
    accept(visitor: DocumentVisitor): void { 
        visitor.visitWordDocument(this); 
    } 
    // Word-specific methods... 
}
```

Implementación de la estructura compuesta y uso por parte del cliente:

`visitor.ts`:
```typescript
export class CompositeDocument implements AcceptsVisitor { 
    private documents: AcceptsVisitor[] = [] 

    addDocument(document: AcceptsVisitor): void { 
        this.documents.push(document) 
    } 

    accept(visitor: DocumentVisitor): void { 
        for (let document of this.documents) { 
            document.accept(visitor) 
        } 
        visitor.visitCompositeDocument(this) 
    } 
}
```

Uso del cliente:

```typescript
const composite = new CompositeDocument(); 
const visitor = new DocumentProcessingVisitor(); 
composite.addDocument(new PdfDocument()); 
composite.addDocument(new WordDocument()); 
composite.accept(visitor);
```

#### Pruebas (*Testing*)

Al probar el patrón Visitor, verifica que:
- El componente compuesto reenvíe el visitante a cada uno de sus elementos hijos.
- Los componentes individuales llamen al método de visitante correcto.
- Los métodos concretos del visitante se ejecuten según lo esperado para cada tipo de componente.

Para ejecutar las pruebas en `visitor.test.ts`:

```bash
$ npm run test -- visitor
```

#### Críticas

- **Acoplamiento más estrecho:** La interfaz `Visitor` incluye métodos para cada tipo de elemento concreto, creando un acoplamiento estrecho. Agregar nuevos tipos de elementos requiere modificar todas las implementaciones de visitantes existentes.
- **Riesgo de errores en la selección de métodos:** Los desarrolladores deben asegurarse de llamar al método de visita correcto para cada tipo de componente.
- **Acceso limitado al estado interno:** Los visitantes normalmente solo tienen acceso a los métodos y propiedades públicos de los elementos que visitan.

#### Casos de uso del mundo real

- **Herramientas de análisis de código estático (AST):**
  ```typescript
  interface ASTVisitor { 
      visitFunctionDeclaration(node: FunctionDeclaration): void; 
      visitVariableDeclaration(node: VariableDeclaration): void; 
      // ... other visit methods 
  } 

  class ComplexityAnalyzer implements ASTVisitor { 
      private complexity = 0; 

      visitFunctionDeclaration(node: FunctionDeclaration): void { 
          this.complexity += 1; 
          // Analyze function body 
      } 
      // ... other visit methods 
  }
  ```
- **Compilador de TypeScript:** Utiliza ampliamente el patrón Visitor para transformar y analizar el Árbol de Sintaxis Abstracta (AST) en `src/compiler`.
- **`@typescript-eslint`:** Utiliza `VisitorBase.ts` para implementar reglas de linting sobre el código TypeScript.

---

### Resumen

Este capítulo examinó cinco patrones de diseño de comportamiento para gestionar el estado y el comportamiento de los objetos a lo largo del tiempo:

- El patrón **Iterator** permite el recorrido de colecciones sin exponer su implementación subyacente.
- El patrón **Memento** permite a los objetos guardar y restaurar su estado interno, controlando la reversión de operaciones mientras se mantiene la encapsulación.
- El patrón **State** altera dinámicamente el comportamiento de un objeto en función de su estado actual, gestionando las transiciones de estado.
- El patrón **Template Method** define algoritmos de alto nivel al tiempo que permite a las subclases implementar pasos específicos.
- El patrón **Visitor** agrega nuevas operaciones a jerarquías de clases sin modificar las clases originales.

Estos patrones proporcionan herramientas poderosas para gestionar transiciones de estado complejas y organizar el comportamiento de los objetos, lo que permite sistemas más dinámicos y adaptables.

En el próximo capítulo, profundizaremos en los principios de la programación funcional, explorando cómo este paradigma enfatiza la inmutabilidad, las funciones de primera clase y las funciones de orden superior para crear un código más predecible y mantenible.

---

### Preguntas y respuestas

1. **¿En qué se diferencia el patrón Visitor del patrón Composite?**
   - **Respuesta:** Los patrones Visitor y Composite a menudo trabajan juntos pero abordan preocupaciones diferentes. El patrón Visitor es de comportamiento y busca separar los algoritmos de las estructuras de objetos sobre las que operan, permitiendo agregar nuevas operaciones sin modificar dichas estructuras. En contraste, el patrón Composite es estructural y se centra en tratar objetos individuales y composiciones de objetos de manera uniforme, permitiendo a los clientes interactuar con objetos individuales y compuestos de la misma manera.
