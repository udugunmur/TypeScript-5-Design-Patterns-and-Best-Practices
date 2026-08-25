# Parte 3: Conceptos avanzados de TypeScript y mejores prácticas

## Capítulo 8: Programación reactiva y asíncrona

La programación reactiva es un paradigma poderoso que se centra en el flujo de datos a través de un sistema y en cómo responde dicho sistema a los cambios. Al adoptar este enfoque, puedes simplificar la comunicación entre componentes y mejorar el rendimiento general.

Este paradigma es particularmente útil en varios escenarios, como la creación de interfaces de usuario (UI) interactivas, canales de datos en tiempo real y herramientas de comunicación. En esencia, la programación reactiva enfatiza la comunicación asíncrona entre servicios, lo que permite a los sistemas gestionar eficientemente cómo y cuándo responden a los cambios en los datos.

Cuando se combina con los principios de la programación funcional, la programación reactiva permite la creación de operadores componibles que facilitan el desarrollo de sistemas escalables y fáciles de mantener. En este capítulo, profundizaremos en los conceptos y técnicas fundamentales de la programación reactiva, dotándote del conocimiento necesario para construir aplicaciones responsivas.

Cubriremos los siguientes temas en este capítulo:

- Aprendizaje de los conceptos de programación reactiva
- Propagación asíncrona de cambios
- Comprensión de Promises y Futures
- Exploración de Observables

Al final de este capítulo, habrás acumulado las habilidades y técnicas necesarias para escribir software altamente escalable y desacoplado utilizando conceptos útiles de programación reactiva.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter08_Reactive_Programming_concepts

---

### Aprendizaje de los conceptos de programación reactiva

Cuando nos referimos a "reactivo" en la programación de computadoras, normalmente discutimos tres conceptos clave:

1. **Programación reactiva (*Reactive programming*):** Es un paradigma de computación que enfatiza la propagación asíncrona del flujo de información. En este modelo, cuando un objeto de servicio consulta datos a otro, la respuesta no ocurre de manera simultánea. En cambio, la respuesta se puede aceptar y evaluar más tarde. Una vez que los datos están listos, se pueden propagar a los consumidores a través de varios mecanismos predefinidos, como callbacks, Promises o Futures. Este enfoque permite que los sistemas sigan siendo receptivos y eficientes, incluso cuando manejan operaciones asíncronas.
2. **Sistemas reactivos (*Reactive systems*):** Un sistema reactivo es un conjunto de principios de diseño y conceptos destinados a construir aplicaciones escalables y distribuidas que mantienen una comunicación asíncrona. Estos principios se derivan del Manifiesto Reactivo (*Reactive Manifesto*). Los sistemas reactivos están diseñados para ser responsivos, resilientes, elásticos y guiados por mensajes, lo que les permite manejar cargas variables y fallos con gracia.
3. **Programación reactiva funcional (*Functional Reactive Programming* o FRP):** La FRP combina los conceptos de la programación reactiva con la programación funcional. Este paradigma utiliza la composición funcional para manipular flujos de datos mientras garantiza la transparencia referencial. En este capítulo, demostraremos el uso de FRP con Observables, mostrando cómo se pueden aplicar estos conceptos en la práctica.

#### Conceptos del Manifiesto Reactivo en breve

El Manifiesto Reactivo es un documento colectivo que describe los principios clave de los sistemas reactivos, divididos en cuatro conceptos fundamentales:
- **Responsivos (*Responsive*):** Responden de manera oportuna y predecible.
- **Resilientes (*Resilient*):** Permanecen responsivos frente a fallos y errores en sistemas de misión crítica.
- **Elásticos (*Elastic*):** Pueden ajustar su escalabilidad en función de las cargas de trabajo disponibles.
- **Guiados por mensajes (*Message-driven*):** Tienen métodos de comunicación desacoplados que les permiten mantener un bajo acoplamiento y alta eficiencia.

