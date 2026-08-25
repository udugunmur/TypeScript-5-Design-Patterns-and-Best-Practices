# Parte 2: Patrones de diseño fundamentales en TypeScript

## Capítulo 4: Patrones de diseño estructurales

Los patrones de diseño estructurales son herramientas poderosas en el arsenal de un desarrollador que ofrecen soluciones elegantes para organizar objetos y clases en sistemas más grandes y complejos. Estos patrones se centran en cómo se componen los objetos para formar estructuras más grandes, manteniendo estas estructuras flexibles y eficientes. En TypeScript 5, con su sistema de tipos mejorado y sus características de lenguaje, la implementación de estos patrones se vuelve aún más robusta y segura en cuanto a tipos.

Este capítulo profundiza en los patrones de diseño estructurales, explorando cómo se pueden aprovechar para crear aplicaciones TypeScript más fáciles de mantener, escalables y adaptables. Examinaremos cada patrón en detalle, proporcionando tanto conocimientos teóricos como implementaciones prácticas específicas de TypeScript.

En este capítulo, cubriremos los siguientes temas principales:

- Principios fundamentales de los patrones de diseño estructurales
- El patrón Adapter
- El patrón Decorator
- El patrón Façade
- El patrón Composite
- El patrón Proxy
- El patrón Bridge
- El patrón Flyweight

Al final de este capítulo, tendrás una comprensión integral de los patrones de diseño estructurales y las habilidades para aplicarlos eficazmente en tus proyectos TypeScript. También podrás identificar escenarios donde estos patrones pueden resolver problemas de diseño comunes, lo que conducirá a un código más elegante y mantenible.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en GitHub en:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter04_Structural_Design_Patterns

---

### Comprensión de los patrones de diseño estructurales

Los patrones de diseño estructurales ofrecen un enfoque distinto en comparación con los patrones creacionales. Se centran en organizar objetos y clases en estructuras más grandes mientras mantienen la flexibilidad y la extensibilidad. Estos patrones son particularmente valiosos en proyectos de TypeScript 5, donde el tipado estricto y las características avanzadas del lenguaje pueden mejorar su implementación.

Comprender los patrones de diseño estructurales es importante en el desarrollo en equipo porque proporcionan un marco compartido para organizar el código. Por ejemplo, considera un sistema de registro de eventos (*logging*) donde diferentes desarrolladores implementan el registro de formas incoherentes: algunos pueden registrar en la consola, mientras que otros pueden escribir en archivos o bases de datos. Esta incoherencia puede dar lugar a una base de código desordenada, no escalable y difícil de gestionar y depurar.

Al emplear patrones de diseño estructurales, como los patrones Adapter o Composite, los equipos pueden estandarizar cómo se gestiona el registro de eventos en toda la aplicación. Por ejemplo, usar una clase `Logger` común que se adhiera a una interfaz específica permite a los desarrolladores intercambiar implementaciones (como registrar en diferentes salidas) sin tener que cambiar la lógica central de la aplicación.

#### Características clave y casos de uso

Los patrones estructurales destacan en varios escenarios clave:

- **Composición de objetos en estructuras más grandes:** Esto se vuelve crucial cuando necesitas agregar nueva funcionalidad a objetos existentes sin alterar significativamente su estructura central. Tales situaciones surgen a menudo al adaptarse a requisitos cambiantes o al mejorar la organización del código. Estos patrones permiten una fácil extensión de objetos al tiempo que minimizan la duplicación de código y la sobrecarga, un factor crítico para mantener bases de código limpias y eficientes.
- **Simplificación de relaciones complejas entre objetos:** Estos patrones destacan en la gestión eficiente de las relaciones entre diferentes objetos. En el diseño orientado a objetos, encontramos principalmente dos tipos de relaciones entre objetos:
  - La relación *has-a* (tiene un), donde un objeto contiene una referencia a otro.
  - La relación *is-a* (es un), que implica herencia o relaciones basadas en tipos.
  Los patrones estructurales tienen como objetivo hacer que estas relaciones sean más manejables, extensibles y reemplazables, lo cual es esencial para crear sistemas de software flexibles y mantenibles.
- **Mejora de la flexibilidad y la mantenibilidad del código:** Facilitan una gestión más sencilla de las dependencias entre componentes, lo que es particularmente importante en aplicaciones a gran escala. Al promover el bajo acoplamiento, estos patrones hacen que los sistemas sean más adaptables al cambio. Esta adaptabilidad es crucial en los entornos de desarrollo acelerados de hoy en día, donde los requisitos pueden cambiar rápidamente. Además, los patrones estructurales bien implementados mejoran la legibilidad del código, lo que facilita a los desarrolladores la comprensión y el mantenimiento de la base de código a lo largo del tiempo.

Para todos estos casos, querrás considerar la aplicación de patrones de diseño estructurales para superar problemas específicos relacionados con la estructura y el tipo de relación de tus entidades con el fin de dar cabida a futuros cambios.

Ahora que comprendes los conceptos básicos de los patrones estructurales, podemos comenzar a explorar estos patrones en detalle uno por uno, comenzando con el patrón Adapter.

---

### El patrón Adapter

El patrón Adapter es un potente patrón de diseño estructural que nos permite integrar interfaces incompatibles sin alterar su implementación central. Este patrón es particularmente útil en proyectos TypeScript 5, donde la seguridad de tipos y la coherencia de interfaces son primordiales.

> [!NOTE]
> Al usar el patrón Adapter, es esencial tener precaución con el uso de los tipos `unknown` y `any` en TypeScript al adaptar objetos o servicios. Habilitar el modo estricto de TypeScript ayuda a mitigar estos riesgos al hacer cumplir comprobaciones de tipo más estrictas y garantizar que la conversión de tipos se realice correctamente.

#### Comprensión del patrón Adapter

En su núcleo, el patrón Adapter actúa como un puente entre dos interfaces incompatibles. Envuelve un objeto existente dentro de una nueva estructura o interfaz, lo que permite que sea utilizado por clientes que esperan una interfaz diferente. Este mecanismo de envoltura expande la usabilidad de los objetos a través de diversas interfaces, promoviendo la reutilización y la flexibilidad del código.

Piensa en el patrón Adapter como un adaptador de corriente universal que podrías usar cuando viajas. Así como este dispositivo te permite conectar tus aparatos electrónicos a diferentes tipos de enchufes en todo el mundo, el patrón Adapter permite que tu código funcione con interfaces diversas y aparentemente incompatibles.

