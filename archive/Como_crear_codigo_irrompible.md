---
idx: '013'
title: 'Como crear código irrompible'
date: '2022-11-10'
timage: '../images/archive/013/0.png'
author: 'Jesús David Bossa'
authorImage: '../images/author/photo.JPG'
authorDescription: 'Arquitecto de software, intentando crear cosas geniales.'
tags: ['Opinión', 'charla', 'meetup']
---

De pronto suena raro, al principio, código irrompible, que significa.

En resumidas cuentas, es un código que se rompe, básicamente que el compilador o interprete, nos genere un error.

## ¿Como llegamos a algo así?

Bueno, cuando escribimos código muchas cosas de la que normalmente hacemos está expuesto a errores, unas más que otras, incluso ocasiones que no son provocadas por los desarrolladores o usuarios, por ejemplo tengamos en cuenta el siguiente ejemplo.

![Flujo normal de una app](../images/archive/013/1.png)

<div>

_Flujo normal de una app_

</div>

Un simple request verdad, queremos actualizar nuestra edad en una red social, entonces el back-end recibe el request del fornt-end, luego válida que los datos estén bien, luego haga el cambio en la base de datos y por último nos responda que todo fue bien, nada raro, pero en cualquiera de todos esos puntos el ejemplo podría fallar, ya sea la conexión a internet, datos incompletos, etc.

Entonces como manejamos casos de error, dependiendo el lenguaje, hay varias opciones, try/catch, promise catch, lo que viene siendo manejo de excepciones.

## Hablemos del try / catch

Esta forma de manejar excepciones fue implementada hace mucho tiempo atrás en [los 60](https://en.wikipedia.org/wiki/Exception_handling#History), empezó en lisp, fue lo suficientemente popular como para que los lenguajes de programación de la época decidirán implementarlo también.

¿Por qué creo que esta no es la mejor idea? Bueno pasa que en esta estructura.

![Try / catch y la railway oriented programming](../images/archive/013/2.png)

<div>

_Try / catch y la railway oriented programming_

</div>

`TRY` ejecuta el happy path, todo lo que debería pasar si todo sale bien, si no se ejecuta él `CATCH`, el control de la excepción.

`CATCH` que es lo que sucede cuando el proceso anterior cae en un error.

Básicamente en el `CATCH` estamos diciendo que paso algo que probablemente no sabemos qué paso, entramos a una excepción, nos salimos del sistema de ejecución normal de la app y gente, los errores no deberían ser excepciónales, al menos nos no en un sistema o arquitectura robusta de desarrollo.

## ¿Entonces si no es así entonces cómo? 

Bueno podríamos empezar haciéndonos cargo de saber que es lo que pasa, el mejor amigo en estos casos es el compilador, un intérprete de código bien informado de cada cosa que ingresamos en el código siempre es muy útil, avisándonos de problemas de manera temprana, para eso deberíamos evitar el tipado dinámico.


- Python

```python
def sum_numbers(a, b):
    return a + b

print(sum_numbers(10, 5)) # 15
print(sum_numbers('Bob', 'Mark')) # BobMark
```
-----
```python
def sum_numbers(a: int, b: int) -> int:
    return a + b

print(sum_numbers(10, 5)) # 15
print(sum_numbers('Bob', 'Mark')) # Error
```

- Javascript / Typescript

```jsx
function sum_numbers(a,b) {
    return a + b
}
console.log(sum_numbers(10, 5)) // 15
console.log(sum_numbers("Bob", "Mark")) // BobMark
```
-----
```tsx
function sum_numbers(a: number, b: number) {
    return a + b
}
console.log(sum_numbers(10, 5)) // 15
console.log(sum_numbers("Bob", "Mark")) // Error
```

En el primer ejemplo de cada lenguaje vemos un ejemplo de como una función común e inofensiva, pero al no saber el tipo de cada parámetro que recibe la función podría desencadenar un error a largo o corto plazo.

Recomendaría en estos casos usar tipado fuerte o strong typing, deberíamos decirle en el ejemplo que son números o texto y números, lo que consideres que te funciona, pero deberíamos asegurarnos de que vamos a recibir en las funciones para no recibir parámetros desconocidos y desencadenar un error.

## Sin embargo

Esto soluciona los problemas más sencillos, pero que pasa cuando la data es opcional o es la una o la otra, pues simplemente aquí podríamos manejar `IF`, `CASES`, identificar si viene data, tipo o un error.

Sin embargo, hay un caso más complicado, que pasa cuando las funciones nativas del intérprete o compilador nos fallan, que tal si hacemos una división por cero, que el compilador sabe que va a trabajar con números pero nunca qué números.

Hay veces que no es hasta la ejecución que nos damos cuenta, a menos que los tengamos en cuenta, la fácil es un `IF` en caso de tener un cero de un lado y otro del otro no ejecutar, pero que tal si no tenemos forma de saberlo, normalmente siempre existen librerías de operaciones matemáticas seguras que se encargan de este tipo de validación, los lenguajes de programación también tienden a actualizarse para ser más amables con el desarrollador y detectar este tipo de problemitas a tiempo, como por ejemplo `TypeScript` o `Rust`, también podrían usar `eslint` o `rome` que analizan el código cada vez que escriben avisando de posibles errores.

A mi parecer hay un lenguaje que hace esto de una forma espectacular, `Rust` introduce un concepto llamado panic, que no es un error, pero nos da un aviso temprano que eso que estamos haciendo puede llegar a fallar y casi que nos obliga a manejarlo, de hecho un dato curioso de porque es tan amado `Rust` es porque es muy difícil hacerlo fallar, ya que el compilador de este lenguaje es superbueno, tanto así que es muy fácil llegar a hacer un código irrompible.

Un poco poético, a mi parecer, pero el mundo real a veces no lo es tanto y para llegar a usar estas tecnologías aquí y hablo de lo que les he venido hablando, `Rust`, tipado fuerte puede llegar a ser más o menos complicado por el tipo de empresa o por el momento educativo que estén, pero hey al menos el saber que estas cosas existen ya es importante y bueno espero al menos alguno haya aprendido algo nuevo hoy o se anime a usar alguna de estas tecnologías, muchas gracias por leer.