#### Beneficios de la programación reactiva

- **Eficiencia:** Al permitir la comunicación asíncrona y las operaciones no bloqueantes, optimiza la utilización de recursos y mejora el rendimiento general del sistema.
- **Operaciones desacopladas:** Los patrones de comunicación promueven un bajo acoplamiento entre componentes, haciendo que los sistemas sean más fáciles de mantener y extender.
- **Desarrollo simplificado:** Los desarrolladores pueden aprovechar clientes estandarizados y fáciles de usar para realizar tareas asíncronas o componerlas abstractamente sin necesidad de gestionar manualmente las complejidades de la propagación de datos.

> [!WARNING]
> El inconveniente de este estilo es que introduce una mayor complejidad tanto en la estructura del código como en la arquitectura del sistema. Si no se mantiene la naturaleza asíncrona y no bloqueante, se puede provocar una degradación del rendimiento o pérdida de datos.

#### ¿Cuándo usar la programación reactiva?

- **Flujos de datos asíncronos:** Si tu aplicación maneja múltiples flujos de datos asíncronos, como entradas de usuario, solicitudes de red o actualizaciones en tiempo real.
- **Arquitecturas dirigidas por eventos (*Event-Driven Architectures* o EDA):** Para sistemas que dependen de EDAs, como microservicios o aplicaciones sin servidor (*serverless*).
- **Aplicaciones en tiempo real:** Aplicaciones que requieren baja latencia o sistemas de Internet de las Cosas (IoT), donde manejar actualizaciones de datos de alta frecuencia y propagar cambios eficientemente es esencial.

---

### Propagación asíncrona de cambios

En términos prácticos, la programación reactiva representa un paradigma donde usamos código declarativo para describir comunicaciones y eventos asíncronos. Cuando enviamos una solicitud o un mensaje a un canal, se procesará o aceptará en un momento posterior. A medida que obtenemos datos como parte de la respuesta, los enviamos de vuelta de forma asíncrona. Luego, es responsabilidad del consumidor reaccionar en función de esos cambios.

Examinemos los patrones de comunicación más populares para la propagación de datos:

#### El patrón Pull

Con el patrón Pull (Tirar / Extraer), el consumidor de los datos debe consultar proactivamente a la fuente para obtener actualizaciones y reaccionar ante cualquier información nueva mediante sondeos periódicos (*polling*) (*Figura 8.1: Diagrama del patrón Pull*).

Implementación de una función de sondeo asíncrono (*async polling*):

`patterns.ts`:
```typescript
export interface AsyncRequest<T> { 
    success: boolean; 
    data?: T; 
} 

export async function asyncPoll<T>( 
    fn: () => PromiseLike<AsyncRequest<T>>, 
    pollInterval = 5000, 
    pollTimeout = 30000, 
    abortSignal?: AbortSignal 
): Promise<T> { 
    if (abortSignal?.aborted) { 
        throw new Error("Polling aborted"); 
    } 

    const endTime = new Date().getTime() + pollTimeout; 

    const condition = (resolve: (value: T) => void, reject: (reason?: any) => void): void => { 
        if (abortSignal?.aborted) { 
            reject(new Error("Polling aborted")); 
            return; 
        } 

        Promise.resolve(fn()).then((result) => { 
            const now = new Date().getTime(); 
            if (result.success) { 
                if (result.data === undefined) { 
                    reject(new Error("Successful response must include data")); 
                    return; 
                } 
                resolve(result.data); 
            } else if (now < endTime) { 
                setTimeout(condition, pollInterval, resolve, reject); 
            } else { 
                reject(new Error("Reached timeout. Exiting")); 
            } 
        }) 
        .catch(reject); 
    }; 

    return new Promise(condition); 
}
```

Uso del sondeo:

`patterns.ts`:
```typescript
const result = asyncPoll(async () => { 
    try { 
        const result = await Promise.resolve({ data: "Value" }) 
        if (result.data) { 
            return Promise.resolve({ 
                success: true, 
                data: result, 
            }) 
        } else { 
            return Promise.resolve({ 
                success: false, 
            }) 
        } 
    } catch (err) { 
        return Promise.reject(err) 
    } 
}) 

result.then((d) => { 
    console.log(d.data) // Value 
})
```

Extracción periódica desde un iterador:

`patterns.ts`:
```typescript
const source = [1, 3, 4]; 
const iter = new ListIterator(source); 

function pollOnData(iterator: ListIterator<number>) { 
    while (iterator.hasNext()) { 
        console.log("Processing data:", iterator.next()); 
    } 
} 

// Producer 
setTimeout(() => { 
    source.push(Math.floor(Math.random() * 100)); 
}, 1000); 

// Consumer 
setTimeout(() => { 
    pollOnData(iter); 
}, 2000);
```

#### El patrón Push

En el patrón Push (Empujar / Publicar), el consumidor recibe nuevos valores del productor tan pronto como están disponibles (*Figura 8.2: Diagrama del patrón Push*). Esto elimina la necesidad de que el consumidor realice consultas continuas.

Implementación con RxJS Subject:

`patterns.ts`:
```typescript
import {Subject} from 'rxjs'; 

class Producer { 
    private subject = new Subject<number>(); 

    sendData(data: number) { 
        this.subject.next(data); 
    } 

    subscribe(callback: (data: number) => void) { 
        this.subject.subscribe(callback); 
    } 
}
```

Uso del patrón Push:

`patterns.ts`:
```typescript
// Create a producer instance 
const producer = new Producer(); 

producer.subscribe((data) => { 
    console.log('Subscriber 1 received:', data); 
}); 

producer.subscribe((data) => { 
    console.log('Subscriber 2 received:', data); 
}); 

// Send some data 
producer.sendData(42); 
producer.sendData(100);
```

##### Usos del patrón Push en el mundo real

- **WebSockets:** Para comunicación bidireccional en tiempo real entre cliente y servidor.
- **Notificaciones en aplicaciones móviles:** Notificaciones push que informan de mensajes o alertas sin abrir la aplicación.
- **Servicios de streaming:** Para actualizaciones en tiempo real de contenido multimedia o emisiones en directo.
- **Dispositivos IoT:** Para emitir lecturas de sensores (por ejemplo, temperatura) a un servidor central.

#### El patrón Pull-Push

El patrón Pull-Push es un enfoque híbrido donde el productor no envía directamente la carga completa de datos, sino que envía una notificación indicando el extremo (*endpoint*) o recurso donde el consumidor puede extraer los datos actualizados (*Figura 8.3: Diagrama del patrón Pull-Push*).

Es especialmente útil cuando no se pueden enviar cargas grandes directamente por limitaciones de red o seguridad.

`pull-push.ts`:
```typescript
class Producer { 
    private storage: Storage 
    ... 
} 

class Consumer { 
    private collector: Collector 
    ... 
} 

class Storage { 
    private data: any[] = [] 
    ... 
} 

class Consumer { 
    constructor(private collector: Collector) {} 

    async *pullDataStream() { 
        while (await this.collector.hasMoreData()) { 
            const data = await this.collector.pullData(); 
            for (const item of data) { 
                yield item; 
            } 
            await new Promise(resolve => setTimeout(resolve, 1000)); 
        } 
    } 
} 

// Example usage 
async function example() { 
    const storage = new Storage(); 
    const producer = new Producer(storage); 
    const collector = new Collector(storage); 
    const consumer = new Consumer(collector); 

    // Producer adds data 
    await producer.updateData({ id: 1, value: "First Data" }); 
    await producer.updateData({ id: 2, value: "Second Data" }); 

    // Consumer pulls data using async generator 
    for await (const item of consumer.pullDataStream()) { 
        console.log('Received:', item); 
    } 
} 

example();
```