#### Cuándo usar el patrón Adapter

El patrón Adapter resulta invaluable en varios escenarios:

- **Resolución de discrepancias de interfaces:** Cuando tienes un cliente que espera una interfaz de tipo A, pero posees un objeto que implementa el tipo B, el patrón Adapter acude al rescate. Es especialmente útil cuando no puedes o no deseas modificar el objeto existente para implementar la interfaz A directamente.
- **Integración de código heredado (*Legacy code*):** En escenarios donde estás trabajando con sistemas heredados o bibliotecas de terceros, el patrón Adapter te permite integrar estos componentes con tu base de código moderna sin modificaciones extensas.
- **Mejora de la interoperabilidad:** El patrón funciona bien para hacer que clases incompatibles trabajen juntas a la perfección. Es particularmente útil cuando deseas usar dos clases juntas a través de sus interfaces, pero son inherentemente incompatibles.
- **Mantenimiento de la seguridad de tipos:** En TypeScript 5, donde la seguridad de tipos es crucial, el patrón Adapter te permite mantener un tipado estricto mientras unes interfaces incompatibles.

Al usar este patrón, obtienes muchos beneficios, ya que puedes hacer que cosas incompatibles funcionen juntas mediante el uso de envoltorios (*wrappers*) sin romper la funcionalidad existente.

#### Diagrama de clases UML

El patrón Adapter es una herramienta poderosa para resolver incompatibilidades de interfaz sin la necesidad de modificar el código existente.

Consideremos un escenario en el que tenemos una clase `Client` que espera trabajar con una interfaz `ApiServiceV1`, pero queremos usar una nueva clase `ApiClientV2` que tiene una interfaz diferente (*Figura 4.1: Incompatibilidad de interfaz del Adapter*). En este diagrama, la clase `Client` usa la interfaz `ApiServiceV1`, que tiene un método `callApiV1()`. La clase `ApiClientV2` tiene un método `callApiV2()`, que es incompatible con `ApiServiceV1`.

Para resolver esta incompatibilidad, introducimos un adaptador (*Figura 4.2: Adapter para ApiClientV2*). El componente clave aquí es `ApiClientV2Adapter`, que implementa `ApiServiceV1` y contiene una referencia a `ApiClientV2`. El método `callApiV1()` del adaptador llamará internamente al método `callApiV2()` de la clase `ApiClientV2`.

La clase `ApiClientV2Adapter` resuelve este problema alineando las interfaces para que la clase `Client` pueda usar un método de `ApiClientV2` sin cambiar su referencia de interfaz.

#### Implementación clásica

Consideremos un escenario simple en el que tenemos un sistema heredado que funciona con metros, pero necesitamos integrarlo con un nuevo sistema que utiliza pies. Usaremos el patrón Adapter para cerrar esta brecha.

Primero, definamos nuestras interfaces y clases:

`adapter.ts`:
```typescript
interface MetricCalculator { 
    getDistanceInMeters(): number; 
} 

class MetricSystem implements MetricCalculator { 
    private distanceInMeters: number; 

    constructor(distanceInMeters: number) { 
        this.distanceInMeters = distanceInMeters; 
    } 

    getDistanceInMeters(): number { 
        return this.distanceInMeters; 
    } 
} 

class ImperialSystem { 
    private distanceInFeet: number; 

    constructor(distanceInFeet: number) { 
        this.distanceInFeet = distanceInFeet; 
    } 

    getDistanceInFeet(): number { 
        return this.distanceInFeet; 
    } 
}
```

La interfaz `MetricCalculator` especifica un método llamado `getDistanceInMeters()`, que se espera que devuelva una distancia en metros. La clase `MetricSystem` implementa esta interfaz, almacenando una distancia en metros y proporcionando el método `getDistanceInMeters()` para devolver este valor. La clase `ImperialSystem`, por otro lado, maneja distancias medidas en pies, almacenando una distancia en pies y proporcionando un método llamado `getDistanceInFeet()` para devolver este valor.

> [!NOTE]
> Es importante tener en cuenta que las interfaces de TypeScript se tipan estructuralmente. Esto significa que un objeto no necesita implementar explícitamente una interfaz, siempre que se ajuste a la estructura esperada. Esta característica puede dar lugar a diferencias sutiles en el comportamiento del patrón Adapter en TypeScript en comparación con otros lenguajes que utilizan tipado nominal. Además, si no se espera que propiedades como `distanceInMeters` y `distanceInFeet` cambien después de la inicialización, aprovechar la característica de propiedad de solo lectura (`readonly`) de TypeScript en `MetricSystem` e `ImperialSystem` puede reforzar la inmutabilidad.

Ahora, crearemos un adaptador para hacer que `ImperialSystem` sea compatible con la interfaz `MetricCalculator`:

`adapter.ts`:
```typescript
class ImperialToMetricAdapter implements MetricCalculator { 
    private imperialSystem: ImperialSystem; 

    constructor(imperialSystem: ImperialSystem) { 
        this.imperialSystem = imperialSystem; 
    } 

    getDistanceInMeters(): number { 
        const feet = this.imperialSystem.getDistanceInFeet() 
        if (typeof feet !== "number" || isNaN(feet)) { 
            throw new Error("Invalid distance in feet provided") 
        } 
        return feet * 0.3048 // Convert feet to meters 
    } 
}
```

El código anterior define una clase adaptadora llamada `ImperialToMetricAdapter` que permite usar un objeto `ImperialSystem` donde se espera `MetricCalculator`. La clase `ImperialToMetricAdapter` implementa la interfaz `MetricCalculator`, asegurando que tenga el método `getDistanceInMeters()`. El método `getDistanceInMeters()` recupera la distancia en pies de la instancia de `ImperialSystem` usando `getDistanceInFeet()`, convierte esta distancia a metros y devuelve el valor convertido.

He aquí cómo un cliente podría usar estas clases:

`adapter.ts`:
```typescript
class Reporter { 
    static reportDistance(calculator: MetricCalculator) { 
        console.log(`The distance is ${calculator.getDistanceInMeters()} meters.`); 
    } 
} 

const metricDistance = new MetricSystem(5); 
Reporter.reportDistance(metricDistance); 

const imperialDistance = new ImperialSystem(10); 
const adapter = new ImperialToMetricAdapter(imperialDistance); 
Reporter.reportDistance(adapter);
```

