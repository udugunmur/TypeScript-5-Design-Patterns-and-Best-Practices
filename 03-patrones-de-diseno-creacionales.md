# Parte 2: Patrones de diseño fundamentales en TypeScript

## Capítulo 3: Patrones de diseño creacionales

Al desarrollar aplicaciones, con frecuencia diseñas y gestionas objetos. Los creas dinámicamente o los asignas a variables para su uso posterior. Si no se controla, esto puede dar lugar a un código frágil debido a las numerosas formas alternativas de creación de objetos o a una gestión inadecuada de su ciclo de vida, lo que resulta en fugas de memoria.

La primera y más fundamental categoría de patrones que exploraremos en este libro son los patrones de diseño creacionales.

Comenzarás aprendiendo cómo el patrón **Singleton** garantiza que solo se mantenga una instancia de un objeto a lo largo de la vida de un programa. Luego, cubriremos el patrón **Prototype**, que te enseñará cómo copiar objetos existentes sin tener que recrearlos desde cero.

Utilizando el patrón **Builder**, comprenderás cómo agilizar la construcción de objetos complejos dividiendo el flujo de construcción en una representación más legible.

A continuación, aprenderás cómo el patrón **Factory Method** ayuda a determinar el momento adecuado para instanciar objetos de un tipo específico en tiempo de ejecución. Finalmente, el patrón **Abstract Factory** te mostrará cómo usar interfaces para modelar la creación de objetos relacionados, dejando los detalles de implementación a fábricas concretas en tiempo de ejecución.

En este capítulo, cubriremos los siguientes temas principales:

- Patrones de diseño creacionales
- El patrón Singleton
- El patrón Prototype
- El patrón Builder
- El patrón Factory Method
- El patrón Abstract Factory

Al final de este capítulo, tendrás un conocimiento profundo de cada uno de los principales patrones creacionales y podrás aplicarlos prácticamente en tus aplicaciones. También obtendrás las perspectivas necesarias para utilizar estos patrones solo cuando sea apropiado, evitando la optimización prematura y las soluciones inadecuadas.

> [!NOTE]
> Los enlaces a todas las fuentes mencionadas en el capítulo, así como los materiales de lectura complementarios, se proporcionan en la sección *Lecturas complementarias* hacia el final de este capítulo.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter03_Creational_Design_Patterns

---

### Patrones de diseño creacionales

Cuando declaras interfaces y clases en TypeScript, el compilador utiliza esta información para comprobaciones de tipo y aserciones. En tiempo de ejecución, el navegador o el servidor evalúa el código y gestiona estos objetos a lo largo del ciclo de vida de la aplicación. A veces, los objetos se crean al inicio de la aplicación. Por ejemplo, en el capítulo anterior, viste la creación de un objeto de aplicación de Express.js:

```typescript
const app = express();
```

Otras veces, los objetos se crean dinámicamente utilizando un descriptor de objetos. Por ejemplo, en el Capítulo 2, aprendiste a crear elementos `span` de HTML:

```typescript
const span = document.createElement("span");
```

Ambos enfoques abordan la creación de objetos, centrándose en cómo instanciar un tipo de objeto y almacenarlo. Reflexionar sobre esto revela dos fases distintas:

1. **Creación de un objeto de un tipo específico o para un propósito específico:** Deseas crear un objeto durante el tiempo de ejecución de la aplicación manteniendo una forma coherente y fácil de usar para instanciar estos objetos. A menudo, necesitas controlar los parámetros que se utilizan, la categoría de objetos a crear o cómo clonar objetos basados en los existentes.
2. **Gestión del ciclo de vida del objeto:** Necesitas controlar el número de instancias de objetos y su almacenamiento. Además, es crucial destruir de forma segura las instancias cuando ya no sean necesarias.

Al aprender sobre los patrones de diseño creacionales, adquirirás las habilidades necesarias para crear y gestionar de manera flexible objetos de cualquier tipo. Además, al separar el proceso de creación de objetos de su implementación concreta, puedes lograr un sistema desacoplado. El uso de interfaces con métodos análogos que describen los tipos de objetos que creas, en lugar de cómo crearlos, te permite proporcionar diferentes implementaciones en tiempo de ejecución sin alterar el algoritmo general ni la lógica condicional. Gestionar referencias a objetos en tiempo de ejecución puede volverse problemático si se microgestiona o se permite que diverja, por lo que tener una abstracción simple para ceder objetos bajo demanda resulta beneficioso.

La *Figura 3.1: Diagrama que ilustra las fases de creación y gestión de objetos, enfatizando el papel de los patrones de diseño creacionales en aplicaciones TypeScript* ilustra los conceptos clave que se tratarán en este capítulo:

- **Declaración de objetos (tipos e interfaces):** Esta fase implica definir la estructura y el comportamiento de los objetos a través de tipos o interfaces en TypeScript.
- **Creación de objetos:** Una vez declarada la estructura del objeto, se crea durante el tiempo de ejecución según sea necesario. Esta fase representa la instanciación de objetos.
- **Mecanismos reutilizables:** Los patrones de diseño creacionales proporcionan mecanismos reutilizables para la creación de objetos. Estos patrones ofrecen enfoques estandarizados para crear objetos, mejorando la mantenibilidad y la flexibilidad del código.
- **El patrón Singleton:** Uno de los patrones creacionales que se discutirán en este capítulo es el patrón Singleton. Garantiza que solo exista una instancia de un objeto a lo largo del ciclo de vida de la aplicación.
- **Gestión de objetos:** Después de la creación, los objetos deben gestionarse de manera efectiva. Esto incluye tareas como el seguimiento de instancias, el control de su ciclo de vida y la garantía de una destrucción adecuada cuando ya no sean necesarios.
- **Destrucción de objetos:** La fase final implica desechar de forma segura los objetos cuando ya no se requieran. Esto ayuda a prevenir fugas de memoria y optimiza el uso de recursos.

La creación y gestión de objetos son cruciales para ciertos tipos de aplicaciones, por lo que es importante utilizar los patrones adecuados para cada caso de uso.

En las secciones siguientes, aprenderemos más sobre los patrones de diseño creacionales, comenzando con el patrón Singleton.

---

### El patrón Singleton

El patrón Singleton es un patrón de diseño creacional que garantiza que una clase tenga solo una instancia y proporciona un punto de acceso global a ella. Este patrón tiene como objetivo controlar la creación de objetos y proporcionar una instancia única y compartida de una clase en toda la aplicación. El término Singleton describe algo que tiene una presencia única en el programa. Se utiliza cuando se desea obtener un único objeto en lugar de muchos diferentes por diversas razones. Ejemplos de esto podrían ser objetos de conexión a bases de datos o servicios de registro de eventos (*logging*) que requieren un único punto de acceso para todos los componentes de una aplicación.

Por ejemplo, es posible que desees mantener solo una instancia de una clase en particular si su creación es costosa o si no tiene sentido mantener más de una durante la vida útil del programa. Los casos de uso comunes para el patrón Singleton incluyen la administración de un grupo de conexiones a bases de datos, la gestión de un servicio de registro de eventos o la gestión de un objeto de configuración.

> [!NOTE]
> Cuando mencionamos un programa, nos referimos al entorno de ejecución actual, que en la mayoría de los casos consiste en un solo proceso que tiene acceso a toda la memoria del programa. Debido al sistema operativo (SO) y otras consideraciones, cuando inicias otro proceso, este creará sus propias instancias Singleton.

#### Características clave

He aquí las características clave del patrón Singleton:

- **Punto de acceso global:** Cuando tienes un Singleton, esencialmente tienes un único punto de acceso a su instancia. Por eso a menudo se hace referencia a un Singleton como una instancia global.
- **Almacenamiento en caché de la instancia:** La instancia del objeto Singleton se almacena en caché en algún lugar para que puedas recuperarla a petición. Por lo general, se almacena dentro de la propia instancia de la clase como una variable estática, pero también se puede almacenar dentro de un contenedor de Inversión de Control (IoC).
- **Inicialización perezosa (*Lazy initialization*):** La instancia no se crea en el momento de la declaración. En su lugar, se crea de forma diferida, ante la primera demanda, evitando inicializaciones costosas al iniciar las aplicaciones.
- **Instancia única por clase:** La instancia es única por clase, lo que significa que diferentes clases tienen sus propios Singletons.

La característica clave de este patrón es que se puede utilizar en una variedad de escenarios. A continuación, explicaremos los usos prácticos comunes del patrón Singleton.

#### Cuándo usar el patrón Singleton

El patrón Singleton se utiliza habitualmente para gestionar recursos que deben compartirse entre múltiples partes de una aplicación, garantizando al mismo tiempo que solo exista una única instancia de ese recurso. Es particularmente útil en los siguientes escenarios:

- **Gestión del estado global o la configuración:** Cuando tienes un objeto de configuración o un estado al que se debe acceder globalmente en toda la aplicación, el patrón Singleton puede proporcionar un único punto de acceso y garantizar la coherencia.
- **Control del acceso a recursos compartidos:** El patrón Singleton se utiliza a menudo para controlar el acceso a recursos externos como conexiones a bases de datos, sistemas de archivos o endpoints de API. Ayuda a evitar condiciones de carrera, fugas de recursos y problemas de integridad que podrían surgir de múltiples objetos no coordinados que acceden al mismo recurso.
- **Proporcionar una capa de almacenamiento en caché:** Los Singletons se pueden utilizar para implementar un mecanismo de almacenamiento en caché, donde una sola instancia gestiona la lógica de caché y proporciona un punto de acceso centralizado para los datos almacenados en caché.
- **Registro de eventos y manejo de errores:** Se puede utilizar una instancia Singleton para gestionar un sistema global de registro de eventos o de manejo de errores, asegurando que las entradas del registro o los mensajes de error se escriban en una sola ubicación o sean manejados por un solo componente de manera consistente.
- **Grupos de subprocesos o grupos de objetos (*Thread pools* u *Object pools*):** Al tratar con objetos que consumen muchos recursos, se puede utilizar un Singleton para crear y gestionar un grupo de objetos reutilizables, mejorando el rendimiento y reduciendo la sobrecarga de memoria.

Sin embargo, es importante tener en cuenta que el patrón Singleton debe usarse con prudencia y solo cuando sea necesario. El uso excesivo o incorrecto del patrón puede provocar un acoplamiento estrecho, dificultad en las pruebas y posibles problemas de seguridad en entornos multiproceso (*thread-safety*).

En situaciones donde realmente no se requiere una única instancia o se prefiere un bajo acoplamiento, se deben considerar alternativas modernas como la inyección de dependencias y los contenedores de IoC en lugar del patrón Singleton.

> [!NOTE]
> El bajo acoplamiento (*loose coupling*) es un concepto en el diseño de software que se refiere a la práctica de mantener los componentes o módulos lo más independientes y desacoplados posible entre sí. En un sistema débilmente acoplado, los componentes tienen un conocimiento mínimo sobre los detalles de implementación de otros componentes con los que interactúan. En su lugar, confían en interfaces o abstracciones bien definidas para comunicarse e interactuar entre sí.

El patrón Singleton es uno de los primeros patrones que puedes encontrar en casi cualquier lugar. Es simple y prevalente. Veamos brevemente su diagrama de clases antes de ver algunos ejemplos de código.

#### Diagrama de clases UML

El diagrama de clases UML para el patrón Singleton es sencillo. Comunica que una clase es un Singleton cuando contiene al menos los siguientes elementos:

- Una variable estática privada (por ejemplo, `instance`) para contener la instancia única de la clase.
- Un constructor privado para evitar la instanciación directa desde fuera de la clase.
- Un método estático público (por ejemplo, `getInstance()`) que devuelve la instancia única de la clase.

La *Figura 3.2: Diagrama de clases de Singleton* muestra estos elementos:
- La variable de instancia estática privada se utiliza para almacenar en caché la instancia única de la clase Singleton.
- El constructor privado garantiza que las instancias no se puedan crear directamente desde fuera de la clase, haciendo cumplir la regla de instancia única.
- El método estático público `getInstance()` es responsable de crear o devolver la instancia en caché.

Si bien el diagrama de clases muestra la estructura de componentes de un Singleton, es posible que debas inspeccionar la parte de implementación para comprender mejor lo que se necesita hacer para utilizar este patrón.

En la siguiente sección, exploraremos los detalles de implementación del patrón Singleton y explicaremos la importancia de estos elementos.

#### Implementación clásica

La implementación clásica del patrón Singleton sigue algunos pasos generales. Comenzamos con la declaración de una clase base:

```typescript
class Singleton {}
```

Luego, debes implementar los siguientes pasos:

1. **Constructor privado:** Primero, debes evitar que se construyan nuevas instancias haciendo que el constructor sea privado:
   ```typescript
   class Singleton {
       // Prevents creation of new instances
       private constructor() {}
   }
   ```
   Hacer que el constructor sea privado garantiza que la clase Singleton no se pueda instanciar desde fuera de la clase, evitando que se creen accidentalmente múltiples instancias.

2. **Instancia en caché:** A continuación, deseas almacenar en caché la instancia global del Singleton. Puedes usar una variable estática para esto, ya que el entorno de ejecución asegurará que solo se reserve una instancia por clase:
   ```typescript
   class Singleton {
       // Stores the singleton instance
       private static instance: Singleton;
       // Prevents creation of new instances
       private constructor() {}
   }
   ```
   La instancia en caché está reservada para una sola clase y es privada para evitar que se recupere o modifique fuera de la clase.

3. **Punto de acceso único:** Por último, deseas un punto de acceso único para recuperar la instancia en caché del Singleton de esta clase. Puedes usar un método estático para esto:

`singleton.ts`:
```typescript
class Singleton { 
    // Stores the singleton instance 
    private static instance: Singleton; 
    // Prevents creation of new instances 
    private constructor() {} 

    // Method to retrieve instance 
    static getInstance(): Singleton { 
        if (!Singleton.instance) { 
            Singleton.instance = new Singleton(); 
        } 
        return Singleton.instance; 
    } 
}
```

Observa que creamos la instancia de forma perezosa (*lazy*), y no cuando la clase se descubre en tiempo de ejecución. Aunque la instanciación diferida nos ayuda a evitar un uso innecesario de memoria y efectos secundarios durante el arranque, puede retrasar la disponibilidad de recursos críticos hasta que se soliciten por primera vez. Esto puede generar cuellos de botella en el rendimiento si el proceso de inicialización requiere muchos recursos o si el recurso se necesita inmediatamente al iniciar la aplicación.

He aquí un ejemplo de una clase Singleton con un método real:

`singleton.ts`:
```typescript
class UserService { 
    private static instance: UserService 
    private constructor() {} 

    static getInstance(): UserService { 
        if (!UserService.instance) { 
            UserService.instance = new UserService() 
        } 
        return UserService.instance 
    } 

    getUsers(): string[] { 
        return ["Alex", "John", "Sarah"] 
    } 
} 

// Usage 
const userService = UserService.getInstance() 
const users = userService.getUsers() 
console.log(users) // Output: ['Alex', 'John', 'Sarah']
```