##### Estrategias de prueba del patrón Pull-Push

Para probar el patrón Pull-Push:
- Realiza pruebas unitarias del productor, el almacenamiento y el consumidor de forma aislada.
- Realiza pruebas de integración para validar el flujo completo de producción, notificación y extracción.

Para ejecutar las pruebas:

```bash
$ npm run test -- pull-push
```

---

### Comprensión de Promises y Futures

Tanto Promises como Futures representan el resultado eventual de una operación asíncrona, proporcionando mecanismos limpios para manipular valores no disponibles de inmediato.

#### Promises

Una Promise representa un contenedor que puede resolverse con un valor en el futuro o rechazarse con un motivo de error.

Ejemplo típico:

`promises.ts`:
```typescript
import fetch from "node-fetch" 

interface Todo { 
    userId: number; 
    id: number; 
    title: string; 
    completed: boolean; 
} 

const pullFromApi = new Promise<Todo>(async (resolve, reject) => { 
    try { 
        const response = await fetch('https://jsonplaceholder.typicode.com/todos/1'); 
        // Check if the response is ok (status in the range 200-299) 
        if (!response.ok) { 
            throw new Error(`HTTP error! status: ${response.status}`); 
        } 
        const json = await response.json(); 
        resolve(json as Todo); 
    } catch (err) { 
        reject(err instanceof Error ? err : new Error('Unknown error occurred')); 
    } 
}); 

(async () => { 
    await pullFromApi 
})()
```

Métodos auxiliares de la API de Promises:
- `Promise.all`: Espera a que todas las promesas se resuelvan o que alguna sea rechazada.
- `Promise.race`: Se resuelve o rechaza tan pronto como una de las promesas finalice.
- `Promise.allSettled`: Espera a que todas las promesas finalicen, devolviendo una lista de resultados con su estado (`fulfilled` o `rejected`).

Ejemplo con `Promise.race` y `Promise.all`:

`patterns.ts`:
```typescript
function delay(ms: number = 1000) { 
    return new Promise((resolve) => setTimeout(resolve, ms)) 
} 

function failAfter(ms: number = 1000) { 
    return new Promise((_, reject) => setTimeout(reject, ms)) 
} 

const races = Promise.race([delay(1000), failAfter(500)]); 
const all = Promise.all([delay(1000), failAfter(1500)]); 

(async () => { 
    races 
        .then((value) => { 
            console.log(value) 
        }) 
        .catch((_) => { 
            console.log("Error") 
        }) 
})(); 

(async () => { 
    all 
        .then((value) => { 
            console.log(value) 
        }) 
        .catch((_) => { 
            console.log("Error") 
        }) 
})()
```

Configuración y uso de `Promise.allSettled`:

`tsconfig.json`:
```json
"lib": [ 
    "dom", 
    "es2015", 
    "es2020" 
],
```

`promises.ts`:
```typescript
const settled = Promise.allSettled([delay(1000), failAfter(500)]); 

(async () => { 
    settled 
        .then((value) => { 
            console.log(value) 
        }) 
        .catch((_) => { 
            console.log("Error") 
        }) 
})()
```

Salida resultante:
```text
[ 
  { status: 'fulfilled', value: undefined }, 
  { status: 'rejected', reason: undefined } 
]
```

#### Futures

A diferencia de las Promises, los Futures presentan tres distinciones clave:
1. **Pereza (*Laziness*):** Son perezosos y no comienzan a ejecutarse hasta que se llama explícitamente a un método como `fork()` o `run()`. Las Promises son ansiosas (*eager*) y comienzan inmediatamente al instanciarse.
2. **Cancelación (*Cancellation*):** Proporcionan mecanismos nativos para abortar una operación asíncrona en curso.
3. **Contexto de ejecución:** Retornan una función de cancelación que permite abortar la tarea.

Implementación de un `Future` personalizado en TypeScript:

`futures.ts`:
```typescript
type Reject = (reason?: any) => void 
type Resolve<T> = (value: T) => void 
type Execution<E, T> = (resolve: Resolve<T>, reject: Reject) => () => void
```

`futures.ts`:
```typescript
class Future<E, T> { 
    private fn: Execution<E, T> 

    constructor(ex: Execution<E, T>) { 
        this.fn = ex 
    } 

    fork(reject: Reject, resolve: Resolve<T>): () => void { 
        return this.fn(resolve, reject) 
    } 

    static success<E, T>(value: T): Future<E, T> { 
        return new Future((resolve) => { 
            resolve(value) 
            return () => {} 
        }) 
    } 

    static fail<E, T>(error: E): Future<E, T> { 
        return new Future((_, reject) => { 
            reject(error) 
            return () => {} 
        }) 
    } 

    then<U>(f: (value: T) => Future<E, U>): Future<E, U> { 
        return new Future((resolve, reject) => { 
            return this.fn((value: T) => f(value).fork(reject, resolve), reject) 
        }) 
    } 
}
```

Uso y cancelación de un Future:

`futures.ts`:
```typescript
const delayedTask = new Future<Error, string>( 
    (resolve, reject) => { 
        const timerId = setTimeout(() => resolve("Hello, Future!"), 1000) 
        return () => clearTimeout(timerId) // Cancellation function 
    }
) 

const uppercaseTask = delayedTask.then((value) => Future.success(value.toUpperCase())) 

const cancelTask = uppercaseTask.fork( 
    (error) => console.error("Task failed:", error), 
    (result) => console.log("Task succeeded:", result), 
) 

// cancelTask();
```

##### Implementación de código abierto de Futures

La biblioteca **Fluture** proporciona una implementación robusta de Futures:

```typescript
import { Future } from 'fluture'; 

const futureValue: Future<number> = Future((reject, resolve) => { 
    setTimeout(() => resolve(42), 1000); 
}); 

futureValue.fork( 
    error => console.error('Error:', error), 
    value => console.log('Value:', value) 
);
```

##### Estrategias de prueba para Futures

Para ejecutar las pruebas de Futures:

```bash
$ npm run test -- futures
```

---

### Exploración de Observables

Un **Observable** representa una secuencia invocable que produce valores o eventos futuros (*Figura 8.4: Representación de Observables*). En RxJS, los observables son perezosos por defecto y no emiten datos hasta que se añade al menos un suscriptor.

Creación de observables con RxJS:

`observables.ts`:
```typescript
import { Observable, of, from } from 'rxjs'; 

of(1, 2, 3, 4, 5); 
of({ id: 1, data: "value" }); 
from([1, 2, 3, 4, 5]); 
from(Promise.resolve("data")); 

function* getNextRandom() { 
    yield Math.random() * 100; 
} 

const randomValues = new Observable<number>((subscriber) => { 
    subscriber.next(1); 
    subscriber.next(2); 
    subscriber.next(3); 
    setInterval(() => { 
        subscriber.next(getNextRandom().next().value); 
    }, 1000); 
});
```

Suscripción a un observable:

`observables.ts`:
```typescript
let origin = from([1, 2, 3, 4, new Error("Error")]); 

origin.subscribe({ 
    next: (v: any) => { 
        console.log("Value accepted: ", v); 
    }, 
    error: (e) => { 
        console.log("Error accepted: ", e); 
    }, 
    complete: () => { 
        console.log("Finished"); 
    } 
}); 

of([1, 2, 3]).subscribe({ 
    next: (values) => console.log("Values:", values), 
    complete: () => console.info("Completed") 
});
```

Reutilización de la secuencia para un nuevo suscriptor:

`observables.ts`:
```typescript
setTimeout(() => { 
    origin.subscribe( 
        (v: any) => { 
            console.log("Value accepted: ", v); 
        } 
    ); 
}, 1000);
```

#### Operadores componibles

