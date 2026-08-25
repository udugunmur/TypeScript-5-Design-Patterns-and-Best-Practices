# Parte 3: Conceptos avanzados de TypeScript y mejores prácticas

## Capítulo 11: Exploración de patrones de diseño en arquitecturas de código abierto

En este capítulo final, exploraremos la aplicación práctica de patrones de diseño y mejores prácticas dentro de dos populares frameworks de TypeScript: **Apollo Client** y **TypeScript Remote Procedure Call (tRPC)**.

A lo largo de este capítulo, desarrollarás habilidades para identificar cómo se utilizan los patrones de diseño en Apollo Client y tRPC para optimizar la arquitectura de las aplicaciones. Aprenderás a leer e identificar patrones en el código fuente y a comprender las razones por las que se utilizan de esa manera.

Cubriremos los siguientes temas principales:

- Introducción a Apollo Client y tRPC
- Técnicas para revisar patrones de diseño en proyectos de código abierto
- Patrones de diseño en Apollo Client
- Patrones de diseño en tRPC

Al final de este capítulo, habrás adquirido una visión valiosa sobre cómo se utilizan los patrones de diseño en tecnologías de código abierto del mundo real. Esta comprensión práctica te permitirá seguir enfoques similares al diseñar tus propios proyectos.

---

### Requisitos técnicos

El paquete de código para este capítulo está disponible en el repositorio de GitHub de este libro:
https://github.com/PacktPublishing/TypeScript-5-Design-Patterns-and-Best-Practices/tree/main/chapters/chapter11-Open_source_patterns

---

### Introducción a Apollo Client y tRPC

En esta sección, proporcionaremos una breve descripción general de dos proyectos populares de código abierto que utilizan TypeScript internamente: Apollo Client y tRPC. Aunque tienen funcionalidades que compiten en ciertos aspectos, ambos tienen como objetivo simplificar el proceso de conectar el frontend con el backend.

#### Apollo Client

Apollo Client es un cliente integral para APIs GraphQL con gestión de estado y estrategias de almacenamiento en caché integradas. Ofrece un amplio conjunto de características para consultar, almacenar en caché y actualizar datos en tu aplicación.

> [!NOTE]
> **Estrategias de almacenamiento en caché de Apollo:**
> - **Cache-First:** Prioriza los datos en caché comprobando si existen entradas antes de realizar una solicitud de red.
> - **Network-Only:** Siempre obtiene datos nuevos del servidor, garantizando la información más actualizada a expensas de una mayor latencia.
> - **Cache-and-Network:** Devuelve los datos en caché inmediatamente mientras obtiene en paralelo datos actualizados de la red.
> - **No Cache:** Desactiva completamente el almacenamiento en caché.

##### ¿Por qué usar Apollo Client?

- **Backend GraphQL:** Es la herramienta de referencia para consumir APIs GraphQL.
- **Single-Page Applications (SPAs) complejas:** Proporciona un control total sobre las estrategias de caché tanto en el cliente como en el servidor.
- **Experiencia de depuración:** Cuenta con Apollo DevTools para inspeccionar consultas y estado de la caché.
- **Documentación sólida:** Amplia documentación oficial con multitud de ejemplos prácticos.
- **Soporte TypeScript:** Escrito en TypeScript, ofrece clientes totalmente tipados e integración fluida con `graphql-codegen`.

##### Uso básico

Instalación de dependencias:
```bash
$ npm install @apollo/client graphql
```

Creación del cliente:

`apollo/src/client.ts`:
```typescript
import { ApolloClient, InMemoryCache, gql } from "@apollo/client/core" 

const client = new ApolloClient({ 
    uri: "https://countries.trevorblades.com/", 
    cache: new InMemoryCache(), 
})
```

Consulta a la API GraphQL con seguridad de tipos:

`apollo/src/client.ts`:
```typescript
type Country = { 
    name: string 
    capital: string 
    currency: string 
} 

client 
    .query<Country>({ 
        query: gql` 
            query { 
                country(code: "IE") { 
                    name 
                    capital 
                    currency 
                } 
            } 
        `, 
    }) 
    .then((result) => { 
        // Type guard to ensure result.data.country is defined and matches expected structure 
        if (result.data && result.data.country) { 
            console.log(result.data.country.name); 
        } else { 
            throw new Error("Unexpected API response structure"); 
        } 
    }) 
    .catch((error) => { 
        console.error("Error fetching country data:", error); 
    });
```