En este ejemplo, la clase `UserService` es un Singleton que proporciona un método `getUsers` para recuperar una lista de usuarios. El método `getInstance` garantiza que solo exista una instancia de `UserService` en toda la aplicación y que se pueda acceder al método `getUsers` a través de esta única instancia.

Si bien la implementación clásica del patrón Singleton es sencilla, puede ser engorroso copiar y pegar el código estándar para cada clase a la que desees aplicar este patrón. Idealmente, deberías recurrir a utilizar el patrón Singleton solo cuando necesites controlar una única instancia de un objeto por aplicación.

Al final de esta sección, explicaremos más deficiencias de este patrón y discutiremos alternativas modernas, especialmente con respecto a las características del lenguaje TypeScript. Por ahora, exploremos algunas variaciones modernas alternativas de este patrón que son más adecuadas para los modismos de TypeScript.

#### Implementaciones modernas

La implementación clásica del patrón de diseño Singleton sigue un conjunto específico de pasos; sin embargo, esta no es la única forma de crear objetos en TypeScript. Además, puedes aprovechar algunas características del lenguaje y del entorno para obtener el comportamiento de Singleton de forma gratuita. Exploremos juntos algunas implementaciones y variaciones alternativas.

##### Uso de Singletons por resolución de módulos

En lugar de crear tu propia implementación de Singleton y hacer que la clase almacene en caché esta instancia, puedes aprovechar el mecanismo de carga del sistema de módulos. Node.js almacena en caché los módulos después de que se cargan por primera vez, lo que significa que las llamadas posteriores a `require()` o `import` para el mismo módulo devolverán la instancia almacenada en caché en lugar de volver a ejecutar el código del módulo. Este comportamiento de almacenamiento en caché se gestiona a través del objeto `require.cache`, que almacena referencias a módulos cargados en función de sus nombres de archivo resueltos.

En este ejemplo, simplemente creas una clase:

```typescript
class ApiServiceSingleton {}
```

Luego, exportas una variable de instancia predeterminada:

```typescript
export default new ApiServiceSingleton();
```

Esto aprovecha el sistema de módulos de Node.js para exportar una variable predeterminada que apunta a una instancia de `ApiServiceSingleton`. Este patrón se utiliza a menudo porque es fácil de implementar. Con esta exportación predeterminada implementada, siempre que importes el objeto predeterminado, apuntará a la misma instancia:

```typescript
import apiService from "./ApiServiceSingleton"; // apiService instance here is the same as exported
```

Básicamente, este enfoque delega el control del Singleton al sistema de módulos. No tendrás la oportunidad de cambiar esta instancia a menos que simules (*mock*) todo el módulo.

Además, debes comprender las peculiaridades del sistema de módulos de Node.js, ya que almacena en caché los módulos según la ruta absoluta requerida de este módulo. Si importamos este archivo y se resuelve en la misma ruta absoluta, el sistema de módulos utilizará la misma instancia en caché. Este podría no ser el caso si tu código reside en `node_modules` como una dependencia con una versión conflictiva:

```typescript
// Importing with the first absolute path 
import apiService1 from "/users/theo/projects/typescript-4-design-patterns/chapters/chapter-3/ModuleSingleton.ts"; 
console.log(apiService1.getData()); // Output: API data 

// Importing with a second absolute path (different node_modules) 
import apiService2 from "/users/theo/projects/typescript-4-design-patterns/node_modules/SomeLibrary/node_modules/singleton/ModuleSingleton.ts"; 
console.log(apiService2.getData()); // Output: API data
```

En este ejemplo, ambas importaciones apuntan a una instancia diferente ya que las rutas resuelven una ubicación diferente. Esta distinción subraya la importancia de comprender el comportamiento de almacenamiento en caché del sistema de módulos de Node.js y cómo influye en las implementaciones de Singleton.

##### Uso de decoradores para la implementación de Singleton

Los decoradores de TypeScript ofrecen una forma potente de modificar el comportamiento de las clases y sus miembros. Podemos usar un decorador para hacer cumplir el patrón Singleton. He aquí un ejemplo:

`decorator-singleton.ts`:
```typescript
function Singleton<T extends { new (...args: any[]): {} }>(constructor: T) { 
    return class extends constructor { 
        private static _instance: T | null = null; 
        constructor(...args: any[]) { 
            super(...args) 
            if (!(<any>this.constructor)._instance) { 
                ;(<any>this.constructor)._instance = this 
            } 
            return (<any>this.constructor)._instance 
        } 
    } as unknown as T & { _instance: T } 
} 

@Singleton 
class DecoratedSingleton { 
    constructor() { 
        console.log("DecoratedSingleton instance created") 
    }
}
```

En este caso, la función decoradora `Singleton` acepta una función constructora (`T`) como su parámetro. Devuelve una nueva clase que extiende el constructor original (`T`). Dentro de esta clase extendida, se define una propiedad estática privada llamada `_instance` para contener la instancia única de la clase o `null` inicialmente.

Cuando se instancia una clase decorada con `new DecoratedSingleton()`, el constructor sobrescrito comprueba si `_instance` es `null`. Si lo es, el constructor asigna la instancia actual (`this`) a `_instance`, asegurando que las llamadas posteriores a `new DecoratedSingleton()` devuelvan la misma instancia. El uso de decoradores facilita y simplifica la asignación de este comportamiento a los objetos.

##### Singleton paramétrico

Una limitación del patrón Singleton clásico es que no puedes pasar parámetros de inicialización cuando instancias el objeto por primera vez. Esto se debe a que permitir diferentes parámetros crearía diferentes objetos, lo que contradice el principio de Singleton.

Una solución a esto es el patrón Singleton paramétrico, donde en lugar de mantener una sola instancia, mantienes múltiples instancias almacenadas en caché por una clave. Generas una clave única basada en los parámetros proporcionados en el método `getInstance`. Al pasar dos parámetros diferentes, debería devolver un objeto diferente; pasar el mismo dará como resultado que se devuelva el mismo objeto:

`parametric-singleton.ts`:
```typescript
export class ParametricSingleton { 
    private static instances: Map<string, ParametricSingleton> = new Map() 
    private constructor(private param: string) { 
        this.param = param; 
    } 

    public getParam(): string { 
        return this.param; 
    } 

    static getInstance(param: string): ParametricSingleton { 
        if (!ParametricSingleton.instances.has(param)) { 
            ParametricSingleton.instances.set(param, new ParametricSingleton(param)) 
        } 
        return ParametricSingleton.instances.get(param) as ParametricSingleton 
    } 
} 

const singletonA = ParametricSingleton.getInstance('/v1/users'); 
console.log(singletonA.getParam()); // Output: /v1/users 

const singletonB = ParametricSingleton.getInstance('/v2/users'); 
console.log(singletonB.getParam()); // Output: /v2/users
```

En este ejemplo, `singletonA` y `singletonB` son diferentes instancias de la clase `ParametricSingleton` porque se crearon llamando al método `getInstance()` con diferentes valores de parámetros.

La solución anterior funciona eficazmente con unos pocos parámetros básicos, pero tendrás que crear tu propio esquema para crear claves únicas que correspondan a cada objeto Singleton. Lo importante es mantenerlo simple y tener una forma consistente de definir Singletons para permitir este enfoque flexible.

A continuación, exploraremos cómo probar objetos Singleton.

#### Pruebas (*Testing*)

Cuando escribes una implementación del patrón Singleton, debes asegurarte de que se comporte según lo previsto. Querrás escribir pruebas unitarias que capturen el comportamiento esperado y se ejecuten cada vez que ejecutes el conjunto de pruebas. De esta manera, puedes asegurarte de que si cambias la implementación en el futuro, las pruebas verificarán que nada haya cambiado inesperadamente.