Cuando ejecutas este código, verás una salida similar a la mostrada en la *Figura 4.3: Salida del ejemplo del patrón Adapter*.

Este ejemplo demuestra cómo el patrón Adapter te permite utilizar interfaces incompatibles juntas. La clase `ImperialToMetricAdapter` hace que `ImperialSystem` parezca implementar la interfaz `MetricCalculator`, permitiendo que se use en lugares donde se espera `MetricCalculator`.

#### Pruebas (*Testing*)

Al implementar el patrón Adapter, es crucial verificar que el adaptador funcione como se espera: que convierta con precisión los pies en metros, que implemente adecuadamente la interfaz `MetricCalculator` y que tenga el método `getDistanceInMeters`.

Hemos proporcionado casos de prueba en el archivo `adaptor.test.ts` ubicado en el código fuente de este capítulo para que los revises. Para ejecutar los casos de prueba, ejecuta el siguiente comando:

```bash
$ npm run test -- adaptor
```

#### Críticas

La crítica principal al patrón Adapter es que introduce capas adicionales de abstracción y código:

- **Carga de mantenimiento:** Cada adaptador agrega otra clase a tu base de código que necesita mantenimiento y actualizaciones. Considera el uso de tipos parciales o mapeados en TypeScript para ayudar a reducir el código repetitivo al construir adaptadores.
- **Desafíos de depuración:** Cuando surgen problemas, los desarrolladores deben rastrear a través del adaptador, lo que puede complicar los procesos de depuración.
- **Posible sobrecarga de rendimiento:** Aunque generalmente es mínima, la capa adicional puede introducir ligeros costos de rendimiento, especialmente si el adaptador realiza transformaciones complejas.
- **Inconsistencias de comportamiento:** Los adaptadores pueden no imitar perfectamente el comportamiento de la interfaz original, lo que genera errores sutiles.
- **Desajustes de versiones:** A medida que evoluciona la clase adaptada, es posible que el adaptador no siga el ritmo, lo que conduce a una funcionalidad desactualizada o incorrecta.

#### Casos de uso del mundo real

Sequelize, un ORM popular para Node.js, utiliza el patrón Adapter para admitir múltiples sistemas de bases de datos. Sequelize proporciona diferentes adaptadores de dialecto para MySQL, PostgreSQL, SQLite y otras bases de datos:

```typescript
import { Sequelize } from 'sequelize'; 

// MySQL adapter 
const mysqlDB = new Sequelize('database', 'username', 'password', { 
    dialect: 'mysql' 
}); 

// PostgreSQL adapter 
const postgresDB = new Sequelize('database', 'username', 'password', { 
    dialect: 'postgres' 
});
```

En el código anterior, la clase `Sequelize` actúa como un adaptador que abstrae los detalles de la base de datos subyacente, permitiendo que la aplicación interactúe con las bases de datos MySQL y PostgreSQL utilizando los mismos métodos y propiedades.

A continuación, examinaremos el siguiente patrón: el patrón Decorator.

---

### El patrón Decorator

El patrón Decorator es un patrón de diseño estructural que mejora la funcionalidad de una clase existente sin que tengamos que modificar la implementación existente. Es una alternativa flexible a la creación de subclases para extender la funcionalidad.

> [!NOTE]
> En TypeScript 5, los decoradores se han convertido en parte del estándar ECMAScript, lo que permite a los desarrolladores decorar propiedades, métodos o clases enteras. Esto difiere de las implementaciones tradicionales de programación orientada a objetos (POO) del patrón, donde los decoradores a menudo se limitan a mejoras a nivel de clase. En TypeScript, los decoradores se pueden aplicar a varios elementos, lo que permite un control más granular sobre cómo se extiende la funcionalidad.

Imagina que tienes una habitación vacía (nuestro objeto base). En lugar de alterar permanentemente la estructura de la habitación (subclases), puedes agregar decoraciones como flores, pinturas o muebles (decoradores) para mejorar su apariencia y funcionalidad. Estas decoraciones se pueden agregar o quitar fácilmente sin tener que cambiar la habitación en sí. Esta es la esencia del patrón Decorator en la POO.

#### Características clave

El patrón Decorator tiene las siguientes características:

- **Extensión del comportamiento en tiempo de ejecución:** Los decoradores te permiten modificar el comportamiento de un objeto en tiempo de ejecución.
- **Composición sobre herencia:** Utilizan la composición de objetos para lograr diseños flexibles en lugar de depender de la herencia.
- **Envoltura recursiva:** Se pueden apilar, permitiendo combinar múltiples comportamientos.
- **Consistencia de interfaz:** Los decoradores normalmente implementan la misma interfaz que el objeto decorado, lo que garantiza una integración perfecta.

#### Cuándo usar el patrón Decorator

Querrás usar el patrón Decorator una vez que hayas identificado los siguientes problemas:

- **Adición dinámica de responsabilidades:** Cuando necesitas agregar responsabilidades a objetos de forma dinámica y transparente, sin afectar a otros objetos.
- **Evitar la explosión de clases:** En situaciones donde el uso de subclases conduciría a la creación de muchas clases que degradan la mantenibilidad de la base de código.
- **Extensión sin modificación:** Cuando deseas extender el comportamiento de una clase sin modificar su código existente (adherirse al Principio Abierto/Cerrado).
- **Aspectos transversales (*Cross-cutting concerns*):** Para implementar aspectos transversales como registro de eventos (*logging*), gestión de transacciones o almacenamiento en caché.
- **Comportamiento condicional:** Cuando necesitas agregar un comportamiento que se puede controlar o configurar en tiempo de ejecución.

#### Diagrama de clases UML

- Primero, tenemos el objeto al que queremos adjuntar el nuevo comportamiento en tiempo de ejecución (*Figura 4.4: Objeto a decorar*). `Component` es la interfaz base para todos los componentes. `ConcreteComponent` implementa la interfaz `Component`.
- Para proporcionar la clase `Decorator`, implementamos la misma interfaz que implementa el objeto y envolvemos la misma llamada de método (*Figura 4.5: La clase Decorator*). `ConcreteDecoratorA` y `ConcreteDecoratorB` extienden la clase `Decorator` y agregan sus propios comportamientos.

#### Implementación clásica

