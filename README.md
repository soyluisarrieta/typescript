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