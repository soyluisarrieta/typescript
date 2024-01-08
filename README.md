# Mis Notas de Aprendiendo Typescript

Curso: [De Novato a Ninja: Aprende TypeScript - curso intensivo (#1)](https://youtu.be/fUgxxhI_bvc)  
Autor del curso: [Miguel Angel Durán](https://www.youtube.com/@midulive)

## ¿Qué es Typescript?

Es un lenguaje de programación de **[código abierto](https://github.com/microsoft/TypeScript) desarrollado por Microsoft** que se basa en JavaScript pero sólo funciona en tiempo de compilación y no de ejecución. 

**TypeScript** añade funcionalidades y herramientas adicionales a JavaScript, como la **verificación de tipos estáticos**, lo que significa que es posible revisar los errores en el código antes de ejecutarlo. 

> **DATO CURIOSO:** Typescript está hecho con Typescript 🤯

## Ventajas de usar Typescript

- [x] Facilitar el desarrollo de aplicaciones grandes y complejas.
- [x] Aumentar la calidad y la eficiencia del código.
- [x] Autocompletado preciso.
- [x] Se está autodocumentando el código con los tipos.
- [x] Añade tipado fuerte y dinámico a JavaScript.
- [x] Prácticamente un estandard en la industria.
- [x] Una gran cantidad de recursos realizados con typescript, por ejemplo [Shadcn/ui](https://ui.shadcn.com/)

## Extensión recomendada para VSCODE

**ID:** yoavbls.pretty-ts-errors  
**Nombre:** [Pretty TypeScript Errors](https://marketplace.visualstudio.com/items?itemName=yoavbls.pretty-ts-errors)  
**Descripción:** Haga que los errores de TypeScript sean más bonitos y legibles para los humanos en VSCode.  
**Autor:** yoavbls  

## Lista de tipos de Typescript

| Tipo        | Descripción                                                                     | Ejemplo                                                                |
| ----------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `number`    | Para valores numéricos, decimales o enteros.                                    | `let num: number = 5;`                                                 |
| `string`    | Para cadenas de texto.                                                          | `let str: string = "Hola";`                                            |
| `boolean`   | Para valores verdaderos o falsos.                                               | `let isTrue: boolean = true;`                                          |
| `null`      | Para valores que no tienen ningún valor o no existen.                           | `let empty: null = null;`                                              |
| `undefined` | Para variables que han sido declaradas pero aún no se les ha asignado un valor. | `let notDefined: undefined;`                                           |
| `object`    | Para estructuras más complejas, como objetos, matrices, etc.                    | `let obj: object = {key: "value"};`                                    |
| `any`       | Para cualquier tipo de dato, evita la verificación de tipo estático.            | `let anything: any = "Hola";`                                          |
| `void`      | Usualmente para indicar que una función no retorna ningún valor.                | `function nothing(): void {}`                                          |
| `unknown`   | Similar a `any`, pero más seguro. Requiere verificar el tipo antes de su uso.   | `let unknownValue: unknown;`                                           |
| `never`     | Para valores que nunca deben ocurrir.                                           | `function error(message: string): never { throw new Error(message); }` |

Es posible usar el operador | para indicar que puede ser de un tipo u otro:

```ts
let text: number | string;
let text2: string | 2;
```

## Inferencia

```ts
// TypeScript infiere automáticamente que a y b son de tipo number
const a = 1
const b = 2
const c = a + b
// c también será number

// TS también tiene inferencia en el tipo que retorna
function saludar({name, age}: {name: string, age: number}) {
  console.log(`Hola ${name}, tienes ${age} años`)
  return age
}
// TS sabe perfectamente que age es de tipo number

// También existe inferencia en funciones anonimas segun el contexto
const avengers = ['Iron Man', 'Hulk', 'Loki']

avangers.forEach(function (avanger) {
  console.log(avanger.toUpperCase())
})
// TS sabe perfectamente que recibe un string y que no retorna nada 
```

## Tipado de funciones y callbacks

```ts
// Función que ejecuta el callback
function funcionDecirHola (callback: (name: string) => void) => {
  callback()
}

// Callback que ejecuta el log
const decirHola = (name: string) => {
  console.log(`Hola ${name}`)
}

// Ejecutar función enviando el callback
funcionDecirHola(decirHola)
```

Existe el tipo `Function` para referirse al callback pero es como `any` y en lo posible se debe evitar estos dos tipos. 
En su lugar es mejor tipar los parámetros del callback en la función. En este caso:

- En la **función** con el parámetro *callback* se especifica de tipo que es una arrow function que contiene el parámetro `name` de tipo `string` y que retonar `void`, es decir nada.
- En el **callback**, `name` se especifica que es de tipo `string` y se infiere que retorna `void`
- Si el callback retornara un string, entonces en la función se especificaría de tipo `string` en lugar de `void`

Los tipos para una función o un callback pueden usarse para los parámetros y para el tipo que retorna, por ejemplo:

```ts
// Se tipa los parámetros de la arrow function y el tipo que retorna
const sumar = (a: number, b: number): number => {
  return a + b
}

// En este caso se tipa la constante como arrow function y luego se añade la función
const restar: (a: number, b: number) => number = (a,b) => {
  return a - b
}

// Ambos casos son iguales, pero es más legible la primera
```

`never` es un tipo de dato que representa un valor que nunca ocurre. Se utiliza en funciones que siempre lanzan una excepción o una función que tiene un bucle infinito. Por ejemplo:

```ts
// Función que siempre lanza una excepción
function throwError(message: string): never {
    throw new Error(message);
}

// Función con un bucle infinito
function infiniteLoop(): never {
    while (true) {}
}
```

## Los Type Alias

Los alias de tipo en TypeScript son una forma de **crear un nuevo nombre para un tipo existente**. Se podría considerar como darle un apodo a un tipo. Esto puede ser útil cuando se trabaja con tipos complejos que **podría ser reutilizado en el código**. Al utilizar un alias de tipo, puedes mejorar la legibilidad y mantener el código más limpio, por ejemplo:

```ts
  type Usuario = {
      nombre: string;
      email: string;
      fechaNacimiento: Date;
  };

  function imprimirUsuario(usuario: Usuario) {
      console.log(`
        Nombre: ${usuario.nombre}, 
        Email: ${usuario.email}, 
        Fecha de Nacimiento: ${usuario.fechaNacimiento}
      `);
  }

  let usuario1: Usuario = {
      nombre: 'Juan',
      email: 'juan@example.com',
      fechaNacimiento: new Date(1990, 1, 1)
  };

  imprimirUsuario(usuario1);
```

### Optional Properties

Es posible tener *Optional Properties* (propiedades opcionales) simplemente añadiendo el signo de interrogación, en el siguiente ejemplo se crea un segundo usuario pero sin la fecha de nacimiento, sin este interrogante Typescript mostraría errores:

```ts
  type Usuario = {
      nombre: string;
      email: string;
      fechaNacimiento?: Date;
  };

  let usuario2: Usuario = {
      nombre: 'Juan',
      email: 'juan@example.com',
  };
```

### Read Only

También es posible establecer una propiedad como *Read Only* (Sólo lectura), es decir no es posible editar el valor de la propiedad.

```ts
  type Usuario = {
      readonly id: number;
      nombre: string;
      email: string;
      fechaNacimiento?: Date;
  };
```

### Template union types

Con los Tipos de unión de plantilla podemos hacer que un valor siga un patrón y asignarle un tipo a cada parte de la plantilla.

```ts
type UsuarioID = `${string}-${string}-${string}-${string}-${string}`

type Usuario = {
    readonly id?: UsuarioID; // <- ✅ Asignamos el template union type
    nombre: string;
    email: string;
    fechaNacimiento: Date;
};
```

Otro caso es por ejemplo los colores hexadecimales.

```ts
type HexadecimalColor = `#${string}`

const color: HexadecimalColor = `0033ff`    // <- ❌
const color2: HexadecimalColor = `#0033ff`  // <- ✅
```

### Union types

Indicar que una variable puede contener uno u otra cadena de texto.

```ts
type UsuarioCountry = 'Colombia' | 'México' | 'Estados Unidos' | 'España'

type Usuario = {
    readonly id?: UsuarioID;
    nombre: string;
    email: string;
    fechaNacimiento: Date;
    country: UsuarioCountry;     // <- ✅ Asignamos el union type
};

Usuario.country = 'Venezuela'   // <- ❌
Usuario.country = 'España'      // <- ✅
```

### Interception types

Crear un nuevo tipo en base a otros tipos usando el operador &

```ts
type Usuario = {
    readonly id?: UsuarioID;
    nombre: string;
    fechaNacimiento: Date;
    country: UsuarioCountry;
}

type Credentials = {
    email: string;
    password: string;
}

type Admin = Usuario & Credentials // <- ✅ Fusionamos dos o más tipos 
```

### Type indexing

Permite reutilizar partes de un tipo, es decir, solo alguna propiedad en lugar de todas

```ts
type Usuario = {
    nombre: string;
    direccion: {
      carrera: string,
      calle: string
    };
}

const direccionUsuario: Usuario['direccion'] = {
    carrera: '123',
    calle: '45'
}
```

### Type from value

Con `typeof` en Typescript, es como un "supraconjunto" con el que puedo crear un nuevo tipo a partir de un valor.

```ts
const direccionUsuario = {
    carrera: '123b',    // <- Tipo string
    calle: 45           // <- Tipo string
}

type Direccion = typeof direccionUsuario

const direccionAdmin1: Direccion = {
    carrera: 33,                   // <- ❌ TS reconoce que el tipo debe ser string
    calle: true                    // <- ❌ TS reconoce que el tipo debe ser number
}

const direccionAdmin2: Direccion = {
    carrera: '123b',               // <- ✅ TS reconoce que es de tipo string
    calle: 45                      // <- ✅ TS reconoce que es de tipo number
}
```

### Type from function return

En este caso no lo toma a partir de un valor sino de lo que retorne una función.

```ts
function crearUsuario() {
  return {
    nombre: 'Luis Arrieta',
    edad: 27
  }
}

type Usuario = ReturnType<typeof crearUsuario>  // <- ✅ TS reconoce los tipos que retorna la función
```

## Arrays

Teniendo en cuenta la inferencia, podría verse normal inicializar una variable como un array y que TS lo tome como tipo array, pero hay un problema en este caso. TS lo infiere como tipo never, es decir que siempre debe estar vacío.

```ts
const lenguajes = []
lenguajes.push('Javascript') // <- ❌ TS no permite indices ya que es tipo never
```

Para este caso es posible especificarle el tipo, por ejemplo:

```ts
const lenguajes: string[] = [] // <- ✅ Los contenidos del array son de tipo string

 // ✅ TS permite indices pero SOLO strings
lenguajes.push('Javascript')
lenguajes.push('PHP')
lenguajes.push('Python')

lenguajes.push(3) // <- ❌
```

Otra forma de especificarlo es de la siguiente manera:

```ts
const lenguajes: Array<string> = [] // <- ✅ Los contenidos del array son de tipo string
```

Ahora para varios tipos en un array, la forma de especificarlo sería así:

```ts
const lenguajes: (string | number)[] = [] // <- ✅ Los contenidos del array son de tipo string o number
```

### Matrices y tuplas

Para indicar el tipo para las matrices es tan sencillo como especificar que es un array de arrys:

```ts
const gameBoard: string[][] = [  // <- ✅ 
  ['X' , ''  ,'O'],
  [''  , 'O' ,'X'],
  ['O' , 'O' ,'X'],
]
```

pero podría definirse cualquier string, para indicar que solo reciba ciertos caracteres podemos usar los [Union types](#union-types) y limitar el array con las tuplas para que sea un Tic Tac Toe de 3x3:

```ts
type CellValue = 'X' | 'O' | ''
type GameBoard = [
  [CellValue, CellValue, CellValue],
  [CellValue, CellValue, CellValue],
  [CellValue, CellValue, CellValue]
]

const gameBoard: GameBoard = [  
  ['X' , ''  ,'O'],
  [''  , 'O' ,'X'],
  ['O' , 'O' ,'X'],
]
```

Otros ejemplos sobre tuplas:

```ts
type State = [string, (newName: string) => void]
const [hero, setHero]: State = useState('Hulk')

// ---

type RGB = [number, number, number]
const colorRGB: RGB = [255, 53, 23]
```

## Objetos

### Enums

En TS, lo mejor sería usar Enums:

```ts
enum ERROR_TYPES {
  NOT_FOUND,
  UNAUTHORIZED,
  FORBIDDEN
}

function mostrarMensaje (tipoDeError: ERROR_TYPES) {
  if (tipoDeError === ERROR_TYPES.NOT_FOUND){
    console.log('No se encuentra el recurso')
  } else if (tipoDeError === ERROR_TYPES.UNATHORIZED){
    console.log('No tienes permisos para acceder')
  } else if (tipoDeError === ERROR_TYPES.FORBIDDEN){
    console.log('No tienes permisos para acceder')
  }
}
```

Así es como TS lo transforma a JS:

```js
var ERROR_TYPES;
(function (ERROR_TYPES) {
  ERROR_TYPES[ERROR_TYPES["NOT_FOUND"] = 0] = "NOT_FOUND";
  ERROR_TYPES[ERROR_TYPES["UNAUTHORIZED"] = 1] = "UNAUTHORIZED";
  ERROR_TYPES[ERROR_TYPES["FORBIDDEN"] = 2] = "FORBIDDEN";
})(ERROR_TYPES || (ERROR_TYPES = {}));

function mostrarMensaje(tipoDeError) {
  if (tipoDeError === ERROR_TYPES.NOT_FOUND) {
    console.log('No se encuentra el recurso');
  } else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED) {
    console.log('No tienes permisos para acceder');
  } else if (tipoDeError === ERROR_TYPES.FORBIDDEN) {
    console.log('No tienes permisos para acceder');
  }
}
```

Como puedes ver, el enumerador Typescript se ha transformado en una función de autoinvocación que crea y manipula un objeto llamado `ERROR_TYPES`. En este objeto, cada clave de enum se convierte en una propiedad con un valor numérico, y cada valor numérico se convierte en una propiedad con una clave de enum como valor.

#### Enums constantes

```ts
const enum ERROR_TYPES {
  NOT_FOUND,
  UNAUTHORIZED,
  FORBIDDEN
}

function mostrarMensaje (tipoDeError: ERROR_TYPES) {
  if (tipoDeError === ERROR_TYPES.NOT_FOUND){
    console.log('No se encuentra el recurso')
  } else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED){
    console.log('No tienes permisos para acceder')
  } else if (tipoDeError === ERROR_TYPES.FORBIDDEN){
    console.log('No tienes permisos para acceder')
  }
}
```

```ts
function mostrarMensaje(tipoDeError) {
  if (tipoDeError === 0 /* NOT_FOUND */) {
    console.log('No se encuentra el recurso');
  } else if (tipoDeError === 1 /* UNAUTHORIZED */) {
    console.log('No tienes permisos para acceder');
  } else if (tipoDeError === 2 /* FORBIDDEN */) {
    console.log('No tienes permisos para acceder');
  }
}
```

Como puedes ver, los miembros de `ERROR_TYPES` se han reemplazado en línea con sus valores correspondientes en el código JavaScript generado. 

Los comentarios `/* NOT_FOUND \*/`, `/* UNAUTHORIZED \*/` y `/* FORBIDDEN */` sólo están ahí para facilitar la lectura del código y no se incluirían en la salida final de la compilación de TypeScript a JavaScript.

#### Con un valor especifico

```ts
enum ERROR_TYPES {
  NOT_FOUND = 'NO_ENCONTRADO',
  UNAUTHORIZED = 'NO_AUTORIZADO',
  FORBIDDEN = 'PROHIBIDO'
}

function mostrarMensaje (tipoDeError: ERROR_TYPES) {
  if (tipoDeError === ERROR_TYPES.NOT_FOUND){
    console.log('No se encuentra el recurso')
  } else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED){
    console.log('No tienes permisos para acceder')
  } else if (tipoDeError === ERROR_TYPES.FORBIDDEN){
    console.log('No tienes permisos para acceder')
  }
}
```

Y a JS quedaría:

```js
var ERROR_TYPES;
(function (ERROR_TYPES) {
  ERROR_TYPES["NOT_FOUND"] = "NOT_FOUND";
  ERROR_TYPES["UNAUTHORIZED"] = "UNAUTHORIZED";
  ERROR_TYPES["FORBIDDEN"] = "FORBIDDEN";
})(ERROR_TYPES || (ERROR_TYPES = {}));

function mostrarMensaje(tipoDeError) {
  if (tipoDeError === ERROR_TYPES.NOT_FOUND) {
    console.log('No se encuentra el recurso');
  } else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED) {
    console.log('No tienes permisos para acceder');
  } else if (tipoDeError === ERROR_TYPES.FORBIDDEN) {
    console.log('No tienes permisos para acceder');
  }
}
```

#### enum vs const enums

Se prefiere `const enum` para optimizar el tamaño del código, ya que se resuelven en tiempo de compilación. Sin embargo, si los enums se van a utilizar fuera de la aplicación, como en APIs externas, es mejor usar `enum` regulares, dado que se traducen en objetos en tiempo de ejecución.

## Aserciones de tipos

Las aserciones de tipo en TypeScript son una forma de decirle al compilador "confía en mí, sé lo que estoy haciendo". Es como una conversión de tipo en otros lenguajes, pero no realiza ninguna comprobación especial o reestructuración de datos. No tiene impacto en tiempo de ejecución, y el compilador lo usa únicamente para la verificación de tipos.

```ts
let algunaVariable: any = "Hola, soy una cadena";

// Uso de aserción de tipo para tratar 'algunaVariable' como una cadena
let longitudDeCadena: number = (algunaVariable as string).length;
```

```ts
const canvas = document.querySelector('canvas') as HTMLCanvasElement // <- Confía que es un Canvas
const contexto = canvas.getContext('2d') 
```

En este caso, no es una buena idea usar aserciones debido a que podría ocultar errores evidentes y potencialmente dañinos. Aquí está lo que está ocurriendo con cada línea de código, como por ejemplo:

- **Canvas no encontrado:** document.querySelector('canvas') puede retornar null.
- **Contexto no soportado:** getContext('2d') o getContext('webgl') puede retornar null.
- **Dimensiones del canvas no definidas:** puede causar problemas al dibujar.
- **Uso incorrecto de los métodos del contexto:** puede resultar en dibujos inesperados.
- **Problemas de rendimiento:** dibujar en el canvas puede ser costoso en términos de rendimiento.
- **Compatibilidad con navegadores antiguos:** no todos los navegadores soportan el elemento canvas.

Lo ideal sería usar JS y dejar que TS infiera los tipos:

```ts
const canvas = document.querySelector('canvas')

if (canvas instanceof HTMLCanvasElement) {
  const contexto = canvas.getContext('2d') 
}
```

Este código es correcto porque primero selecciona un elemento canvas del documento. Luego, antes de intentar usar el método `getContext('2d')`, verifica que `canvas` sea realmente una instancia de `HTMLCanvasElement`. Esto evita errores en tiempo de ejecución al intentar llamar a `getContext('2d')` en caso de que canvas sea `null`.

## Interfaces

Tanto las interfaces como los alias de tipo en TypeScript nos permiten darle un nombre a un tipo de objeto, pero se usan de manera un poco diferente.

Las interfaces son como un plano que define cómo debería ser un objeto. Son útiles cuando queremos asegurarnos de que un objeto tenga una cierta forma.

```ts
interface Punto {
    x: number;
    y: number;
}
```

### Diferencia entre Interfaces y Types aliases

La principal diferencia es que las interfaces son más útiles para definir la forma de los objetos, mientras que los alias de tipo son más flexibles y pueden usarse con cualquier tipo de valor. Algunas otras diferencias son:

| Característica                      | Interfaces                                                              | Type Aliases                                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Extensiones                         | Pueden ser extendidas y fusionadas.                                     | No pueden ser extentidos o fusionados.                                                         |
| Tipos primitivos y literales        | No pueden representar tipos primitivos.                                 | Pueden representar tipos primitivos, como `number`,`string`,`boolean`.                         |
| Usos de Uniones y tipos de utilidad | Tienen retricciones en su uso con uniones de tipos y tipos de utilidad. | Pueden utilizarse con uniones de tipos y tipos de utilidad (como `Partial<t>` o `Readonly<t>`) |
| Declaraciones duplicadas            | Pueden tener múltiples declaraciones y se fusionarán automáticamente.   | No puede tener declaraciones duplicadas.                                                       |
| Sintaxis                            | Usa la palabra clave `Interface`                                        | Usa la palabra clave `type`                                                                    |

**La recomendación es usar** `Interfaces` cuando se trata de programación orientado a objetos y **`Types aliases` siempre que se pueda**.