#### tRPC – un framework RPC centrado en TypeScript

tRPC es un framework ligero diseñado para simplificar la creación de APIs seguras de extremo a extremo (*end-to-end typesafe*). Está especialmente adaptado para monorepos y proyectos donde tanto el frontend como el backend están escritos en TypeScript.

No es un reemplazo directo de GraphQL, sino un enfoque alternativo que aprovecha la inferencia estática del sistema de tipos de TypeScript sin necesidad de esquemas intermedios ni generación manual de código.

##### ¿Por qué usar tRPC?

- **Proyectos Full-stack en TypeScript:** Integración fluida y seguridad de tipos compartida entre cliente y servidor.
- **Arquitecturas Monorepo:** Permite compartir tipos, esquemas (como Zod) y enrutadores directamente.
- **Prototipado rápido:** Configuración mínima y sin código repetitivo (*boilerplate*), con inferencia automática de tipos.

##### Cuándo considerar Apollo Client

- **Proyectos políglotas o no exclusivos de TypeScript:** Si el backend está escrito en Python, Go o Java, GraphQL y Apollo Client ofrecen un estándar agnóstico al lenguaje.
- **Ecosistema y comunidad:** Apollo Client cuenta con un ecosistema maduro y herramientas multiplataforma consolidadas.

##### Uso básico

Instalación de dependencias:
```bash
$ npm install @trpc/server@next @trpc/client@next
```

Configuración del servidor con definición de rutas:

`Trpc/src/server.ts`:
```typescript
import { initTRPC } from "@trpc/server" 
import { createHTTPServer } from "@trpc/server/adapters/standalone" 

const t = initTRPC.create() 

const appRouter = t.router({ 
    hello: t.procedure 
        .input((val: unknown) => { 
            if (typeof val === "string") return val 
            throw new Error("Invalid input: expected string") 
        }) 
        .query((req) => { 
            return `Hello, ${req.input}!` 
        }), 
}) 

const server = createHTTPServer({ 
    router: appRouter, 
}) 

server.listen(3000) 

export type AppRouter = typeof appRouter
```

Configuración del cliente con inferencia de tipos de `AppRouter`:

`Trac/src/client.ts`:
```typescript
import { createTRPCClient, httpBatchLink } from '@trpc/client'; 
import type { AppRouter } from './server'; 

const trpc = createTRPCClient<AppRouter>({ 
    links: [ 
        httpBatchLink({ 
            url: 'http://localhost:3000', 
        }), 
    ], 
}); 

async function main() { 
    try { 
        const response = await trpc.hello.query('tRPC User'); 
        console.log(response); 
    } catch (error) { 
        console.error('Error:', error); 
    } 
} 

main();
```

---

### Técnicas para revisar patrones de diseño en proyectos de código abierto

Para identificar y aprender de la arquitectura de proyectos de código abierto consolidados:

1. **Comenzar por la documentación y registros de decisiones arquitectónicas (ADRs):** Revisa guías de arquitectura y decisiones de diseño registradas en el repositorio.
2. **Analizar la estructura del proyecto y convención de nombres:** La distribución de carpetas (por capas, por características o monorepo) y nombres como `CarFactory.ts` o `UserObserver.ts` revelan patrones implícitos.
3. **Buscar implementaciones de patrones conocidos:**
   - *Creacionales:* `Factory Method`, `Singleton`, `Builder`.
   - *Estructurales:* `Adapter`, `Decorator`, `Façade`, `Composite`, `Proxy`.
   - *Comportamiento:* `Observer`, `Strategy`, `Command`, `Chain of Responsibility`.
4. **Analizar dependencias, importaciones e inyección de dependencias:** Examinar cómo se comunican los módulos y si se utilizan interfaces para desacoplar clases concretas.
5. **Estudiar los casos de prueba y mocks:** Las pruebas unitarias e integración muestran cómo se diseñaron los componentes para interactuar y aislarse.

---

### Patrones de diseño en Apollo Client

Repositorio oficial: https://github.com/apollographql/apollo-client

#### Estructura del proyecto