Consideremos un sistema de procesamiento de archivos donde tenemos un lector de archivos básico, pero queremos agregar varias funcionalidades como cifrado y compresión sin tener que modificar la clase original.

Primero, definimos nuestra interfaz de componente base y una implementación concreta:

`decorator.ts`:
```typescript
interface FileReader { 
    read(filePath: string): string 
} 

class SimpleFileReader implements FileReader { 
    read(filePath: string): string { 
        console.log(`Reading file from: ${filePath}`) 
        return `Content of ${filePath}` 
    } 
}
```

Ahora, creemos decoradores que agreguen capacidades de cifrado y compresión:

`decorator.ts`:
```typescript
abstract class FileReaderDecorator implements FileReader { 
    constructor(protected readonly reader: FileReader) {} 
    abstract read(filePath: string): string; 
} 

class EncryptionDecorator extends FileReaderDecorator { 
    // implement FireReaderDecorator methods 
} 

class CompressionDecorator extends FileReaderDecorator { 
    // implement FireReaderDecorator methods 
}
```

Los dos decoradores concretos, `EncryptionDecorator` y `CompressionDecorator`, extienden `FileReaderDecorator` para agregar funcionalidades de cifrado y compresión, respectivamente. Ambos sobrescriben el método `read` para procesar el contenido del archivo llamando al método `read` de la clase base y luego aplicando sus operaciones específicas (cifrar o comprimir) al contenido.

Aquí se muestra cómo podemos usar estos decoradores para crear un lector de archivos con funcionalidades combinadas:

`decorator.ts`:
```typescript
let reader: FileReader = new SimpleFileReader(); 
reader = new CompressionDecorator(reader); 
reader = new EncryptionDecorator(reader); 
const content = reader.read("example.txt"); 
console.log("Final content:", content);
```

La salida se muestra en la *Figura 4.6: Salida de la ejecución del ejemplo del decorador*.

#### Variantes modernas del patrón Decorator

TypeScript 5 ofrece características de lenguaje que facilitan el cambio de comportamiento existente con el uso de decoradores de ECMAScript. En lugar de definir una clase, podemos definir una función especial para decorar clases, métodos, propiedades o parámetros:

`decorator-variant.ts`:
```typescript
function Encrypt() { 
    return function <T extends { new (...args: any[]): FileReader }>(constructor: T) { 
        return class extends constructor { 
            read(filePath: string): string { 
                const content = super.read(filePath) 
                console.log("Encrypting content") 
                return `Encrypted(${content})` 
            } 
        } 
    } 
} 

function Compress() { 
    return function <T extends { new (...args: any[]): FileReader }>(constructor: T) { 
        return class extends constructor { 
            read(filePath: string): string { 
                const content = super.read(filePath) 
                console.log("Compressing content") 
                return `Compressed(${content})` 
            } 
        } 
    } 
} 

interface FileReader { 
    read(filePath: string): string 
} 

@Compress() 
@Encrypt() 
class SimpleFileReader implements FileReader { 
    read(filePath: string): string { 
        console.log(`Reading file from: ${filePath}`) 
        return `Content of ${filePath}` 
    } 
} 

const reader = new SimpleFileReader() 
const content = reader.read("example.txt") 
console.log("Final content:", content)
```

Gracias a esta sintaxis de decoradores, puedes adjuntar un comportamiento común en muchos lugares sin instanciar nuevas clases de decoradores cada vez, haciendo que el código sea más conciso.

#### Pruebas (*Testing*)

Al probar decoradores, debemos centrarnos en:
- **Preservación de la funcionalidad:** Verificar que se conserve la funcionalidad original de la clase o método decorado.
- **Comportamiento del decorador:** Probar que el decorador esté realizando realmente la funcionalidad prevista.
- **Orden de ejecución:** Asegurar que múltiples decoradores se ejecuten en el orden correcto.
- **Efectos secundarios:** Verificar cualquier efecto secundario que puedan tener los decoradores.

Para ejecutar las pruebas en `decorator.test.ts`:

```bash
$ npm run test -- decorator
```

#### Críticas

- **Dependencia de la interfaz:** Si la interfaz del objeto envuelto cambia, todos los decoradores deben actualizarse.
- **Complejidad y legibilidad:** A medida que se agregan más decoradores, el flujo de ejecución puede volverse más difícil de seguir.
- **Sobrecarga de rendimiento:** Cada decorador agrega un nivel de indirección, lo que puede introducir una pequeña sobrecarga de rendimiento en aplicaciones con requisitos de tiempo real críticos.
- **Dependencias de orden:** El orden de los decoradores es importante cuando introducen efectos secundarios.

#### Casos de uso del mundo real

Nest.js utiliza decoradores de forma intensiva:

```typescript
import { Controller, Get } from '@nestjs/common'; 

@Controller('dogs') 
export class DogsController { 
    @Get() 
    findAll() { 
        return 'This action returns all dogs'; 
    } 
}
```

El decorador `@Controller('dogs')` define la ruta base y `@Get()` especifica que el método `findAll` debe manejar las solicitudes GET a la ruta `/dogs`.

A continuación, examinaremos el patrón Façade.

---

### El patrón Façade

El patrón Façade (Fachada) es un patrón de diseño estructural que proporciona una interfaz simplificada para un subsistema complejo de clases, bibliotecas o frameworks. Encapsula un grupo de interfaces en una interfaz de nivel superior, lo que hace que el subsistema sea más fácil de usar.

Su propósito principal es:
- Simplificar las interacciones del cliente con un sistema.
- Desacoplar al cliente de los componentes del subsistema.
- Proporcionar una abstracción simple sobre un sistema complejo para facilitar su uso.

Podemos usar un sistema doméstico inteligente (*smart home*) como analogía: en lugar de interactuar individualmente con luces, termostatos y alarmas, usas una sola aplicación que actúa como fachada para controlarlos a todos con unos pocos toques.

#### Cuándo usar el patrón Façade

- **Simplificación de sistemas complejos:** Cuando necesitas proporcionar una interfaz simple para un subsistema complejo.
- **Creación de abstracciones de subsistemas:** Para definir un punto de entrada para cada nivel de subsistema.
- **Estructuración de sistemas en capas:** Cuando deseas estructurar un sistema en capas.
- **Reducción de dependencias:** Para desacoplar el código del cliente de los componentes del subsistema.

