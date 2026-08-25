# Parte 2: Patrones de diseño fundamentales en TypeScript

## Capítulo 5: Patrones de diseño de comportamiento para la comunicación entre objetos

Los patrones de comportamiento abordan el desafío de asignar responsabilidades a los objetos de una manera que promueva el bajo acoplamiento y la alta cohesión. Estos patrones tienen como objetivo lograr un equilibrio óptimo entre estos dos conceptos importantes, permitiendo que las interfaces de cliente interactúen con los objetos sin necesidad de comprender sus relaciones internas.

Hemos dividido todos los patrones de comportamiento en dos capítulos lógicos para facilitar su estudio.

En este capítulo, nos centramos en los patrones que facilitan la comunicación y la interacción entre objetos. En el siguiente capítulo, nos centraremos en los patrones de comportamiento que gestionan el estado y el comportamiento de los objetos.

Los patrones cuyos detalles proporcionamos en este capítulo tienen como objetivo desacoplar el emisor de una solicitud de su receptor. Al introducir objetos intermediarios o abstraer comportamientos específicos, estos patrones aseguran que los objetos puedan colaborar sin necesidad de un conocimiento detallado del funcionamiento interno de cada uno.

Examinaremos los siguientes patrones de diseño de comportamiento en este capítulo:

- El patrón Strategy
- El patrón Chain of Responsibility
- El patrón Command
- El patrón Mediator
- El patrón Observer

Nuevamente, para cada patrón, explicaremos su concepto central y propósito, beneficios y posibles inconvenientes, junto con detalles de implementación y mejores prácticas.

Al final de este capítulo, tendrás una comprensión profunda de cómo implementar estos patrones para desacoplar la comunicación entre objetos, haciendo que tus aplicaciones sean más flexibles y fáciles de mantener.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub aquí:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter05_Behavioral_Design_Patterns_Communication

---

### Comprensión de los patrones de diseño de comportamiento

Los patrones de diseño de comportamiento, en general, se centran en la comunicación entre objetos, definiendo abstracciones que gestionan relaciones y responsabilidades. Estos patrones abordan cómo interactúan los objetos y distribuyen responsabilidades haciendo hincapié en el paso eficiente de mensajes y la gestión de referencias.

Un ejemplo destacado es el patrón Observer, que ilustra cómo ciertos patrones utilizan sistemas de eventos nativos para pasar mensajes de manera eficiente. Este patrón es particularmente relevante en el modelo de manejo de eventos de JavaScript.

En el núcleo de los patrones de comportamiento se encuentran varios conceptos clave:

- **Protocolos de comunicación:** Establecen protocolos de comunicación que dictan cómo y cuándo intercambian información los objetos. Esto asegura que las interacciones entre objetos sean estructuradas y predecibles.
- **Flexibilidad en el comportamiento:** Permiten la flexibilidad en el comportamiento, habilitando a los objetos a exhibir diversos comportamientos sin recurrir a lógica codificada rígida (*hardcoded*) o código repetitivo.
- **Encapsulación de la funcionalidad:** Enfatizan la encapsulación de la funcionalidad, utilizando a menudo clases auxiliares e interfaces para empaquetar los comportamientos de los objetos en componentes reutilizables.

Los patrones de comportamiento son especialmente adecuados para abordar desafíos comunes en el diseño de software. Proporcionan soluciones elegantes para acceder a funcionalidades sin llamadas a métodos directas, lo que puede ser beneficioso cuando se trabaja con sistemas complejos o en evolución. Estos patrones también destacan en la gestión de interacciones complejas entre objetos, ofreciendo enfoques estructurados para escenarios donde múltiples objetos necesitan cooperar o comunicarse. Además, proporcionan alternativas para implementar comportamientos variables sin recurrir a una lógica condicional extensa, lo que conduce a un código más limpio y fácil de mantener.

Veamos estos patrones en mayor detalle comenzando con el patrón Strategy.

---

### El patrón Strategy

El patrón Strategy es un patrón de diseño de comportamiento que encapsula una familia de algoritmos dentro de una interfaz común, haciéndolos intercambiables en tiempo de ejecución. Este poderoso patrón permite que un sistema altere dinámicamente su comportamiento cambiando entre diferentes implementaciones de un proceso o lógica de negocio específica.

En esencia, esta abstracción es conceptualmente sencilla pero profundamente impactante, ya que permite que un sistema adapte su comportamiento sin modificar su estructura central.