- `src/`: Código fuente principal.
  - `cache/`: Mecanismos de normalización y almacenamiento en memoria (`InMemoryCache`).
  - `core/`: Lógica central del cliente (`ApolloClient`, `QueryManager`, `ObservableQuery`).
  - `link/`: Capa de red y canalización de solicitudes compuestas (`ApolloLink`).
  - `react/`: Integraciones y hooks específicos para React (`useQuery`, `useMutation`).
  - `utilities/`: Funciones auxiliares comunes.
- `config/`, `docs/`, `integration-tests/`, `patches/`, `scripts/`, `eslint-local-rules/`.

##### Proyectos monorepo frente a multi-repo

- **Monorepo:** Consolida múltiples paquetes en un solo repositorio, facilitando el código compartido y versiones coordinadas, aunque puede aumentar los tiempos de compilación.
- **Multi-repo:** Separa proyectos en repositorios independientes con límites claros y permisos granulares, a costa de una mayor complejidad en la sincronización de dependencias.

#### Patrones de diseño observados

##### Patrón Observer

Se utiliza extensamente en las clases `ObservableQuery` y `QueryManager` para emitir actualizaciones cuando los datos relevantes cambian en la caché:

```typescript
public watchQuery< 
    T = any, 
    TVariables extends OperationVariables = OperationVariables, 
>(options: WatchQueryOptions<TVariables, T>): ObservableQuery<T, TVariables> { 
    ... 
}
```

Permite que múltiples componentes de UI se suscriban a una consulta y reaccionen automáticamente a las mutaciones que modifican esos datos en caché.

##### Patrón Strategy

Permite alternar dinámicamente las políticas de obtención de datos (`fetchPolicy`: `cache-first`, `network-only`, `cache-and-network`):

```typescript
if (this.disableNetworkFetches && (options.fetchPolicy === "network-only" || options.fetchPolicy === "cache-and-network")) { 
    options = { ...options, fetchPolicy: "cache-first" }; 
} 
return this.queryManager.watchQuery<T, TVariables>(options);
```

##### Patrón Composite

Encadena múltiples instancias de `ApolloLink` para formar una capa de red unificada y modular mediante `ApolloLink.from()`:

```typescript
public static from(links: (ApolloLink | RequestHandler)[]): ApolloLink { 
    if (links.length === 0) return ApolloLink.empty(); 
    return links.map(toLink).reduce((x, y) => x.concat(y)) as ApolloLink; 
}
```

Uso del enlace compuesto:

```typescript
const link = ApolloLink.from([errorLink, authLink, httpLink]); 
const client = new ApolloClient({ 
    link, 
    cache: new InMemoryCache(), 
});
```

Cada enlace maneja una responsabilidad aislada (autenticación, registro, reintentos, transporte HTTP), interactuando como un único objeto compuesto.

##### Patrón Memento

Implementado en los métodos `extract` y `restore` de `InMemoryCache` para guardar y restaurar instantáneas del estado de la caché, fundamental para la hidratación en Renderizado del Lado del Servidor (SSR).

---

### Patrones de diseño en tRPC

Repositorio oficial: https://github.com/trpc/trpc

#### Estructura del proyecto

Estructura monorepo con paquetes desacoplados:
- `packages/server`: Implementación del núcleo del servidor tRPC.
- `packages/client`: Cliente tipado para interactuar con enrutadores.
- `packages/react-query`: Integración nativa con TanStack React Query.
- `packages/next`: Adaptadores y manejadores para rutas API de Next.js.
- `examples/`, `www/`, `scripts/`, `tests/`.

#### Patrones de diseño observados

##### Patrón Observable

Utilizado en `server/src/observable` para propagar errores de suscripción y gestionar estados de conexión en tiempo real:

```typescript
observer.error(TRPCClientError.from(error));
```

```typescript
const connectionState = behaviorSubject< 
    TRPCConnectionState<TRPCClientError<any>> 
>({ 
    type: 'state', 
    state: 'connecting', 
    error: null, 
}); 

const connectionSub = connectionState.subscribe({ 
    next(state) { 
        observer.next({ 
            result: state, 
        }); 
    }, 
});
```

##### Patrón Adapter

Los adaptadores en `server/src/adapters` traducen la API interna de tRPC a los manejadores de solicitudes de cada framework web (Next.js, Express, Fastify):