#### Diagrama de clases UML

- En la *Figura 4.7: Subsistemas complejos con los que el cliente tiene que interactuar*, la clase `Client` tiene que interactuar directamente con `SubsystemA`, `SubsystemB` y `SubsystemC`.
- En la *Figura 4.8: El objeto Façade*, se introduce la clase `Facade`, que compone instancias de todos los subsistemas y proporciona operaciones simplificadas que gestionan la complejidad internamente.

#### Implementación clásica

`facade.ts`:
```typescript
interface SubsystemA { 
    operationA1(): void 
    operationA2(): void 
} 

interface SubsystemB { 
    operationB1(): void 
    operationB2(): void 
} 

class ConcreteSubsystemA implements SubsystemA { 
    operationA1(): void { 
        console.log("SubsystemA: Performing operation A1") 
    } 
    operationA2(): void { 
        console.log("SubsystemA: Performing operation A2") 
    } 
} 

class ConcreteSubsystemB implements SubsystemB { 
    operationB1(): void { 
        console.log("SubsystemB: Performing operation B1") 
    } 
    operationB2(): void { 
        console.log("SubsystemB: Performing operation B2") 
    } 
}
```

Ahora definimos la clase `Facade`:

`facade.ts`:
```typescript
class Facade { 
    private subsystemA: SubsystemA 
    private subsystemB: SubsystemB 

    constructor(subsystemA: SubsystemA, subsystemB: SubsystemB) { 
        this.subsystemA = subsystemA 
        this.subsystemB = subsystemB 
    } 

    public simplifiedOperation1(): void { 
        console.log("Facade: Coordinating operations in simplifiedOperation1") 
        this.subsystemA.operationA1() 
        this.subsystemB.operationB1() 
    } 

    public simplifiedOperation2(): void { 
        console.log("Facade: Coordinating operations in simplifiedOperation2") 
        this.subsystemA.operationA2() 
        this.subsystemB.operationB2() 
        this.subsystemA.operationA1() 
    } 
} 

function clientCode(facade: Facade) { 
    console.log("Client: Calling simplifiedOperation1") 
    facade.simplifiedOperation1() 
    console.log("\nClient: Calling simplifiedOperation2") 
    facade.simplifiedOperation2() 
} 

const subsystemA = new ConcreteSubsystemA() 
const subsystemB = new ConcreteSubsystemB() 
const facade = new Facade(subsystemA, subsystemB) 
clientCode(facade)
```

La salida se muestra en la *Figura 4.9: Salida del ejemplo de la clase Facade*.

#### Pruebas (*Testing*)

Al probar el patrón Façade, el objetivo principal es verificar que:
- El patrón Façade orquesta las llamadas a los subsistemas correctamente.
- Maneja adecuadamente cualquier lógica compleja o condiciones.

Para ejecutar los casos de prueba en `facade.test.ts`:

```bash
$ npm run test -- facade
```

#### Críticas

- **Antipatrón de Objeto Dios (*God object*):** Si una fachada administra demasiados servicios e interfaces, puede convertirse en una clase que sabe o hace demasiado.
- **Aumento descontrolado del alcance (*Scope creeping*):** Con el tiempo, los desarrolladores pueden tener la tentación de seguir agregando más y más funcionalidad a la fachada.
- **Disminución de la flexibilidad:** Podría limitar la capacidad del cliente para usar los subsistemas de maneras no previstas por la fachada.
- **Rigidez del código:** Si el sistema subyacente evoluciona significativamente, la fachada puede requerir cambios frecuentes.

Para mitigar estos problemas, crea múltiples fachadas para diferentes casos de uso en lugar de una sola fachada gigante.

#### Casos de uso del mundo real

Coordinación entre servicios de autenticación y de perfil de usuario:

```typescript
class UserManagementFacade { 
    private authService: AuthService; 
    private userProfileService: UserProfileService; 

    constructor() { 
        this.authService = new AuthService(); 
        this.userProfileService = new UserProfileService(); 
    } 

    async login(username: string, password: string): Promise<object> { 
        const token = await this.authService.login(username, password); 
        const userProfile = await this.userProfileService.getUserProfile(username); 
        return { token, userProfile }; 
    } 
    // Other methods... 
}
```

A continuación, examinaremos el patrón Composite.

---

### El patrón Composite

El patrón Composite es un patrón de diseño estructural que te permite componer objetos en estructuras de árbol para representar jerarquías de parte-todo.

La idea central es crear una interfaz común tanto para objetos simples (hojas o *leaves*) como para objetos complejos (compuestos o *composites*) que pueden contener otros objetos. Esta interfaz unificada nos permite tratar ambos tipos de manera coherente.

Una analogía común es un sistema de archivos, donde los directorios (objetos compuestos) pueden contener tanto archivos (objetos hoja) como otros directorios.

#### Cuándo usar el patrón Composite

- Cuando necesitas representar estructuras jerárquicas de objetos.
- Cuando deseas que los clientes ignoren la diferencia entre composiciones de objetos y objetos individuales.
- Cuando tu estructura puede tener cualquier nivel de complejidad y deseas que las operaciones se apliquen de manera uniforme en todos los niveles.

#### Diagrama de clases UML

- La interfaz `FileSystemComponent` (*Figura 4.10: Composite FileSystemComponent*) define operaciones comunes para archivos y directorios.
- Las clases `File` y `Directory` (*Figura 4.11: El patrón Composite*) implementan esta interfaz. `Directory` mantiene una lista de objetos `FileSystemComponent` e implementa métodos para agregar y eliminar componentes, así como para delegar operaciones de forma recursiva.

#### Implementación clásica

`composite.ts`:
```typescript
interface FileSystemComponent { 
    display(): string; 
} 

class File implements FileSystemComponent { 
    private name: string; 

    constructor(name: string) { 
        this.name = name; 
    } 

    display(): string { 
        return `File: ${this.name}`; 
    } 
} 

class Directory implements FileSystemComponent { 
    private name: string; 
    private components: FileSystemComponent[]; 

    constructor(name: string) { 
        this.name = name; 
        this.components = []; 
    } 
    // Rest of methods to add and remove files or directories 
}
```

Ejemplo de uso:

```typescript
// Usage example 
const root = new Directory("Root"); 
const file1 = new File("file1.txt"); 
const file2 = new File("file2.txt"); 
const subDir = new Directory("Subdirectory"); 
const file3 = new File("file3.txt"); 

subDir.add(file3); 
root.add(file1); 
root.add(file2); 
root.add(subDir); 

console.log(root.display());
```

La salida se ilustra en la *Figura 4.12: Salida del patrón Composite*.

#### Pruebas (*Testing*)

Para ejecutar las pruebas del patrón Composite provistas en `composite.test.ts`:

```bash
$ npm run test -- composite
```

#### Críticas

- **Equilibrio entre generalidad y especificidad:** Crear una interfaz lo suficientemente genérica pero significativa es un desafío.
- **Sobrecarga de métodos (*Method bloating*):** Los componentes hoja pueden verse obligados a implementar métodos que no tienen sentido para ellos (como `add` o `remove`), teniendo que lanzar excepciones.
- **Posibles problemas de rendimiento:** Las jerarquías profundas pueden ralentizar el recorrido del árbol.
- **Ciclos accidentales:** Si no se diseña con cuidado, es posible crear ciclos en la jerarquía, lo que provoca bucles infinitos o desbordamientos de pila (*stack overflow*).

#### Casos de uso del mundo real

El DOM utilizado por los navegadores web es un ejemplo clásico del patrón Composite:

```typescript
let body: HTMLBodyElement = document.createElement("body") 
let div: HTMLDivElement = document.createElement("div") 
let p: HTMLParagraphElement = document.createElement("p") 
let text: Text = document.createTextNode("Hello, World!") 

p.appendChild(text) 
div.appendChild(p) 
body.appendChild(div) 

function traverse(node: Node, depth: number = 0): void { 
    console.log(" ".repeat(depth) + node.nodeName) 
    node.childNodes.forEach((child: Node) => { 
        traverse(child, depth + 1) 
    }) 
} 

traverse(body)
```

A continuación, examinaremos el patrón Proxy.

---

### El patrón Proxy

El patrón Proxy es un patrón de diseño estructural que nos proporciona un sustituto o marcador de posición para otro objeto para controlar el acceso a él.

El patrón Proxy se puede utilizar para controlar el acceso a objetos en términos de seguridad y optimizaciones de rendimiento (como la inicialización perezosa o la restricción de acceso a datos confidenciales).

Una analogía es la de una secretaria que atiende llamadas en nombre del director de una empresa, filtrando y regulando quién puede hablar con él.

> [!NOTE]
> Con el patrón Decorator, puedes envolver un objeto con la misma interfaz para agregarle responsabilidades y apilar múltiples decoradores. Con el patrón Proxy, generalmente permites un solo proxy por objeto, utilizándolo para controlar su acceso y delegar sus métodos.

#### Cuándo usar el patrón Proxy

- **Inicialización perezosa (*Virtual Proxy*):** Para retrasar la creación de objetos pesados que consumen muchos recursos hasta que sean realmente necesarios.
- **Control de acceso (*Protection Proxy*):** Para implementar control de acceso basado en roles.
- **Registro y auditoría (*Logging Proxy*):** Para agregar un registro exhaustivo de operaciones sin modificar la lógica empresarial central.
- **Almacenamiento en caché (*Smart Proxy*):** Para almacenar en caché resultados de operaciones costosas.
- **Validación y manejo de errores:** Para validar entradas antes de pasarlas al objeto real o implementar mecanismos de reintento.

#### Diagrama de clases UML

- En la *Figura 4.13: Objeto TextStore básico*, tenemos la interfaz `Store` y la clase `TextStore`.
- En la *Figura 4.14: Objeto Proxy*, la clase `ProxyTextStore` implementa la interfaz `Store` y mantiene una referencia interna a `TextStore`, controlando su instanciación y acceso.

#### Implementación clásica

`proxy.ts`:
```typescript
export interface Store { 
    save(data: string): void 
} 

export class TextStore implements Store { 
    save(data: string): void { 
        console.log(`Called 'save' from TextStore with data=${data}`) 
    } 
}
```

Implementación de `ProxyTextStore`:

`proxy.ts`:
```typescript
export class ProxyTextStore implements Store { 
    constructor(private textStore?: TextStore) {} 

    save(data: string): void { 
        console.log(`Called 'save' from ProxyTextStore with data=${data}`) 
        if (!this.textStore) { 
            console.log("Lazy init: textStore.") 
            this.textStore = new TextStore() 
        } 
        this.textStore.save(data) 
    } 
}
```

Uso por parte del cliente:

`proxy.ts`:
```typescript
// Direct usage 
const directStore = new TextStore(); 
directStore.save("Direct data"); 

// Proxy usage with lazy initialization 
const proxyStore = new ProxyTextStore(); 
proxyStore.save("Proxy data 1"); 
proxyStore.save("Proxy data 2"); 

// Proxy with pre-initialized store 
const preInitStore = new ProxyTextStore(new TextStore()); 
preInitStore.save("Pre-init data");
```

#### Pruebas (*Testing*)

Para probar que el proxy realiza la inicialización perezosa y reenvía las llamadas correctamente:

```bash
$ npm run test -- proxy
```

#### Críticas

- **Sobrecarga de rendimiento:** Cada llamada pasa por un objeto adicional.
- **Complejidad adicional:** Puede dificultar el seguimiento del código cuando hay múltiples proxys anidados.
- **Gestión del ciclo de vida:** Vincular el ciclo de vida del proxy al objeto envuelto puede ocasionar fugas de memoria si no se gestiona adecuadamente.
- **Desafíos en pruebas:** Simular escenarios de carga diferida o recursos remotos puede requerir configuraciones de prueba más complejas.

#### Casos de uso del mundo real

- **Sistema de reactividad de Vue.js:** Utiliza Proxies de JavaScript para rastrear cambios en propiedades de datos y disparar nuevas representaciones visuales (*re-renders*).
- **Librerías ORM:** Carga perezosa de registros de bases de datos relacionados.
- **Carga diferida de imágenes (*Lazy loading*):** Marcadores de posición que cargan la imagen real solo cuando entra en el viewport.
- **Almacenes observables de MobX:** Intercepta mutaciones en estructuras de datos observables.

A continuación, examinaremos el patrón Bridge.

---

### El patrón Bridge

El patrón Bridge es un patrón de diseño estructural que divide una abstracción de su implementación, permitiendo que ambas entidades se extiendan de forma independiente sin estar fuertemente acopladas.