> [!NOTE]
> Aunque el patrón Strategy proporciona flexibilidad y mantenibilidad, el cambio frecuente entre estrategias puede introducir una sobrecarga de rendimiento, especialmente cuando el cambio desencadena efectos secundarios. Cada vez que se cambia una estrategia, es posible que el sistema deba instanciar un nuevo objeto de estrategia, lo que puede ser costoso en términos de rendimiento si esto ocurre en un bucle cerrado o en un contexto de alta frecuencia.

#### Cuándo usar el patrón Strategy

El patrón Strategy es particularmente útil en los siguientes escenarios:

- **Variantes de algoritmos:** Cuando tienes múltiples variantes de un algoritmo y necesitas cambiar entre ellas dinámicamente según las condiciones en tiempo de ejecución. Por ejemplo, en un sistema de cálculo de impuestos, se pueden aplicar diferentes estrategias según el estado civil, el nivel de ingresos o el estado de discapacidad de una persona.
- **Encapsulación del comportamiento:** Cuando deseas aislar los detalles de implementación de un algoritmo de su uso. Esta separación permite que los clientes permanezcan ajenos al algoritmo específico que se utiliza, promoviendo el bajo acoplamiento.
- **Evitar la complejidad condicional:** En lugar de utilizar múltiples sentencias condicionales para determinar el comportamiento, el patrón Strategy aprovecha el polimorfismo para alterar el comportamiento, lo que da como resultado un código más limpio y fácil de mantener.
- **Configurabilidad:** Cuando un sistema necesita ser altamente configurable, permitiendo a los usuarios o administradores cambiar su comportamiento sin modificar el código fuente.
- **Comunicación entre objetos:** En el patrón Strategy, la comunicación entre objetos se facilita a través de la clase de contexto que mantiene una referencia a un objeto de estrategia (comúnmente con el prefijo `Strategy`). Esta clase de contexto delega tareas específicas al objeto de estrategia sin importarle qué algoritmo se esté utilizando.

#### Comparación: Patrón State frente a patrón Strategy

Dado que acabamos de presentar el patrón Strategy, es importante señalar que a menudo se compara con el patrón State (que se presentará en el próximo capítulo). Ambos patrones encapsulan el comportamiento y permiten cambios dinámicos, pero tienen propósitos diferentes:
- El patrón **Strategy** se utiliza cuando deseas seleccionar un algoritmo de una familia de algoritmos en tiempo de ejecución (por ejemplo, un algoritmo de ordenación según la preferencia del usuario).
- En contraste, el patrón **State** se utiliza cuando el comportamiento de un objeto necesita cambiar en función de su estado interno (por ejemplo, un reproductor multimedia puede comportarse de manera diferente en los estados de reproducción, pausa o detención, y cada estado encapsula su propio comportamiento).

#### Diagrama UML

El patrón Strategy, en su núcleo, consta de una interfaz con múltiples implementadores y un objeto de contexto que gestiona la estrategia actual (*Figura 5.1: El patrón Strategy*):

- `Strategy` define el método común para todas las estrategias concretas.
- `ConcreteStrategyA` y `ConcreteStrategyB` son implementaciones de la interfaz `Strategy`.
- `Context` es la clase que gestiona la estrategia actual y proporciona un método para ejecutarla.

Esta estructura permite cálculos de facturación flexibles. `Context` puede cambiar entre diferentes estrategias en tiempo de ejecución, lo que brinda la capacidad de alterar el comportamiento de facturación dinámicamente según diversas condiciones o reglas comerciales.

#### Implementación clásica

Veamos una implementación del patrón Strategy utilizando un ejemplo simple de algoritmos de ordenación:

`Strategy.ts`:
```typescript
interface SortStrategy { 
    sort(data: number[]): number[]; 
} 

class BubbleSort implements SortStrategy { 
    sort(data: number[]): number[] { 
        // bubble sort implementation 
    } 
} 

class QuickSort implements SortStrategy { 
    sort(data: number[]): number[] { 
        //quick sort implementation 
        if (data.length <= 1) return data; 
    } 
} 

class Sorter { 
    constructor(private strategy: SortStrategy) { 
        if (!strategy || typeof strategy.sort !== 'function') { 
            throw new Error('Invalid strategy provided'); 
        } 
    } 
} 

const data = [64, 34, 25, 12, 22, 11, 90]; 
const sorter = new Sorter(new BubbleSort()); 
sorter.setStrategy(new QuickSort());
```