```typescript
import { createNextApiHandler } from '@trpc/server/adapters/next'; 
import { createContext } from '../../../server/trpc/context'; 
import { appRouter } from '../../../server/trpc/router/_app'; 

export default createNextApiHandler({ 
    router: appRouter, 
    createContext, 
});
```

Extensión de contexto con autenticación:

```typescript
import type { CreateNextContextOptions } from '@trpc/server/adapters/next'; 
import { getSessionFromCookie, type Session } from './auth'; 

interface CreateInnerContextOptions extends Partial<CreateNextContextOptions> { 
    session: Session | null; 
} 

export async function createContextInner(opts?: CreateInnerContextOptions) { 
    return { 
        session: opts?.session || null, 
    }; 
} 

export async function createContext(opts: CreateNextContextOptions) { 
    const session = getSessionFromCookie(opts.req); // Get session from cookie 
    const contextInner = await createContextInner({ session }); 
    return { 
        ...contextInner, 
        req: opts.req, 
        res: opts.res, 
    }; 
} 

export type Context = Awaited<ReturnType<typeof createContextInner>>;
```

##### Patrón Proxy

Permite invocar procedimientos anidados dinámicamente (`client.foo.bar.query()`) interceptando las llamadas en tiempo de ejecución sin definir métodos manualmente:

`server/src/unstable-core-do-not-import/createProxy.ts`:
```typescript
return createRecursiveProxy<ReturnType<RouterCaller<any, any>>>( 
    async ({ path, args }) => { 
        const fullPath = path.join('.'); 
        // Rest of logic to resolve the procedure and execute it 
    } 
);
```

Comportamiento del proxy recursivo:

```typescript
const basic = createRecursiveProxy((opts) => { 
    return opts; // This function typically handles the resolution logic 
}); 

const queryResult = basic.foo.bar.query(); 
console.log(queryResult); // Output: { path: ['foo', 'bar', 'query'], args: [] } 

const queryWithArgsResult = basic.foo.bar.query({ id: 1 }); 
console.log(queryWithArgsResult); // Output: { path: ['foo', 'bar', 'query'], args: [{ id: 1 }] } 

const mutateResult = basic.foo.bar.mutate(); 
console.log(mutateResult); // Output: { path: ['foo', 'bar', 'mutate'], args: [] }
```

---

### Resumen

En este capítulo final, analizamos la implementación práctica de patrones de diseño en tecnologías consolidadas de código abierto:
- **Apollo Client:** Utiliza el patrón **Observer** para consultas reactivas, **Strategy** para políticas de caché, **Composite** para cadenas de enlaces de red (`ApolloLink`) y **Memento** para serialización y restauración del estado de la caché en SSR.
- **tRPC:** Aplica el patrón **Proxy** para la resolución dinámica de procedimientos con seguridad de tipos de extremo a extremo, el patrón **Adapter** para interoperar con diversos frameworks HTTP y el patrón **Observable** para gestionar estados de conexión y suscripciones en tiempo real.

Comprender cómo se aplican estos patrones en proyectos reales te proporciona las herramientas arquitectónicas para diseñar sistemas limpios, escalables y mantenibles en TypeScript.

---

### Preguntas y respuestas

1. **¿Cómo mejora el patrón Observer la funcionalidad de Apollo Client?**
   - **Respuesta:** Permite que los componentes de la interfaz de usuario se suscriban a los resultados de consultas en caché. Cuando una mutación u otra consulta actualiza la información en la caché normalizada, todos los componentes suscritos reciben automáticamente los nuevos datos sin necesidad de recargar manualmente.

2. **¿Cómo se implementa el patrón Composite en Apollo Client?**
   - **Respuesta:** Se implementa a través de la clase `ApolloLink` y métodos como `ApolloLink.from()`, `concat()` y `split()`, que permiten combinar múltiples enlaces independientes (autenticación, manejo de errores, reintentos) en un único enlace compuesto que se ejecuta en secuencia.

3. **¿Qué papel juega el patrón Proxy en tRPC?**
   - **Respuesta:** En tRPC, el patrón Proxy (mediante `createRecursiveProxy`) intercepta las llamadas a métodos encadenados en el cliente (como `trpc.user.getById.query({ id: 1 })`), capturando dinámicamente la ruta del procedimiento y sus argumentos para enviarlos al servidor con verificación de tipos estática en tiempo de compilación.