En el caso de la implementación clásica de Singleton, verificar las suposiciones es relativamente simple. Deseas comprobar si dos invocaciones del método `getInstance` devuelven la misma instancia de objeto. He aquí una prueba de ejemplo que utiliza Vitest:

> [!IMPORTANT]
> Este ejemplo utiliza Vitest, un framework de pruebas moderno diseñado para funcionar sin problemas con proyectos de Vite. Vite es una herramienta de compilación de frontend moderna y rápida que ofrece una mejor personalización y rendimiento.

`singleton.test.ts`:
```typescript
import { Singleton } from './singleton.js'; 
import { test, expect, describe } from 'vitest' 

describe('Singleton', () => { 
    test('getInstance returns the same instance', () => { 
        const instance1 = Singleton.getInstance(); 
        const instance2 = Singleton.getInstance(); 
        expect(instance1).toBe(instance2); 
    }); 
});
```

En esta prueba, importamos la clase `Singleton` y usamos la función `test` de Vitest para definir un caso de prueba. Llamamos al método `getInstance` dos veces y almacenamos las instancias devueltas en `instance1` e `instance2`. Luego, usamos la función `expect` de Vitest para afirmar que ambas instancias son estrictamente iguales (`===`).

Para ejecutar los casos de prueba para el patrón Singleton, puedes ejecutar el siguiente script npm en la consola:

```bash
$ npm run test -- Singleton
```

Este comando ejecutará todos los casos de prueba que incluyan la palabra `Singleton` en su descripción o nombre de archivo. En la mayoría de los casos, contar con la prueba inicial, que verifica que se devuelve la misma instancia, es el requisito mínimo. Sin embargo, a medida que tu implementación se vuelve más compleja, es posible que debas escribir pruebas adicionales para cubrir casos límite, administración de estado y otras funciones proporcionadas por tu clase Singleton.

Continuemos esta sección considerando algunas de las críticas a este patrón.

#### Críticas

En esta sección, discutiremos las críticas al patrón Singleton, que a menudo resaltan su potencial para convertirse en un antipatrón cuando se usa incorrectamente:

- **Contaminación por instancia global:** Se hacen muchas críticas porque los Singletons se utilizan como variables globales, y muchos desarrolladores los descartan por una buena razón. Son problemáticos de probar o simular (*mock*), y el uso de variables globales significa ignorar cualquier flexibilidad que puedas obtener de las interfaces u otras abstracciones. Esto es del todo válido, por lo que si decides utilizar Singletons, deben tratarse como objetos estáticos globales que realizan un trabajo muy específico y estrechamente interrelacionado. He aquí un ejemplo:

```typescript
// Import singleton from a package
import { Singleton } from './Singleton'; 

const instance1 = Singleton.getInstance(); 
instance1.addData('item1'); 

const instance2 = Singleton.getInstance(); 
console.log(instance2.data); // Output: ['item1']
```

En este ejemplo, la clase `Singleton` actúa como una instancia global, lo que dificulta su simulación o prueba de forma aislada. Cualquier cambio en la propiedad `data` o en el método `addData` afectará a toda la aplicación, lo que provocará comportamientos inesperados. `instance2` solo ve los datos modificados cuando su contenido puede cambiar inesperadamente.

- **No es fácilmente comprobable:** Más allá de probar los principios de Singleton, si deseas probar el comportamiento de un objeto, deberás superar algunas restricciones. Por ejemplo, supongamos que deseas simular algunos efectos secundarios como llamadas a API; podrías realizar llamadas a API reales durante las pruebas, lo cual no se recomienda. Esto es bastante riesgoso a menos que adoptes un marco de simulación avanzado como Jest o Vitest.
- **Difícil de implementar correctamente:** El patrón Singleton es difícil de implementar, especialmente si buscas capacidad de prueba e inicialización perezosa, y deseas usarlo como una variable global. Debes asegurarte de que la parte de implementación no cause más acoplamiento del que ya existe. Si administra un estado, este estado debe protegerse adecuadamente contra modificaciones concurrentes. Si múltiples partes del programa llaman a un método idéntico del Singleton, siempre deben funcionar según lo esperado.

Teniendo en cuenta estos puntos, se recomienda mantener los Singletons aislados, generalmente en la parte global de la aplicación, con un conjunto de reglas para las pruebas, y utilizarlos de manera adecuada.

Para concluir nuestra exploración del patrón Singleton, profundicemos en algunos ejemplos del mundo real de este patrón en bibliotecas y frameworks destacados.

#### Ejemplos del mundo real

Concluiremos nuestra exploración de Singletons con algunos ejemplos del mundo real en proyectos y bibliotecas populares de TypeScript de código abierto:

- **API del compilador de TypeScript:** La API del compilador de TypeScript, que utilizan varias herramientas e IDEs para integrar la compatibilidad con TypeScript, utiliza el patrón Singleton para los objetos `CompilerHost` y `CompilerOptions`. Estos objetos representan el entorno del host y las opciones del compilador, respectivamente, y están diseñados como Singletons para garantizar un comportamiento coherente en toda la aplicación.
- **Servicios de Angular:** En Angular, ciertos servicios, como `Router` y `HttpClient`, se implementan como Singletons. Esto significa que solo hay una instancia de estos servicios en toda la aplicación, lo que garantiza una gestión de estado coherente y evita la duplicación de recursos.
- **Planificadores de RxJS (*Schedulers*):** RxJS, la popular biblioteca de programación reactiva para JavaScript y TypeScript, utiliza el patrón Singleton para sus planificadores. Los planificadores son responsables de controlar la ejecución de secuencias de observables, y tener una sola instancia de cada tipo de planificador garantiza un comportamiento de programación coherente en toda la aplicación.
- **Módulos de Nest.js:** En el framework Nest.js para crear aplicaciones del lado del servidor con TypeScript, ciertas clases, como la clase `Module`, se implementan como Singletons. Esto garantiza un comportamiento coherente y evita que los recursos se dupliquen en toda la aplicación.

Probablemente encontrarás más ejemplos del mundo real del patrón Singleton, a menudo implementados con ligeras variaciones de lo que hemos explorado. Sin embargo, los conceptos fundamentales siguen siendo coherentes.

A continuación, exploraremos el siguiente patrón más importante: el patrón Prototype.

---

### El patrón Prototype

El siguiente patrón de diseño creacional que estudiaremos es el patrón Prototype. Este patrón ayuda a abstraer el proceso de creación de objetos.

Un prototipo es un tipo de objeto que toma su estado inicial y propiedades de un objeto existente. La idea principal es evitar tener que crear manualmente un objeto y asignarle propiedades a partir de otro objeto.

Utilizando el patrón Prototype, puedes crear objetos que implementen la interfaz `Prototype`. En lugar de crear un nuevo objeto llamando al operador `new`, sigues un camino diferente: construyes objetos que se adhieren a la interfaz `Prototype`, la cual tiene un solo método, `clone()`. Cuando se llama, creará una copia (o clon) de la instancia existente del objeto y sus propiedades internas. De esta manera, puedes evitar duplicar la lógica de crear un nuevo objeto y asignar funciones comunes.

#### Cuándo usar el patrón Prototype

Deberías considerar el uso del patrón Prototype cuando observes los siguientes criterios:

- **Tienes un grupo de objetos y deseas clonarlos en tiempo de ejecución:** Ya has creado algunos objetos y tienes referencias a ellos en tiempo de ejecución, y deseas obtener rápidamente copias idénticas sin volver al método de fábrica ni asignar propiedades nuevamente.
- **Deseas evitar el uso directo del operador `new`:** En este caso, deseas llamar al método `clone` para obtener una copia. Deseas evitar el uso del operador `new`, ya que puede incurrir en una sobrecarga adicional. En su lugar, tienes una forma diferente de crear un objeto y construirlo desde cero en tiempo de ejecución.
- **Deseas crear objetos con estructuras complejas o jerárquicas:** El patrón Prototype puede ser útil cuando necesitas crear objetos con estructuras complejas o jerárquicas, ya que te permite clonar toda la estructura, incluidos los objetos anidados, sin recrearla manualmente.

Revisemos el diagrama de clases UML de este patrón para comprender su estructura.

#### Diagrama de clases UML

Para demostrar el patrón Prototype en UML:
1. Comenzamos con la interfaz `Prototype` (*Figura 3.3: Interfaz Prototype*), que contiene un único método llamado `clone` que devuelve el mismo tipo de interfaz.
2. Luego, creamos clases concretas que implementan esta interfaz (*Figura 3.4: Instancias de Prototype*). Esto es necesario para que podamos llamar al método `clone` bajo demanda.
3. Ahora, los clientes solo usarán y verán las interfaces `Prototype` en lugar de los objetos reales (*Figura 3.5: Uso de Prototype*). La clase `Client` tiene una referencia a un objeto `Prototype` (que podría ser una instancia de `ConcretePrototype1` o `ConcretePrototype2`) y puede llamar al método `clone()` para crear una nueva instancia del mismo tipo.

A continuación, usemos estos diagramas como referencia para implementar el patrón Prototype en TypeScript.

#### Implementación clásica

Siguiendo el diagrama UML, implementemos el patrón Prototype con un ejemplo.

Imagina que estás creando un juego en el que necesitas crear diferentes tipos de animales. Puedes usar el patrón Prototype para clonar objetos de animales existentes y crear nuevas instancias con propiedades similares.

Primero, definamos la interfaz `AnimalPrototype`:

`prototype.ts`:
```typescript
interface AnimalPrototype { 
    clone(): AnimalPrototype 
}
```

A continuación, implementaremos dos clases concretas que representan diferentes tipos de animales, `Dog` y `Cat`, que implementan la interfaz `AnimalPrototype`:

`prototype.ts`:
```typescript
function deepClone<T>(obj: T): T { 
    return JSON.parse(JSON.stringify(obj)); 
} 

class Dog implements AnimalPrototype { 
    constructor( 
        private breed: string, 
        private age: number, 
    ) {} 

    clone(): Dog { 
        return deepClone(this); 
    } 
} 

class Cat implements AnimalPrototype { 
    constructor( 
        private furColor: string, 
        private weight: number, 
    ) {} 

    clone(): Cat { 
        return deepClone(this); 
    } 
}
```

En este ejemplo, los métodos `clone` en cada clase concreta crean una nueva instancia del mismo objeto preservando su estado inicial y propiedades.

Cuando desees clonar estos objetos en tiempo de ejecución, puedes usar la interfaz `AnimalPrototype` y llamar al método `clone`:

`prototype.ts`:
```typescript
let dog: AnimalPrototype = new Dog("Boxer", 3); 
let clonedDog: Dog = dog.clone() as Dog; 
console.log(clonedDog); // Output: Dog { breed: 'Boxer', age: 3 } 

let cat: AnimalPrototype = new Cat("Scott", 4.5); 
let clonedCat: Cat = cat.clone() as Cat; 
console.log(clonedCat); // Output: Cat { furColor: 'Scott', weight: 4.5 }
```

En este ejemplo, creamos instancias de `Dog` y `Cat` y luego las clonamos utilizando el método `clone`. Las instancias clonadas tienen las mismas propiedades que los objetos originales, pero son instancias separadas en la memoria.

A veces, es posible que desees ignorar ciertas propiedades o realizar operaciones adicionales al clonar objetos. En tales casos, puedes modificar la implementación del método `clone` para que maneje esos requisitos específicos.

#### Implementación alternativa

Una implementación más optimizada de este patrón implica el uso de herramientas externas como Lodash para clonar profundamente un objeto en lugar de tener que usar tu propio método `deepClone`. De esta manera, puedes evitar problemas como errores de dependencia circular, que puedes encontrar al usar `JSON.stringify(obj)`.

Cuando clonas un objeto `Dog`, obtienes una copia completamente independiente con todas sus estructuras de datos anidadas correctamente duplicadas, lo que significa que los cambios en el clon no afectarán al objeto original:

`prototype.ts`:
```typescript
import * as clonedeep from 'lodash.clonedeep'; 

export class Dog implements AnimalPrototype { 
    // ... 
    clone(): Dog { 
        return _.cloneDeep(this); 
    } 
}
```

El enfoque de `lodash.cloneDeep` es más robusto que el método `JSON.parse(JSON.stringify())`. Si bien ambos crean copias profundas de objetos, el método JSON tiene varias limitaciones: no puede manejar referencias circulares ni objetos especiales como `Date`, `RegExp`, `Map` y `Set`.

#### Pruebas (*Testing*)

Al probar el patrón Prototype, deseas verificar que llamar al método `clone` devuelva un objeto con el estado y el tipo de instancia correctos. Por lo general, escribirás casos de prueba para asegurarte de que los objetos clonados tengan las propiedades y los valores esperados y que sean instancias separadas de los objetos originales.

He aquí los aspectos clave a probar:
- Creación de un objeto clonado a partir de un prototipo.
- Verificación de que los objetos clonados tengan diferentes referencias en memoria.
- Confirmación de que los objetos clonados se pueden modificar de forma independiente.

Hemos proporcionado casos de prueba en el archivo `prototype.test.ts` ubicado en el código fuente de este capítulo para que los revises. Para ejecutar los casos de prueba, ejecuta el siguiente comando:

```bash
$ npm run test -- prototype
```

#### Críticas

El patrón Prototype se puede utilizar para crear nuevos objetos a partir de instancias ya creadas llamando a su método `clone`. Sin embargo, este enfoque tiene algunas desventajas:

- **Conversión de tipos (*Type casting*):** Cuando confías únicamente en la interfaz `Prototype`, es posible que debas convertir el objeto clonado al tipo de instancia correcto, ya que no tendrás acceso a ningún otro campo o método. Esto puede ser engorroso y propenso a errores, especialmente en bases de código más grandes:

`prototype-issues.ts`:
```typescript
interface Prototype { 
    clone(): Prototype; 
} 

class Person implements Prototype { 
    constructor(public name: string, public age: number) {} 

    clone(): Person { 
        const cloned = Object.create(this); 
        return cloned; 
    } 
} 

// Usage 
const person: Prototype = new Person('John', 30); 
const clonedPerson = person.clone(); 

// Type casting is required to access properties of Person 
const clonedPersonName = (clonedPerson as Person).name; 
console.log(clonedPersonName); // Output: 'John'
```

- **Método `clone` repetitivo:** Crear un método `clone` para cada objeto que implementa la interfaz `Prototype` puede ser repetitivo y tedioso. Si decides proporcionar un método de clonación base y luego usar la herencia para todas las subclases, contradices el propósito del patrón Prototype, que es evitar el uso de la herencia al crear nuevos objetos:

`prototype-issues.ts`:
```typescript
class BasePrototype implements Prototype { 
    clone(): BasePrototype { 
        const cloned = Object.create(this) 
        return cloned 
    } 
} 

class Person2 extends BasePrototype { 
    constructor( 
        public name: string, 
        public age: number, 
    ) { 
        super() 
    } 
} 

class Employee extends BasePrototype { 
    constructor( 
        public name: string, 
        public salary: number, 
    ) { 
        super() 
    } 
}
```