Una analogía es la de un control remoto universal (abstracción) que funciona con múltiples dispositivos electrónicos como televisores o sistemas de sonido (implementaciones). Se pueden agregar nuevos controles remotos o nuevos dispositivos sin afectar a los existentes, siempre que respeten la interfaz común.

#### Cuándo usar el patrón Bridge

- **Separación de conceptos (*Separation of concerns*):** Cuando deseas separar la interfaz de una abstracción de su implementación para poder cambiar la implementación sin afectar a los clientes.
- **Extensibilidad independiente:** Cuando tanto las abstracciones como las implementaciones deben extenderse de forma independiente mediante subclases.
- **Flexibilidad en tiempo de ejecución:** Cuando necesitas poder cambiar de implementación dinámicamente en tiempo de ejecución.
- **Implementación compartida:** Cuando múltiples abstracciones necesitan compartir una implementación sin crear una jerarquía de herencia compleja.

#### Diagrama de clases UML

1. **Paso 1 – Definir las interfaces:** Definimos dos interfaces separadas: una para la abstracción y otra para la implementación (*Figura 4.15: Interfaces de Bridge*).
2. **Paso 2 – Crear clases concretas:** `Abstraction` es una clase abstracta que contiene una referencia a `Implementation`, formando el puente entre ambas jerarquías (*Figura 4.16: Implementaciones de Bridge*).

#### Implementación clásica

`bridge.ts`:
```typescript
interface WateringMechanism { 
    water(amount: number): void 
    checkWaterLevel(): number 
    refill(amount: number): void 
} 

abstract class SmartPlantCare { 
    protected mechanism: WateringMechanism 
    protected moistureThreshold: number 

    constructor(mechanism: WateringMechanism, moistureThreshold: number) { 
        this.mechanism = mechanism 
        this.moistureThreshold = moistureThreshold 
    } 

    abstract waterPlant(currentMoisture: number): void 
    abstract adjustWatering(weatherForecast: string): void 
}
```

Implementaciones concretas:

`bridge.ts`:
```typescript
class MistSprayer implements WateringMechanism { 
    private waterReservoir: number = 500 // ml 

    water(amount: number): void { 
        this.waterReservoir -= amount 
        console.log(`Misting ${amount}ml of water`) 
    } 

    checkWaterLevel(): number { 
        return this.waterReservoir 
    } 

    refill(amount: number): void { 
        this.waterReservoir += amount 
        console.log(`Refilled misting system with ${amount}ml of water`) 
    } 
} 

class TropicalPlantCare extends SmartPlantCare { 
    constructor(mechanism: WateringMechanism) { 
        super(mechanism, 60) // Tropical plants prefer moist soil 
    } 

    waterPlant(currentMoisture: number): void { 
        if (currentMoisture < this.moistureThreshold) { 
            this.mechanism.water(100) 
        } else { 
            console.log("Tropical plant doesn't need watering") 
        } 
    } 

    adjustWatering(weatherForecast: string): void { 
        if (weatherForecast.includes("humidity")) { 
            this.moistureThreshold += 10 
            console.log("Adjusted watering for humid weather") 
        } else if (weatherForecast.includes("dry")) { 
            this.moistureThreshold -= 10 
            console.log("Adjusted watering for dry weather") 
        } 
    } 
}
```

#### Pruebas (*Testing*)

Para ejecutar las pruebas del patrón Bridge en `bridge.test.ts`:

```bash
$ npm run test -- bridge
```

#### Críticas

- **Mayor complejidad:** Introduce capas adicionales de abstracción que pueden no estar justificadas para problemas simples.
- **Inadecuado para una sola implementación:** Si solo se necesita una única implementación de la abstracción, el patrón resulta excesivo (*overkill*).
- **Riesgo de sobreingeniería:** Los desarrolladores pueden verse tentados a usarlo preventivamente.

#### Casos de uso del mundo real

Sistemas de registro de eventos donde `Logger` es la abstracción y `Appender` es la implementación:

```typescript
interface Appender { 
    append(message: string): void; 
} 

abstract class Logger { 
    protected appender: Appender; 

    constructor(appender: Appender) { 
        this.appender = appender; 
    } 

    abstract log(message: string): void; 
} 

class ConsoleAppender implements Appender { 
    append(message: string): void { 
        console.log(`Console: ${message}`); 
    } 
} 

class DebugLogger extends Logger { 
    log(message: string): void { 
        this.appender.append(`Debug Log: ${message}`); 
    } 
}
```

A continuación, examinaremos el último patrón estructural: el patrón Flyweight.

---

### El patrón Flyweight

El patrón Flyweight (Peso Mosca) es un patrón de diseño estructural que optimiza el uso de memoria de objetos pesados compartiendo partes comunes del estado entre varios objetos en lugar de mantener todos los datos en cada objeto individual.

En entornos como el desarrollo de videojuegos, aplicaciones web de alto rendimiento y procesamiento de datos a gran escala, el patrón Flyweight permite reducir significativamente el consumo de memoria.

Una analogía es compartir vestuario entre bailarines en lugar de comprar un traje exclusivo e idéntico para cada uno.

#### Cuándo usar el patrón Flyweight

- Cuando la aplicación necesita generar una cantidad masiva de objetos similares.
- Cuando los costos de almacenamiento en memoria son altos debido a la duplicación de datos.
- Cuando el estado de la mayoría de los objetos se puede extraer y dividir en estados intrínsecos y extrínsecos.

#### Diagrama de clases UML

- La interfaz `Flyweight` (*Figura 4.17: La interfaz Flyweight*) define el método que deben implementar los objetos peso mosca concretos, aceptando el estado extrínseco como parámetro.
- `FlyweightFactory` (*Figura 4.18: FlyweightFactory*) mantiene un grupo (*pool*) de objetos flyweight y garantiza que se compartan adecuadamente.
- La *Figura 4.19: El patrón Flyweight* muestra la interacción completa con el cliente.

#### Implementación clásica

Distinción clave:
- **Estado intrínseco (*Intrinsic state*):** Invariable, compartido y almacenado dentro del objeto flyweight.
- **Estado extrínseco (*Extrinsic state*):** Variable, dependiente del contexto y pasado al flyweight por el cliente en cada operación.