En RxJS, los operadores son funciones componibles que transforman flujos de datos a través del método `.pipe()`:

`observables.ts`:
```typescript
import { of, from, interval } from "rxjs" 
import { filter, take, share, map } from "rxjs/operators" 

interval(1000).pipe( 
    take(5), 
    map((v: number) => v * v), 
    tap(v => console.log(`Squared value: ${v}`)) // Side effect moved to tap 
).subscribe(); // Output: Squared value: 0, 1, 4, 9, 16 

of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10).pipe( 
    tap(v => console.log(`Processing value: ${v}`)), // Log before filtering 
    filter((v: number) => v % 3 === 0), 
    tap(v => console.log(`Divisible by 3: ${v}`)) // Log after filtering 
).subscribe(); // Output: Divisible by 3: 3, 6, 9 

from([ 
    { id: 1, name: "Alice", age: 30 }, 
    { id: 2, name: "Bob", age: 25 }, 
    { id: 3, name: "Charlie", age: 35 }, 
]) 
.pipe(map((user) => user?.name)) 
.subscribe((name: string) => console.log(`Name: ${name}`)) // Output: Name: Alice, Name: Bob, Name: Charlie
```

#### Observables fríos (*Cold*) frente a calientes (*Hot*)

- **Cold Observables (Fríos):** La secuencia se produce desde el principio de forma independiente para cada nuevo suscriptor.
- **Hot Observables (Calientes):** El productor emite valores de forma continua e independiente de los suscriptores (como una transmisión en vivo). Los nuevos suscriptores solo reciben los valores emitidos a partir del momento de su suscripción.

Creación de un Hot Observable con `share()`:

`observables.ts`:
```typescript
import { interval } from "rxjs"; 
import { take, share } from "rxjs/operators"; 

const stream$ = interval(1000).pipe(take(5), share()); 

stream$.subscribe((v) => console.log("Value accepted from first subscriber: ", v)); 

setTimeout(() => { 
    stream$.subscribe((v) => { 
        console.log("Value accepted from second subscriber: ", v); 
    }); 
}, 3000);
```

Salida de la ejecución:
```text
Value accepted from first subscriber: 0 
Value accepted from first subscriber: 1 
Value accepted from first subscriber: 2 
Value accepted from second subscriber: 2 
Value accepted from first subscriber: 3 
Value accepted from second subscriber: 3 
Value accepted from first subscriber: 4 
Value accepted from second subscriber: 4
```

#### Planificadores (*Schedulers*) en RxJS

Los planificadores controlan la temporización de las emisiones:
- `null`: Ejecuta tareas de forma síncrona (comportamiento predeterminado).
- `AsyncScheduler`: Ejecuta tareas asíncronamente mediante `setTimeout`.
- `QueueScheduler`: Encola tareas para su ejecución secuencial en una estructura de trampolín (*trampoline*).
- `AsapScheduler`: Ejecuta tareas tan pronto como sea posible mediante `setTimeout(task, 0)`.

#### Manejo de contrapresión (*Backpressure*) en RxJS

La contrapresión ocurre cuando el productor emite datos más rápido de lo que el consumidor puede procesar. Operadores clave:
- `buffer` / `bufferTime` / `bufferCount`: Almacenan en búfer los valores emitidos y los entregan en bloques.
- `throttle` / `throttleTime`: Limitan la frecuencia de emisión ignorando eventos intermedios durante una ventana de tiempo.
- `debounce` / `debounceTime`: Emiten un valor solo tras una pausa de inactividad especificada.

#### Ejemplo usando WebSockets y RxJS