- **Acoplamiento y herencia:** A juzgar por los problemas anteriores, debes asegurarte de evaluar este patrón para casos de uso específicos y ciertos objetos que deseas construir a partir de los existentes. De esta manera, puedes minimizar cualquier acoplamiento introducido por la herencia o la necesidad de conversión de tipos.

#### Ejemplos del mundo real

Existen algunos ejemplos del mundo real de patrones similares a prototipos que siguen mecanismos similares a la implementación clásica:

- **Modelo de herencia prototípica de JavaScript:** Tanto JavaScript como TypeScript utilizan la herencia prototípica entre bastidores. Utiliza prototipos para heredar características de un objeto a otro. Al crear un objeto literal:
  ```typescript
  let x = {};
  ```
  Esto crea un nuevo objeto `x` que hereda del prototipo de `Object`. Eventualmente, llegarás al final de la cadena de prototipos:
  ```typescript
  let o = Object.getPrototypeOf(x); // o is Object.prototype 
  Object.getPrototypeOf(o); // null
  ```
- **`Object.create`:** Otra forma de crear un objeto es mediante el método `Object.create`. Esta técnica te permite especificar de qué objeto prototipo heredar propiedades:
  ```typescript
  let User = { type: 'Unauthenticated', name: 'Theo' }; 
  let u = Object.create(User, {name: {value: 'Alex'}}); 
  console.log(u.name); // 'Alex' 
  console.log(u.type); // 'Unauthenticated'
  ```
- **Función `cloneElement` de React:** Permite clonar un elemento de React existente y pasarle nuevas propiedades (*props*):
  ```typescript
  const baseElement = <button className="btn">Click Me </button>; 

  // Use cloneElement to create a new element with additional props 
  const clonedElement = React.cloneElement(baseElement, { 
      className: 'btn btn-primary', 
      onClick: () => alert('Button clicked!') 
  });
  ```

---

### El patrón Builder

El patrón Builder es un patrón de diseño creacional que facilita la construcción paso a paso de objetos que pueden tener múltiples representaciones. A menudo, creas objetos que requieren más de dos o tres parámetros, muchos de los cuales no se conocen de antemano pero son esenciales para inicializar el objeto con el estado correcto.

#### Cuándo usar el patrón Builder

He aquí los criterios clave para usar el patrón Builder:

- **Un conjunto común de pasos para crear un objeto:** Deseas proporcionar una interfaz con pasos comunes para crear un objeto que no esté vinculado a ninguna implementación. Estos pasos deben ser independientes y siempre deben devolver un objeto utilizable cuando se solicite.
- **Múltiples representaciones:** Puedes tener múltiples representaciones de un objeto, quizás como variantes o como un tipo de subclase. Si no anticipas ni requieres múltiples representaciones en el futuro, este patrón puede parecer innecesario y sobreingenierizado.

Al aplicar este patrón, evalúa el objeto que estás construyendo: ¿Tiene más de tres parámetros? ¿Muchos de estos parámetros son opcionales, con valores predeterminados disponibles si no se proporciona ninguno? ¿Son independientes todos los pasos para crearlo? Si la respuesta a alguna de estas preguntas es no, es posible que aún no necesites el patrón Builder.

#### Diagrama de clases UML

Describamos el patrón Builder utilizando un diagrama de clases UML considerando el ejemplo de la construcción de un coche (*Car*):

1. Primero, tenemos una clase `Car` (*Figura 3.6: Producto Car*) que representa el producto que se está construyendo. Tiene varias propiedades que representan diferentes configuraciones (`engine`, `transmission`, `bodyStyle`, `wheels`).
2. A continuación, necesitamos una interfaz que desglose los pasos de creación de la clase `Car` en un formato reutilizable (*Figura 3.7: Interfaz CarBuilder*).
3. Una vez que tenemos estas dos piezas, necesitaremos una implementación concreta de `CarBuilder` para crear una representación específica de la clase `Car` (*Figura 3.8: El patrón Builder*).

La clase `ConcreteCarBuilder` tiene un método `build()` para crear una nueva instancia de la clase `Car` y métodos para configurar los diversos componentes del objeto `Car` (`setEngine()`, `setTransmission()`, `setBodyStyle()`, `setInterior()` y `setWheels()`). Estos métodos devuelven la interfaz `CarBuilder`, lo que permite el encadenamiento de métodos (*method chaining*).

El libro clásico de la Banda de los Cuatro también incluye el objeto **Director** al describir este patrón. Puedes pensar en este objeto como una abstracción sobre la interfaz `CarBuilder` que consolida esos pasos para producir ciertos productos utilizando un solo método en lugar de encadenar varios.

#### Implementación clásica

La implementación clásica del patrón Builder en TypeScript es la siguiente:

Primero, definimos el tipo Producto y sus componentes:

`builder.ts`:
```typescript
class Engine {} 
class Transmission {}
class Wheels {} 

class Car { 
    constructor( 
        public engine?: Engine, 
        public transmission?: Transmission, 
        public bodyStyle?: BodyStyle, 
        public wheels?: Wheels, 
    ) {} 
}
```

A continuación, definimos la interfaz `CarBuilder`, que especifica los métodos para configurar diferentes componentes de `Car` y el método `build` para crear el objeto `Car` final:

`builder.ts`:
```typescript
interface CarBuilder { 
    setEngine(engine: Engine): CarBuilder; 
    setTransmission(transmission: Transmission): CarBuilder; 
    setBodyStyle(bodyStyle: BodyStyle): CarBuilder; 
    setWheels(wheels: Wheels): CarBuilder; 
    build(): Car; 
}
```

Luego, creamos una clase constructora concreta que implementa la interfaz `CarBuilder`:

`builder.ts`:
```typescript
class ConcreteCarBuilder implements CarBuilder { 
    private car: Car 

    constructor() { 
        this.reset() 
    } 

    reset() { 
        this.car = new Car() 
    } 

    // rest of setter methods here 
}
```

La clase `ConcreteCarBuilder` utiliza una API encadenable, lo que significa que cada método de establecimiento (*setter*) devuelve la propia instancia del constructor:

`builder.ts`:
```typescript
const carBuilder = new ConcreteCarBuilder(); 
const car = carBuilder 
    .setEngine(new Engine()) 
    .setTransmission(new Transmission()) 
    .setInterior(new Interior()) 
    .setWheels(new Wheels()) 
    .build();
```

En esta implementación:
- La clase `Car` representa el producto final que se construye.
- La interfaz `CarBuilder` define los métodos para configurar diferentes componentes y el método `build`.
- La clase `ConcreteCarBuilder` es una implementación concreta con una API encadenable.
- El método `build` devuelve el objeto `Car` completamente configurado y reinicia el constructor a su estado inicial.

#### Implementaciones modernas

Una alternativa moderna aprovecha los genéricos de TypeScript y el encadenamiento de métodos para proporcionar una implementación de constructor más intuitiva y flexible:

`builder.ts`:
```typescript
interface Builder<T> { 
    build(): T; 
} 

class GenericBuilder<T> implements Builder<T> { 
    private obj: Partial<T> = {}; 

    public set<K extends keyof T>(key: K, value: T[K]): GenericBuilder<T> { 
        this.obj[key] = value; 
        return this; 
    } 

    public build(): T { 
        return this.obj as T; 
    } 
}
```

En esta implementación, definimos una interfaz genérica `Builder<T>` y una clase `GenericBuilder<T>` que la implementa. El método `set` permite establecer las propiedades del objeto que se está construyendo de forma segura según el tipo.

#### Pruebas (*Testing*)