El código define un mecanismo de ordenación flexible mediante el patrón Strategy en TypeScript. Comienza con una interfaz `SortStrategy` que exige un método `sort`, que cualquier algoritmo de ordenación debe implementar. Se proporcionan dos implementaciones concretas: `BubbleSort` y `QuickSort`. El patrón permite algoritmos flexibles e intercambiables, facilitando la adición de nuevas estrategias de ordenación o el cambio entre ellas sin modificar el código del cliente.

#### Pruebas (*Testing*)

La prueba más fundamental que puedes escribir para este patrón es verificar que cada objeto `Strategy` funcione como se espera.

- Utiliza simulaciones (*mocking*) para simular la clase de contexto que utiliza las estrategias. Esto te permite probar cómo interactúa el contexto con diferentes estrategias sin depender de sus implementaciones reales.
- Además, puedes probar la lógica que determina qué estrategia se aplica internamente en función del contexto, agregando casos de prueba que activen el método `setStrategy` y verifiquen que se utilice la estrategia correcta.

Para ejecutar los casos de prueba provistos en `strategy.test.ts`:

```bash
$ npm run test -- strategy
```

#### Críticas

- **Complejidad frente a beneficio:** Introduce clases e interfaces adicionales. Para escenarios simples con solo dos o tres variaciones, una sentencia `if` simple o una función lambda pueden ser más apropiadas y fáciles de entender.
- **Uso excesivo en escenarios simples:** Para variaciones muy simples o cuando es poco probable que se agreguen más estrategias en el futuro, este patrón puede ser excesivo.
- **Mayor número de clases:** Cada nueva estrategia normalmente requiere una nueva clase, lo que puede provocar una proliferación de clases en el proyecto.
- **Compartición de contexto:** Si las estrategias necesitan compartir estado o contexto, es posible que debas pasar parámetros adicionales a los métodos de estrategia o mantener un estado compartido en el objeto de contexto, lo que a veces genera un acoplamiento más estrecho.

#### Casos de uso de la vida real

Este patrón es muy popular en frameworks de autenticación donde deseas definir múltiples estrategias (por ejemplo, `nuxt-auth` en Vue.js o `Passport.js` en Node.js). Aceptan un tipo `StrategyOptions` y definen múltiples estrategias de autenticación para proveedores como GitHub, Facebook, Google y otros proveedores OAuth.

A continuación, examinaremos en detalle el patrón Chain of Responsibility.

---

### El patrón Chain of Responsibility

El patrón Chain of Responsibility (Cadena de Responsabilidad) es un patrón de diseño de comportamiento que permite que un objeto sea procesado como una serie de manejadores independientes a lo largo de una cadena. Cada manejador procesa la referencia del objeto actual y decide si la pasa al siguiente manejador o la procesa antes de hacerlo.

Imagina una jerarquía corporativa que atiende quejas de clientes:
1. El empleado de recepción intenta resolver el problema.
2. Si no puede, lo escala a su supervisor.
3. El supervisor intenta solucionar el problema.
4. Si sigue sin resolverse, pasa al gerente del departamento.
5. Finalmente, si es necesario, llega al director general (CEO).

Cada nivel en esta jerarquía representa un manejador en la cadena de responsabilidad. La fortaleza del patrón radica en su capacidad para distribuir responsabilidades entre múltiples objetos, evitando la concentración de lógica en una sola función o clase.

#### Cuándo usar el patrón Chain of Responsibility

- **Escenarios de manejadores múltiples:** Cuando tienes múltiples objetos que pueden manejar una solicitud y el manejador no se conoce a priori.
- **Procesamiento de solicitudes desacoplado:** Cuando deseas desacoplar al emisor de una solicitud de sus receptores.
- **Configuración dinámica de la cadena:** Cuando necesitas la capacidad de agregar, eliminar o reordenar manejadores en tiempo de ejecución.
- **Manejo jerárquico de solicitudes:** En sistemas con una jerarquía natural (como el manejo de eventos en interfaces gráficas o sistemas de registro de eventos), donde las solicitudes deben subir en la jerarquía hasta ser atendidas.

#### Comparación: Patrón Mediator frente a patrón Chain of Responsibility

