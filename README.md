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