`flyweight.ts`:
```typescript
export interface Flyweight { 
    perform(customization: { id: string }): void 
} 

export class ConcreteFlyweight implements Flyweight { 
    constructor(private sharedState: Object) {} 

    public perform(customization: { id: string }): void { 
        console.log( 
            `ConcreteFlyweight: Shared (${JSON.stringify(this.sharedState)}) and unique (${customization.id}) state.`, 
        ) 
    } 
}
```

Definición de `FlyweightFactory` utilizando una caché LRU:

`flyweight.ts`:
```typescript
import QuickLRU from 'quick-lru'; 

export class FlyweightFactory { 
    private cache = new QuickLRU({maxSize: 1000}); 

    public getFlyweight(sharedState: Object): Flyweight { 
        const key = JSON.stringify(sharedState) 
        if (!this.cache.has(key)) { 
            console.log("FlyweightFactory: Can't find a flyweight, creating new one.") 
            this.cache.set(key, new ConcreteFlyweight(sharedState)) 
        } else { 
            console.log("FlyweightFactory: Reusing existing flyweight.") 
        } 
        return this.cache.get(key)! 
    } 

    public listFlyweights(): void { 
        const count = this.cache.size 
        console.log(`\nFlyweightFactory: I have ${count} flyweights:`) 
        this.cache.forEach((_, key) => { 
            console.log(key); 
        }); 
    } 
}
```

Uso por parte del cliente:

`flyweight.ts`:
```typescript
const factory = new FlyweightFactory() 

function addCar(plates: string, owner: string, brand: string, model: string, color: string) { 
    console.log("\nClient: Adding a car to database.") 
    const flyweight = factory.getFlyweight({ brand, model, color }) 
    flyweight.perform({ id: `${plates}_${owner}` }) 
} 

addCar("CL234IR", "James Doe", "Chevrolet", "Camaro2018", "pink") 
addCar("CL234IR", "James Doe", "BMW", "M5", "red") 
addCar("CL234IR", "James Doe", "BMW", "X1", "red") 

factory.listFlyweights()
```

#### Pruebas (*Testing*)

Para ejecutar las pruebas en `flyweight.test.ts`:

```bash
$ npm run test -- flyweight
```

#### Críticas

- **Mayor complejidad del código:** Separa el estado de los objetos en dos categorías, lo que requiere una gestión cuidadosa.
- **Sobrecarga de rendimiento:** Buscar y recuperar objetos flyweight de la caché puede introducir una pequeña penalización de CPU a cambio de memoria.
- **Riesgo de optimización prematura:** Aplicarlo antes de tener una necesidad real de optimización de memoria genera complejidad innecesaria.
- **Dificultad para mutar el estado compartido:** Si se altera el estado intrínseco de un objeto compartido, el cambio afecta a todos los contextos que lo utilizan.

#### Casos de uso del mundo real

Representación de caracteres en procesadores de texto y editores de código:

```typescript
interface CharacterFlyweight { 
    render(position: { x: number, y: number }): void; 
} 

class CharacterFlyweightFactory { 
    private characters = new Map<string, CharacterFlyweight>(); 

    getCharacter(char: string, font: string, size: number, color: string): CharacterFlyweight { 
        const key = `${char}-${font}-${size}-${color}`; 
        if (!this.characters.has(key)) { 
            this.characters.set(key, new CharacterFlyweightImpl(char, font, size, color)); 
        } 
        return this.characters.get(key)!; 
    } 
}
```

---

### Resumen

Este capítulo demostró todos los aspectos fundamentales de los patrones de diseño estructurales y cómo utilizarlos eficazmente en la práctica. Estos patrones se centran en la composición interna y externa de las clases y en cómo comparten implementaciones.

Comenzamos descubriendo los detalles del patrón **Adapter** y cómo ayuda a hacer que las clases funcionen con otras mediante la implementación de una interfaz común. Luego, exploramos el patrón **Bridge**, que nos permite separar y abstraer una entidad de su implementación. También aprendimos que, tras utilizar los patrones **Decorator** y **Proxy**, puedes mejorar la funcionalidad de los objetos en tiempo de ejecución sin utilizar la herencia.

Posteriormente, exploramos cómo el patrón **Façade** utiliza una interfaz más simple para controlar flujos de trabajo complejos. Al estructurar un grupo de objetos como compuestos (**Composite**), puedes crear un sistema jerárquico que comparte una interfaz común. Por último, utilizando el patrón **Flyweight**, aprendiste a utilizar el estado compartido para minimizar el uso de memoria o espacio.

En el próximo capítulo, aprenderás a aprovechar los patrones de comportamiento para aumentar la flexibilidad de la comunicación entre entidades.

---

### Preguntas y respuestas

1. **¿En qué se diferencia el patrón Façade del patrón Proxy?**
   - **Respuesta:** Aunque ambos patrones pueden simplificar sistemas complejos, tienen propósitos diferentes. El patrón Façade proporciona una interfaz simplificada para un subsistema complejo sin tener necesariamente la misma interfaz que los componentes del subsistema; su objetivo principal es reducir la complejidad para el cliente. El patrón Proxy, por otro lado, tiene la misma interfaz que el objeto que representa; su propósito principal es controlar el acceso a un objeto, a menudo agregando funcionalidades como inicialización perezosa, control de acceso o registro de eventos.

2. **¿Qué distingue al patrón Decorator del patrón Proxy?**
   - **Respuesta:** Ambos patrones implican envolver un objeto con otro, pero sus intenciones difieren. El patrón Decorator se utiliza para agregar nuevos comportamientos o responsabilidades a los objetos dinámicamente; los clientes pueden apilar múltiples decoradores en un solo objeto. El patrón Proxy controla el acceso a un objeto y normalmente no pretende agregar nuevos comportamientos funcionales; el cliente suele interactuar con un solo proxy que representa al objeto subyacente.

3. **¿Cuál es la diferencia clave entre el patrón Flyweight y la agrupación de objetos (*Object Pooling*)?**
   - **Respuesta:** Ambas técnicas tienen como objetivo mejorar la utilización de recursos, pero operan de manera diferente. El patrón Flyweight comparte una única instancia inmutable de un objeto en múltiples contextos, centrándose en reducir el uso de memoria al compartir un estado intrínseco. Por otro lado, la agrupación de objetos mantiene un grupo de objetos reutilizables individuales para reducir la sobrecarga de creación y destrucción repetida de objetos costosos de instanciar.