Tanto el patrón Mediator como el patrón Chain of Responsibility tienen responsabilidades similares en cuanto al desacoplamiento y facilitación de la comunicación:
- El patrón **Chain of Responsibility** permite que una solicitud pase a lo largo de una cadena de manejadores potenciales hasta que uno de ellos la procese.
- En contraste, el patrón **Mediator** centraliza la comunicación entre componentes, permitiéndoles interactuar indirectamente a través de un objeto mediador sin que sean conscientes unos de otros. Por ejemplo, en una aplicación de chat, los usuarios (colegas) se comunican a través de una sala de chat (mediador).

#### Diagrama de clases UML

- El objeto principal es `Request` (*Figura 5.2: El objeto Request*), que se pasa a lo largo de la cadena y se transforma según los manejadores adjuntos.
- Luego, se adjunta la cadena de manejadores (*Figura 5.3: Chain of Responsibility*). `Handler` es una clase abstracta que define la interfaz para manejar solicitudes y mantener la cadena (`setNext()`, `handle()`). `ConcreteHandlerA` y `ConcreteHandlerB` son implementaciones específicas encadenadas entre sí.

#### Implementación clásica

Consideremos un sistema de tickets de soporte al cliente:

```typescript
constructor( 
    private id: number, 
    private customer: string, 
    private issue: string, 
    private priority: number, 
) {} 
// Getter and Setter methods here }
```

La cadena real se define en la clase `SupportHandler` y sus subclases:

`Chain-of-responsibility.ts`:
```typescript
if (ticket.getPriority() <= 1) { 
    ticket.setResolution("Resolved by Front Desk: General inquiry handled") 
    console.log(`Ticket ${ticket.getId()} handled by Front Desk`) 
} else if (this.nextHandler) { 
    this.nextHandler.handle(ticket) 
} 
} 
} 

class TechnicalSupportHandler extends SupportHandler { 
    handle(ticket: SupportTicket): void { 
        if (ticket.getPriority() <= 3) { 
            ticket.setResolution("Resolved by Technical Support: Technical issue addressed") 
            console.log(`Ticket ${ticket.getId()} handled by Technical Support`) 
        } else if (this.nextHandler) { 
            this.nextHandler.handle(ticket) 
        } 
    } 
} 

const frontDesk = new FrontDeskHandler(); 
const techSupport = new TechnicalSupportHandler(); 
frontDesk.setNext(techSupport); 

const tickets: SupportTicket[] = [ 
    new CustomerSupportTicket(1, "John Doe", "General inquiry", 1), 
    new CustomerSupportTicket(2, "Jane Smith", "Software bug", 2), 
]; 

tickets.forEach(ticket => { 
    console.log(`Processing ticket ${ticket.getId()} for ${ticket.getCustomer()}`); 
    frontDesk.handle(ticket); 
    console.log(`Resolution: ${ticket.getResolution()}\n`); 
});
```

La clase abstracta `SupportHandler` define la estructura para los manejadores y el método `setNext` para encadenar fácilmente los manejadores. `FrontDeskHandler` maneja tickets de baja prioridad y `TechnicalSupportHandler` maneja tickets de prioridad media.

#### Pruebas (*Testing*)

Al escribir pruebas, debes verificar que los tickets se procesen correctamente, que la cadena se recorra según lo esperado y que el sistema se comporte de manera consistente. Utiliza simulaciones (*mocks*) para asegurarte de que cuando un manejador no pueda procesar una solicitud, la pase correctamente al siguiente manejador de la cadena.

Para ejecutar los casos de prueba en `chain-of-responsibility.test.ts`:

```bash
$ npm run test -- chain-of-responsibility
```

#### Críticas

- **Ruptura de la cadena:** La cadena puede romperse si un manejador no pasa la solicitud al siguiente o lanza una excepción no controlada.
- **Sobrecarga de rendimiento:** Las cadenas largas pueden introducir latencia, ya que cada solicitud debe recorrer múltiples objetos.
- **Complejidad y depuración:** A medida que la cadena crece, puede ser difícil depurar y rastrear el flujo de control.
- **Sin garantía de procesamiento:** No hay garantía de que una solicitud sea manejada si llega al final de la cadena sin ser procesada; se debe incluir un manejador predeterminado.
- **Posibilidad de referencias circulares:** Si no se gestiona con cuidado, se pueden crear cadenas circulares que generen bucles infinitos.

#### Casos de uso de la vida real

Los manejadores de middleware de Express.js son un caso de uso clásico del patrón Chain of Responsibility. En el método `handle` de Express.js:

```javascript
var router = this._router; 
router.handle(req, res, done);
```

Aquí, el enrutador representa el objeto `RequestHandler`. Cada middleware registrado mediante `app.use()` agrega un nuevo manejador a la cadena de ejecución.

A continuación, examinaremos el patrón Command.

---

### El patrón Command

El patrón Command es un patrón de diseño de comportamiento que utiliza un objeto para encapsular acciones que se ejecutan en nombre del emisor original. Este patrón proporciona varios beneficios:

- **Separación de conceptos:** Separa el emisor de la operación del ejecutor real (el código que realiza la acción).
- **Extensibilidad:** Se pueden agregar nuevos comandos sin cambiar el código existente.
- **Puesta en cola y registro:** Los comandos se pueden poner en cola, registrar o deshacer fácilmente.
- **Parametrización:** Los objetos se pueden parametrizar con diferentes solicitudes.

Una analogía es un restaurante: el cliente (cliente) crea un pedido (comando). El camarero (invocador) toma el pedido y el chef (receptor) prepara el plato según el pedido.

#### Cuándo usar el patrón Command

- **Desacoplamiento de emisor y receptor:** Cuando necesitas distinguir el objeto que inicia una operación del que la ejecuta.
- **Parametrización de objetos con acciones:** Cuando deseas configurar objetos con diferentes solicitudes en tiempo de ejecución.
- **Implementación de funciones de Deshacer/Rehacer (*Undo/Redo*):** Excelente para editores de texto o herramientas de diseño donde cada comando almacena el estado necesario para revertir sus efectos.
- **Puesta en cola de operaciones:** Cuando necesitas poner en cola solicitudes y ejecutarlas en momentos diferentes o en un orden específico.
- **Creación de comandos compuestos (macros):** Cuando necesitas implementar operaciones complejas compuestas por secuencias de operaciones más simples.
- **Soporte de comportamiento transaccional:** Para garantizar que todas las operaciones de una transacción se completen o se realice un rollback en caso de fallo.

#### Comparación: Patrón Command frente a patrón Observer

Tanto el patrón Command como el patrón Observer buscan desacoplar componentes:
- El patrón **Command** encapsula solicitudes como objetos independientes, permitiendo parametrizar acciones e implementar características como deshacer/rehacer.
- En contraste, el patrón **Observer** permite que un objeto (el sujeto) notifique a múltiples observadores sobre cambios en su estado sin estar estrechamente acoplado a ellos (por ejemplo, múltiples pantallas que actualizan los precios de las acciones automáticamente cuando cambian).

#### Diagrama de clases UML

El diagrama de clases del patrón Command (*Figura 5.4: El patrón Command*) contiene:
- `Command`: La interfaz que declara el método `execute()`.
- `ConcreteCommand`: Implementaciones específicas que vinculan una clase `Receiver` con una acción.
- `Receiver`: La clase que sabe cómo ejecutar la lógica de la operación.
- `Invoker`: La clase que sabe cuándo enviar el comando a los receptores.
- `Client`: Crea y configura los comandos concretos, el receptor y el invocador.

#### Implementación clásica

`Command.ts`:
```typescript
turnOff(): void { 
    console.log("Light is turned off") 
} 
} 

class TurnOnLightCommand implements Command { 
    constructor(private light: Light) {} 
    execute(): void { 
        this.light.turnOn() 
    } 
} 

class TurnOffLightCommand implements Command { 
    constructor(private light: Light) {} 
    execute(): void { 
        this.light.turnOff() 
    } 
}
```

Definición del controlador (`SmartHomeController`) que acepta comandos del cliente:

`Command.ts`:
```typescript
private commands: Command[] = []; 

addCommand(command: Command): void { 
    this.commands.push(command); 
} 

executeCommands(): void { 
    this.commands.forEach(command => command.execute()); 
    this.commands = []; 
} 
} 

const light = new Light(); 
const controller = new SmartHomeController(); 
controller.addCommand(new TurnOnLightCommand(light)); 
controller.addCommand(new TurnOffLightCommand(light)); 
controller.executeCommands();
```

El patrón desacopla el solicitante de la acción (`controller`) del objeto que realiza la acción (`light`). La estructura permite agregar nuevos comandos y dispositivos sin modificar el código existente.

#### Pruebas (*Testing*)

Para probar que los comandos se ejecutan correctamente y que `SmartHomeController` gestiona los comandos según lo esperado, utiliza receptores simulados (*mock receivers*).