Al probar el patrón Builder, es esencial:
- Verificar que los constructores concretos creen los objetos deseados correctamente comprobando sus propiedades.
- Asegurarse de que no haya efectos secundarios al intercalar los pasos de construcción y que el orden de los pasos no genere un objeto no deseado.
- Proporcionar casos de prueba especializados para cada constructor concreto.

Para ejecutar los casos de prueba provistos en `builder.test.ts`:

```bash
$ npm run test -- builder
```

#### Críticas

- **Mayor complejidad:** Separa la lógica de construcción del objeto real, lo que puede aumentar el tamaño del código.
- **Proliferación de clases:** Para crear diferentes representaciones de un objeto, se requieren clases `Builder` concretas separadas para cada variación.
- **Sobrecarga innecesaria:** Para objetos simples, enfoques como constructores con parámetros opcionales u objetos literales pueden ser más directos.
- **Falta de flexibilidad en pasos dinámicos:** Puede carecer de flexibilidad cuando el orden de construcción depende de condiciones complejas en tiempo de ejecución.
- **Posible violación del Principio Abierto/Cerrado (OCP):** Si se requieren nuevos pasos de construcción, las clases `Builder` existentes pueden necesitar modificaciones.

Sugerencias para mitigar estos problemas:
- Utilizar genéricos de TypeScript para crear constructores reutilizables.
- Favorecer la composición sobre la herencia utilizando objetos de configuración.
- Incluir pasos de validación antes y después de cada paso de construcción.

#### Ejemplos del mundo real

Un ejemplo popular es la API encadenable de Lodash a través de su método `_.chain`:

```typescript
const users = [ 
    { 'user': 'alex', 'age': 20 }, 
    { 'user': 'theo', 'age': 40 }, 
    { 'user': 'mike', 'age': 15 } 
]; 

const youngestUser = _.chain(users) 
    .sortBy('age') 
    .head() 
    .value(); // Output: { 'user': 'mike', 'age': 15 }
```

El método `chain` actúa como un constructor, permitiendo encadenar múltiples transformaciones de datos hasta que se llama al método final `value()` para obtener el resultado.

---

### El patrón Factory Method

El patrón Factory Method es un patrón de diseño creacional que proporciona una interfaz para crear objetos en una superclase, permitiendo a las subclases alterar el tipo de objetos que se crearán. Promueve el bajo acoplamiento al eliminar la necesidad de vincular código específico de la aplicación a las clases concretas de objetos que requiere.

El patrón Factory Method consta de varios componentes:
- **Interfaz del Producto (*Product interface*):** Define la interfaz de los objetos que creará el patrón.
- **Productos Concretos (*Concrete products*):** Implementan la interfaz del Producto.
- **Interfaz de la Fábrica (*Factory interface*):** Declara el método o métodos para crear productos.
- **Fábricas Concretas (*Concrete factories*):** Implementan la interfaz de la fábrica y crean productos concretos específicos.

#### Cuándo usar el patrón Factory Method

- **La lógica de creación de objetos es compleja:** Encapsular la lógica de creación en una fábrica mejora la organización y la mantenibilidad.
- **Una clase no puede anticipar los tipos de objetos que necesita crear:** Si los tipos específicos no se conocen hasta el tiempo de ejecución, una fábrica puede gestionar la creación según el contexto.
- **Deseas desacoplar la creación de objetos de su uso:** Separar la lógica de creación promueve el bajo acoplamiento.
- **Necesitas gestionar el ciclo de vida de los objetos:** Las fábricas proporcionan un punto de control centralizado para la creación, inicialización y destrucción de objetos.

#### Diagrama de clases UML

- La interfaz `Vehicle` (*Figura 3.9: Interfaz del Producto de la Fábrica*) define los métodos `startEngine()` y `stopEngine()`.
- Las clases `Car` y `Truck` (*Figura 3.10: Implementaciones del Producto de la Fábrica*) son implementaciones concretas de `Vehicle`.
- La interfaz `VehicleFactory` y las fábricas concretas `CarFactory` y `TruckFactory` (*Figura 3.11: El patrón Factory Method*) manejan la creación de las instancias correspondientes.

#### Implementación clásica

`factory-method.ts`:
```typescript
interface Vehicle { 
    startEngine(): void 
    stopEngine(): void 
} 

class Car implements Vehicle { 
    startEngine(): void { 
        console.log("Starting car engine...") 
    } 
    stopEngine(): void { 
        console.log("Stopping car engine...") 
    } 
} 

class Truck implements Vehicle { 
    startEngine(): void { 
        console.log("Starting truck engine...") 
    } 
    stopEngine(): void { 
        console.log("Stopping truck engine...") 
    } 
} 

interface VehicleFactory { 
    createVehicle(): Vehicle; 
} 

class CarFactory implements VehicleFactory { 
    createVehicle(): Vehicle { 
        return new Car(); 
    } 
} 

class TruckFactory implements VehicleFactory { 
    createVehicle(): Vehicle { 
        return new Truck(); 
    } 
}
```

Uso con múltiples fábricas:

`factory-method.ts`:
```typescript
const carFactory = new CarFactory(); 
const truckFactory = new TruckFactory(); 
const factories: VehicleFactory[] = [carFactory, truckFactory, carFactory]; 

factories.forEach((factory: VehicleFactory) => { 
    const vehicle = factory.createVehicle(); 
    vehicle.startEngine(); 
    vehicle.stopEngine(); 
}); 
// Output: 
// Starting car engine... 
// Starting truck engine... 
// Starting car engine...
```

#### Implementación alternativa

En lugar de crear fábricas separadas para cada producto, puedes usar un parámetro de tipo y una sentencia `switch`:

`factory-method.ts`:
```typescript
enum VehicleType { 
    CAR, 
    TRUCK, 
} 

class VehicleCreator { 
    create(vehicleType: VehicleType): Vehicle { 
        switch (vehicleType) { 
            case VehicleType.CAR: 
                return new Car() 
            case VehicleType.TRUCK: 
                return new Truck() 
            default: 
                throw new Error("Invalid vehicle type") 
        } 
    } 
}
```

A medida que la aplicación crece, se recomienda refactorizar hacia interfaces y clases de fábrica separadas para no tener que modificar constantemente el enum y la sentencia `switch`.

#### Pruebas (*Testing*)

Para verificar que el método de creación genera los tipos de producto correctos utilizando `toBeInstanceOf` en Vitest:

```bash
$ npm run test -- factory-method
```

#### Críticas

- **Más código repetitivo (*boilerplate*):** Crear clases de fábrica individuales puede inflar la base de código.
- **Uso indebido:** Aplicar el patrón indiscriminadamente a objetos simples puede generar sobreingeniería.
- **Mayor complejidad estructural:** Se requiere navegar por múltiples clases para entender la instanciación.

#### Ejemplos del mundo real

- **API del DOM:** Métodos como `document.createElement`, `document.createTextNode` y `document.createEvent`.
- **Librerías de UI:** `React.createElement`, componentes dinámicos en Vue y el `ComponentFactory` de Angular.
- **Desarrollo de videojuegos:** Creación de entidades como enemigos, potenciadores (*power-ups*) y elementos de escenario.

---

### El patrón Abstract Factory

El patrón Abstract Factory es un patrón de diseño creacional que proporciona una interfaz para crear familias de objetos relacionados o dependientes sin especificar sus clases concretas. Actúa como una fábrica de fábricas, permitiendo crear una abstracción común para crear objetos a partir de diferentes fábricas.

El código cliente interactúa únicamente con interfaces de fábricas abstractas, lo que desacopla completamente el cliente de las clases concretas e implementaciones específicas de los productos.

#### Cuándo usar el patrón Abstract Factory