`Websockets.ts`:
```typescript
import { webSocket } from 'rxjs/webSocket'; 
import { throttleTime, bufferTime, map, tap } from 'rxjs/operators'; 
import { Observable } from 'rxjs'; 

const chatSocket = webSocket<string>('ws://localhost:8080/chat'); 

const messageStream$: Observable<string> = chatSocket.pipe( 
    tap(message => console.log('New message received:', message)), // Debugging/logging 
    throttleTime(100), 
    bufferTime(1000), 
    map(messages => messages.join('\n')) 
); 

messageStream$.subscribe({ 
    next: bufferedMessages => { 
        console.log('Buffered messages:', bufferedMessages); 
        updateChatUI(bufferedMessages); 
    }, 
    error: error => console.error('WebSocket error:', error) 
}); 

function updateChatUI(messages: string) { 
    // Update the chat UI with the new messages 
    console.log('Updating chat UI with:', messages); 
}
```

#### Estrategias de prueba para tuberías de observables

Uso de `TestScheduler` y diagramas de canicas (*marble testing*):

```typescript
import { TestScheduler } from 'rxjs/testing'; 

const scheduler = new TestScheduler((actual, expected) => { 
    expect(actual).toEqual(expected); 
}); 

scheduler.run(({ cold, expectObservable }) => { 
    const source$ = cold('a-b-c|', { a: 1, b: 2, c: 3 }).pipe(shareReplay(1)); 
    expectObservable(source$).toBe('a-b-c|', { a: 1, b: 2, c: 3 }); 
});
```

```typescript
import { TestScheduler } from 'rxjs/testing'; 

const testScheduler = new TestScheduler((actual, expected) => { 
    expect(actual).toEqual(expected); 
}); 

testScheduler.run(({ cold, expectObservable }) => { 
    const source$ = cold('---a---b---c|'); 
    expectObservable(source$).toBe('---a---b---c|'); 
});
```

Para ejecutar las pruebas:

```bash
$ npm run test -- observables
```

---

### Resumen

En este capítulo exploramos los conceptos fundamentales de la programación reactiva y su aplicación práctica:
- Los modelos de propagación de cambios **Pull**, **Push** y el modelo híbrido **Pull-Push**.
- Las diferencias entre **Promises** (ansiosas, sin cancelación) y **Futures** (perezosos, cancelables).
- La arquitectura de **Observables** con RxJS, los operadores de programación reactiva funcional (FRP), la distinción entre observables **Cold** y **Hot**, el control de contrapresión y las pruebas temporizadas con diagramas de canicas.

En el próximo capítulo, nos centraremos en las prácticas y técnicas recomendadas para desarrollar aplicaciones TypeScript modernas, robustas y a gran escala.

---

### Preguntas y respuestas

1. **¿En qué se diferencia la programación reactiva de la Programación Orientada a Objetos (POO)?**
   - **Respuesta:** La POO se enfoca en cómo se crean y organizan los objetos, encapsulando estado y comportamiento. Un objeto representa una entidad que interactúa mediante métodos. En cambio, la programación reactiva se centra en el flujo de datos y en cómo los cambios se propagan de forma asíncrona a través del sistema.

2. **¿Cómo se comparan los Observables con el patrón Observer?**
   - **Respuesta:** Aunque comparten la misma base conceptual, los Observables amplían el patrón Observer clásico añadiendo soporte para composiciones funcionales, manejo de flujos asíncronos y operadores componibles (`map`, `filter`, `take`, etc.) que operan sobre secuencias temporales de datos.

3. **¿Cómo se compara la programación reactiva con la programación funcional?**
   - **Respuesta:** La programación funcional trata con funciones puras, inmutabilidad y cálculo determinista. La programación reactiva se ocupa de flujos de datos asíncronos y propagación de eventos. Ambas convergen en la Programación Reactiva Funcional (FRP), donde las transformaciones sobre flujos de eventos se realizan mediante funciones puras y componibles.

---

### Lecturas adicionales

- *Mastering Reactive JavaScript*, de Erich de Souza Oliveira: https://www.packtpub.com/en-ie/product/mastering-reactive-javascript-9781786463388
- *The Reactive Manifesto*: https://www.reactivemanifesto.org/