Para ejecutar los casos de prueba provistos en `command.test.ts`:

```bash
$ npm run test -- command
```

#### Críticas

- **Mayor complejidad y abstracción:** Introduce capas adicionales de indirección, lo que puede dificultar el seguimiento del flujo de ejecución para desarrolladores no familiarizados con el código.
- **Sobrecarga de rendimiento potencial:** Introduce llamadas a métodos adicionales (`execute()`), que aunque generalmente insignificantes, pueden influir en aplicaciones críticas de rendimiento extremo.
- **Riesgo de sobreingeniería:** Para aplicaciones simples con operaciones estáticas, implementar el patrón Command puede generar una sobrecarga de mantenimiento sin beneficios reales.
- **Riesgo de proliferación de clases:** Puede derivar en demasiadas clases pequeñas y específicas si no se gestiona con criterio.

#### Casos de uso de la vida real

La biblioteca **Redux** para la gestión del estado en aplicaciones React es un ejemplo del patrón Command:
- Una **Action** de Redux encapsula el evento o comando a realizar:
  ```typescript
  const addTodoAction: Action = { 
      type: 'todos/addTodo', 
      payload: 'Buy groceries' 
  }
  ```
- El **Reducer** actúa como manejador, recibiendo el estado actual y la acción para producir un nuevo estado:
  ```typescript
  (state: TodoState, action: Action) => TodoState
  ```

A continuación, examinaremos el patrón Mediator.

---

### El patrón Mediator

El patrón Mediator (Mediador) es un patrón de diseño de comportamiento que coordina la comunicación entre múltiples objetos o componentes. Introduce un objeto mediador que actúa como intermediario, gestionando las interacciones entre los diversos elementos de un sistema.

Sus características clave son:
- **Comunicación centralizada:** El mediador actúa como concentrador (*hub*), simplificando la comunicación entre diferentes partes del sistema.
- **Dependencias reducidas:** Los componentes individuales no necesitan tener conocimiento directo entre sí.
- **Interfaces simplificadas:** Oculta la complejidad de los subsistemas a los clientes.
- **Flexibilidad y mantenibilidad:** Los cambios en las relaciones entre subsistemas se localizan en el mediador.

Una analogía es un abogado o apoderado (mediador) que actúa en tu nombre (cliente) para realizar todos los trámites y comunicaciones complejas con una agencia gubernamental (subsistema).

> [!NOTE]
> Es crucial mantener límites claros en las responsabilidades del mediador. Sobrecargarlo con demasiada lógica puede convertirlo en un monolito difícil de mantener.

#### Cuándo usar el patrón Mediator

- **Reducción del acoplamiento y centralización de la comunicación:** Para mantener un único punto de comunicación y permitir cambios en un conjunto de objetos sin afectar a los demás.
- **Simplificación de interacciones complejas:** Para gestionar relaciones intrincadas entre múltiples objetos, reemplazando llamadas a métodos directas entre ellos por una coordinación mediada.

#### Diagrama de clases UML

En la *Figura 5.5: El patrón Mediator*:
- La interfaz `Mediator` es implementada por la clase `ConcreteMediator`.
- `ConcreteMediator` mantiene una relación de composición con la clase abstracta `Workhorse`, agregando todos los objetos que necesita coordinar (`SingleTaskWorker`, `BatchWorker`).
- Los trabajadores heredan de `Workhorse`, lo que les permite utilizar el `Mediator` para comunicarse entre sí sin acoplamiento directo.

#### Implementación clásica

`mediator.ts`:
```typescript
if (message.startsWith("batch_job_completed")) { 
    this.workerB.performWork() 
} 
} 
}
```

Clases de trabajadores derivadas de `Workhorse`:

`mediator.ts`:
```typescript
this.mediator?.triggerEvent(this, "batch_job_completed") 
} 

public finalize(): void { 
    console.log("Performing final work in BatchWorker") 
    this.mediator?.triggerEvent(this, "final_job_completed") 
} 
} 

class SingleTaskWorker extends Workhorse { 
    public performWork(): void { 
        console.log("Performing work in SingleTaskWorker") 
        this.mediator?.triggerEvent(this, "single_job_completed") 
    } 
}
```

Uso por parte del cliente:

`mediator.ts`:
```typescript
const workerA = new BatchWorker(); 
const workerB = new SingleTaskWorker(); 
const mediator = new WorkerCenter(workerA, workerB); 
workerA.performWork();
```