- **Necesidad de familias de objetos relacionados:** Cuando tienes requisitos para crear múltiples objetos interrelacionados (por ejemplo, botones, menús y barras de desplazamiento para diferentes sistemas operativos o temas).
- **Los clientes interactúan con fábricas, no con clases concretas:** Promueve el bajo acoplamiento y permite intercambiar familias completas de productos en tiempo de ejecución sin modificar el código del cliente.
- **Abstracción sobre el proceso de creación:** El cliente solo necesita conocer las operaciones disponibles en la interfaz de la Fábrica Abstracta.
- **Intercambio de fábricas en tiempo de ejecución:** Permite cambiar la fábrica concreta activa dinámicamente según la configuración o el entorno.
- **Familias de objetos coherentes:** Garantiza que los objetos creados por la misma fábrica concreta sean compatibles entre sí.

#### Diagrama de clases UML

1. La interfaz `AbstractFactory` (*Figura 3.12: La interfaz AbstractFactory*) declara métodos para crear diferentes tipos de productos (`ProductA` y `ProductB`).
2. Las interfaces de productos (*Figura 3.13: La interfaz Product*) definen las operaciones comunes de cada tipo de producto.
3. Las fábricas concretas como `ConcreteFactoryA` (*Figura 3.14: Implementación de AbstractFactory*) implementan la creación de los productos concretos correspondientes (`ConcreteProductA`, `ConcreteProductB`).

#### Implementación clásica

`abstract-factory.ts`:
```typescript
interface VehicleFactory { 
    createCar(): Car 
    createMotorcycle(): Motorcycle 
} 

interface Car { 
    drive(): void 
} 

interface Motorcycle { 
    ride(): void 
} 

class CompanyAFactory implements VehicleFactory { 
    createCar(): Car { 
        return new CompanyACar() 
    } 
    createMotorcycle(): Motorcycle { 
        return new CompanyAMotorcycle() 
    } 
} 

class CompanyACar implements Car { 
    drive(): void { 
        console.log("Driving a Company A car") 
    } 
} 

class CompanyAMotorcycle implements Motorcycle { 
    ride(): void { 
        console.log("Riding a Company A motorcycle") 
    } 
} 

// rest of Abstract Factory implementations here.
```

Uso por parte del cliente:

`abstract-factory.ts`:
```typescript
function produceVehicles(factory: VehicleFactory) { 
    const car = factory.createCar() 
    const motorcycle = factory.createMotorcycle() 
    car.drive() 
    motorcycle.ride() 
} 

produceVehicles(new CompanyAFactory()) 
// Output: 
// Driving a Company A car 
// Riding a Company A motorcycle 

produceVehicles(new CompanyBFactory()) 
// Output: 
// Driving a Company B car 
// Riding a Company B motorcycle
```

#### Pruebas (*Testing*)

Para probar que las implementaciones de la fábrica abstracta producen objetos de los tipos correctos:

```bash
$ npm run test -- abstract-factory
```

#### Críticas

- **Mayor complejidad y trabajo inicial:** Introduce múltiples interfaces y clases abstractas que pueden dificultar la navegación en sistemas pequeños.
- **Abstracción prematura:** Riesgo de abstraer antes de comprender completamente la jerarquía y evolución de las familias de objetos.
- **Sobrecarga de refactorización:** Modificar la interfaz de la fábrica abstracta para agregar un nuevo tipo de producto requiere cambiar todas las fábricas concretas existentes.

#### Ejemplos del mundo real

- **Adaptadores de Nest.js:** El módulo `AbstractWsAdapter` en Nest.js sirve como una Fábrica Abstracta para crear diferentes tipos de adaptadores WebSocket (`IoAdapter`, `SocketIoAdapter`).
- **Inversify.js:** El contenedor de Inversify utiliza abstracciones de fábrica para resolver e inyectar dependencias:

```typescript
import 'reflect-metadata'; 
import { container } from './inversify.config'; 
import { Warrior } from './interfaces'; 

const warrior = container.get<Warrior>('Warrior'); 
console.log(warrior.fight()); // Outputs: Ninja fight! 
console.log(warrior.sneak()); // Outputs: Ninja sneak!
```

---

### Resumen

En este capítulo, comenzamos descubriendo los detalles del patrón **Singleton** y cómo nos ayuda a controlar instancias únicas de objetos. A continuación, aprendimos cómo el patrón **Prototype** nos permite especificar qué tipos de objetos deseamos crear y clonarlos utilizando esas instancias como base. Luego, aprendimos cómo el patrón **Builder** nos permite construir objetos complejos paso a paso. Por último, aprendimos que al utilizar los patrones **Factory Method** y **Abstract Factory**, podemos separar el proceso de creación de objetos de su representación y describir fábricas de familias de objetos.

En el próximo capítulo, continuarás aprendiendo más sobre los patrones de diseño estructurales, que facilitan el diseño al identificar formas sencillas de materializar relaciones entre entidades.

---

### Preguntas y respuestas

1. **¿Cuál es el propósito principal del patrón Singleton y cuándo se debe utilizar?**
   - **Respuesta:** El patrón Singleton se utiliza cuando se desea tener una única instancia de una clase para todo el programa. Garantiza que siempre que un cliente utilice un Singleton, será la misma instancia que cualquier otra referencia en el programa.

2. **Explica la diferencia entre el patrón Factory Method y el patrón Abstract Factory.**
   - **Respuesta:** El patrón Factory Method se ocupa de crear objetos de un solo tipo, mientras que el patrón Abstract Factory se ocupa de crear familias de objetos relacionados. Por lo tanto, el patrón Factory Method es una especialización del patrón Abstract Factory.

3. **¿Cuándo usarías el patrón Builder y cuáles son sus ventajas?**
   - **Respuesta:** Debes usar el patrón Builder cuando necesites crear objetos complejos que tengan muchos parámetros opcionales o cuando el proceso de construcción implique múltiples pasos. Sus ventajas incluyen una mejor legibilidad del código, flexibilidad en la construcción de objetos y la capacidad de reutilizar el mismo código de construcción para diferentes representaciones del objeto.

4. **Explica la intención y el caso de uso del patrón Prototype.**
   - **Respuesta:** El patrón Prototype debe usarse cuando crear una instancia de una clase es costoso o complicado, y deseas clonar una instancia existente en lugar de crear una nueva desde cero. Es útil cuando necesitas crear múltiples instancias de un objeto con un estado similar, o cuando el proceso de creación de objetos es complejo y requiere mucho tiempo.

5. **¿Cómo ayuda el patrón Abstract Factory a crear familias de objetos relacionados?**
   - **Respuesta:** El patrón Abstract Factory proporciona una interfaz para crear familias de objetos relacionados o dependientes sin la necesidad de especificar sus clases concretas. Ayuda a crear un sistema de objetos que siguen un tema o patrón específico, facilitando el intercambio de familias completas de objetos por otras.

6. **¿Cuáles son las ventajas de utilizar el patrón Factory Method sobre la instanciación directa de objetos?**
   - **Respuesta:** El uso del patrón Factory Method sobre la instanciación directa proporciona una mejor organización del código, encapsulación y flexibilidad. Permite una creación y extensión de objetos más sencilla, y también nos permite introducir nuevos tipos de objetos sin necesidad de modificar el código existente.

---

### Lecturas complementarias

- El patrón Builder, así como todos los patrones creacionales, se describen en el libro clásico de la Banda de los Cuatro (*Gang of Four*), *Design Patterns: Elements of Reusable Object-Oriented Software*, por Gamma Erich, Helm Richard, Johnson Ralph, Vlissides John, Addison-Wesley. Disponible en: https://archive.org/details/designpatternsel00gamm/page/96/mode/2up