El cliente interactúa directamente con los trabajadores, pero el mediador gestiona la comunicación entre objetos. `performWork` inicia toda la cadena de eventos de forma desacoplada.

#### Pruebas (*Testing*)

Al probar este patrón, verifica que el mediador responda a los eventos de los objetos que escucha y delegue los mensajes en el orden correspondiente. Utiliza componentes simulados (*mocks*) que interactúen con el mediador para aislar su comportamiento.

Para ejecutar las pruebas en `mediator.test.ts`:

```bash
$ npm run test -- mediator
```

#### Críticas

- **Desbordamientos de pila (*Stack overflows*):** Si un servicio llama a otro a través del mediador, existe el riesgo de activar involuntariamente la misma función de nuevo, creando un bucle infinito:
  ```typescript
  if (message.startsWith("single_job_completed")) { 
      this.workerB.performWork(); // stack overflow error 
  }
  ```
- **Complejidad en la depuración:** Como el mediador es el punto central de interacción, puede convertirse en un cuello de botella y fuente de errores difíciles de rastrear.
- **Riesgo de convertirse en un objeto monolítico (*God object*):** A medida que más componentes interactúan a través de él, puede acumular demasiadas responsabilidades, violando el Principio de Responsabilidad Única (SRP).

#### Casos de uso de la vida real

- **Aplicaciones de sala de chat:** Usuarios que se comunican entre sí enviando mensajes al mediador de la sala de chat, que los reenvía al destinatario adecuado.
- **Interacción entre elementos de UI:** Un botón que completa una operación y notifica al mediador, el cual actualiza el contador de un icono independiente en otra parte de la interfaz.

A continuación, examinaremos el último patrón de este capítulo: el patrón Observer.

---

### El patrón Observer

El patrón Observer es un patrón de diseño de comportamiento que implementa un sistema para que los objetos publiquen eventos y otros objetos se suscriban a ellos y actúen en consecuencia. Este patrón también se conoce como el patrón **Publish-Subscribe** (Publicador-Suscriptor).

Consta de dos componentes principales:
- **Sujeto / Publicador (*Subject / Publisher*):** Facilita la publicación de nuevos eventos a la lista registrada de suscriptores.
- **Observador / Suscriptor (*Observer / Subscriber*):** Objeto con un método que se invoca cuando cambia el estado del Sujeto.

Una analogía es la suscripción a un periódico: la editorial (sujeto) mantiene una lista de suscriptores (observadores). Cuando se publica una nueva edición, todos los suscriptores reciben una notificación automáticamente, y pueden unirse o cancelar su suscripción en cualquier momento.

#### Cuándo usar el patrón Observer

- **Comunicación de uno a muchos entre objetos:** Cuando un objeto necesita publicar eventos a múltiples objetos de forma desacoplada.
- **Disparo de eventos a diferentes partes de la aplicación sin acoplar dependencias:** Los suscriptores reciben actualizaciones y ejecutan su propia lógica interna o actualizan su estado sin necesidad de compartir referencias directas entre objetos.

#### Comparación: Patrón Observer frente a patrón Chain of Responsibility

Ambos patrones tienen propósitos similares, pero difieren en su alcance:
- Se puede implementar la Cadena de Responsabilidad utilizando Observables, pero no al revés.
- El patrón **Observer** cubre un caso más general, ya que permite suscripciones dinámicas y la composición de operadores observables.

#### Diagrama de clases UML

En la *Figura 5.6: El patrón Observer*:
- `Subject` representa el objeto que contiene el estado y notifica a los suscriptores de cualquier actualización.
- `Subscriber` representa el objeto observador que escucha eventos de `Subject`.
- El objeto `Subject` puede agregar o eliminar suscriptores dinámicamente en tiempo de ejecución.

#### Implementación clásica

`Observer.ts`:
```typescript
} 

public notify(message?: any): void { 
    console.log("Notifying all subscribers") 
    this.subscribers.forEach((s) => s.notify(message)) 
} 
}
```

Implementación de `ConcreteSubscriber`:

`Observer.ts`:
```typescript
} 
} 

class ConcreteSubscriber implements Subscriber { 
    private state: any; 

    constructor(private subject: ConcreteSubject) {} 

    public notify(message: any): void { 
        this.state = message; 
        console.log(`ConcreteSubscriber: Received update with state: ${this.state}`); 
    } 
}
```

Uso por parte del cliente:

```typescript
const subject = new ConcreteSubject(); 
const subscriberA = new ConcreteSubscriber(subject); 
subject.addSubscriber(subscriberA); 

const subscriberB = new ConcreteSubscriber(subject); 
subject.addSubscriber(subscriberB); 

subject.setState(19); 
subject.removeSubscriber(subscriberB); 
subject.setState(21);
```

La clase `Subject` gestiona la lista de suscriptores, actualiza su propiedad de estado y llama a `notify()`, desacoplando la comunicación entre el publicador y los suscriptores.

#### Pruebas (*Testing*)

Al probar el patrón Observer:
- Verifica que la clase `Subject` limpie adecuadamente sus recursos y elimine referencias para evitar fugas de memoria.
- Comprueba que los métodos para cancelar la suscripción eliminen eficazmente a los suscriptores de la lista.
- Utiliza observadores simulados (*mocks*) para verificar que reciban las actualizaciones esperadas del sujeto.

Para ejecutar las pruebas en `observer.test.ts`:

```bash
$ npm run test -- observer
```

#### Críticas

- **Fugas de memoria (*Memory leaks*):** Si los sujetos retienen referencias fuertes a los observadores y estos no se desuscriben adecuadamente, los objetos no podrán ser recolectados por el recolector de basura (*garbage collector*).
- **Problemas de rendimiento:** Notificar a un número masivo de observadores ocurre en tiempo lineal (O(n)), lo que puede introducir retrasos perceptibles en entornos monohilo.
- **Actualizaciones inesperadas o en cascada:** El bajo acoplamiento puede hacer que sea difícil predecir el orden exacto de ejecución, y las actualizaciones en cascada (donde una actualización dispara otra) pueden generar errores sutiles.
- **Sobrecarga en escenarios simples:** Puede introducir una complejidad innecesaria para relaciones directas de uno a uno.

#### Casos de uso de la vida real

Este patrón es la base de la programación reactiva. En el Capítulo 8, exploraremos en detalle los **Observables** y el uso de **RxJS** para crear, operar y combinar secuencias reactivas a escala.

---

### Resumen

Este capítulo exploró cinco patrones de diseño de comportamiento clave para la comunicación de objeto a objeto, permitiendo interacciones flexibles y desacopladas sin complejidad innecesaria:

- El patrón **Strategy** ofrece algoritmos intercambiables en tiempo de ejecución sin afectar el código del cliente.
- El patrón **Chain of Responsibility** crea un flujo operativo modular a través del procesamiento secuencial de solicitudes por múltiples manejadores.
- El patrón **Command** encapsula solicitudes en objetos independientes, separando la iniciación del manejo de la acción.
- El patrón **Mediator** centraliza las interacciones entre objetos, simplificando la comunicación y evitando dependencias directas.
- El patrón **Observer** implementa un modelo publicador-suscriptor donde los objetos reaccionan a cambios de estado en otros promoviendo el bajo acoplamiento.

Dominar estos patrones te prepara para diseñar sistemas flexibles y mantenibles con una comunicación clara y manejable entre objetos.

En el próximo capítulo, profundizaremos en los patrones de diseño de comportamiento para gestionar el estado y el comportamiento.

---

### Preguntas y respuestas

1. **¿En qué se diferencia el patrón Mediator del patrón Observer?**
   - **Respuesta:** Ambos patrones facilitan la comunicación entre componentes del sistema, pero sus objetivos e implementaciones difieren. El patrón Mediator tiene como objetivo eliminar la comunicación directa entre componentes, centralizando la lógica de interacción dentro de un mediador; este conoce las estructuras dependientes y maneja las llamadas en función de los eventos recibidos. En contraste, el patrón Observer promueve un diseño más débilmente acoplado donde el publicador desconoce los detalles de los suscriptores específicos, y los suscriptores deciden de forma independiente cómo responder o ignorar los mensajes recibidos.

2. **¿En qué se diferencia el patrón Chain of Responsibility del patrón Decorator?**
   - **Respuesta:** El patrón Decorator se centra en extender el comportamiento de un único objeto envolviéndolo con funcionalidades adicionales sin interrumpir el flujo de las solicitudes. Por otro lado, el patrón Chain of Responsibility permite que múltiples objetos procesen una solicitud secuencialmente, con la posibilidad de detener el flujo de la solicitud si se cumple una condición determinada.
