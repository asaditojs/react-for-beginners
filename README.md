# Documentación React for Beginners

![asaditoJs](https://avatars0.githubusercontent.com/u/57371978?s=200&v=4 "asaditoJs")


## Asadito.Js React for Beginners

[00 - Configurando el entorno](#configurando-el-entorno)

[01 - Estructura del proyecto](#estructura-del-proyecto)

[02 - Export e import](#export-e-import)

[03 - Componentes funcionales](#componentes-funcionales)

[04 - JSX](#jsx)

[05 - Estilando un componente](#estilando-un-componente)

[06 - Props](#props)

[07 - Componentes hijos](#componentes-hijos)

[08 - Listas de componentes](#listas-de-componentes)

[09 - Condicionales en JSX](#condicionales-en-jsx)

[10 - Personalizando un componente](#personalizando-un-componente)

[11 - Iconos](#iconos)

[12 - Renderizado condicional](#renderizado-condicional)

[13 - Componentes de clase](#componentes-de-clase)

[14 - Métodos y eventos](#métodos-y-eventos)

[15 - Estado](#estado)

[16 - setState](#setstate)

[17 - Elevando el estado](#elevando-el-estado)

[18 - Elevando el estado - Ejemplo](#elevando-el-estado-ejemplo)

[19 - Inputs controlados](#inputs-controlados)

[20 - Prop drilling](#pop-drilling)

[21 - Contexto](#contexto)

[22 - Contexto Patrón](#contexto-patrón)

[23 - Ciclo de vida](#ciclo-de-vida)

[24 - Fetch en React](#fetch-en-react)

[25 - Videos explicativos Workshop por Cristhian Duran](#videos-explicativos)


## Configurando el entorno

Desde la terminal (ya sea la de VS Code, Git bash, o consola de Linux/Mac), ubicadas en la carpeta donde vamos a querer que se cree la carpeta de nuestro proyecto, escribimos

```terminal
npx create-react-app my-app
```

donde `my-app` es el nombre de nuestro proyecto/aplicación. 

Cuando se termine de bajar e instalar todas las librerías y dependencias, tenemos que movernos a la carpeta de nuestro proyecto y crear dos archivos, `jsconfig.json` y `.env`:

```terminal
cd my-app/
touch jsconfig.json
touch .env
```

En el archivo `jsconfig.json` tenemos que poner

```json
{
    "compilerOptions": {
      "baseUrl": "src"
    },
    "include": ["src"]
}
```

Esto va a permitir que podamos usar rutas absolutas y no relativas a la hora de importar archivos. En el archivo `.env`, *si estamos en Windows* tenemos que colocar:

```text
SASS_PATH=./node_modules;./src
```

y si estamos en *Linux/Mac*,


```text
SASS_PATH=node_modules:src
```

Luego en consola, en la carpeta de nuestro proyecto, tenemos que ejecutar:

```terminal
npm install node-sass
```

Por último, en la carpeta src, tenemos que dejar solamente los siguientes archivos: `App.js`, `index.js`, `index.css`. El código de cada uno de ellos debe quedar así:

```javascript
// App.js
import React from 'react'
import './index.css'

const App = () => (
    <></>
);

export default App
```


```javascript
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

<br />

**Todos estos pasos los hacemos si estamos creando un proyecto desde cero**. Si nos clonamos un repositorio de uno ya hecho, lo primero que tenemos que hacer es, dentro de la carpeta del proyecto:

```terminal
npm install
```

Para que se instalen todas las dependencias (cuando se sube un proyecto a git las dependencias y librerías no se suben). Si en algún momento hacemos `git pull` puede que tengamos que hacer lo mismo si es que se instaló alguna dependencia o librería que nos falte.

<br />

Para iniciar la aplicación, en la carpeta de la misma, hacemos en consola:

```terminal
npm start
```

## Estructura del proyecto

Es bastante usual que un proyecto de React conste de varias decenas (e incluso centenas) de archivos, por lo que tener una buena organización de los mismos es fundamental. React no nos dice ni nos obliga a ordernarlos de cierta manera, por lo que, en proyectos personales, tenemos que buscar un criterio con el que nos sintamos cómodas. Hay una sugerencia bastante conocidad en el ámbito que dice: *mueve cosas hasta que se sientan bien*.

De todas formas, hay un par de sugerencias a tener en cuenta. Los lineamientos principales o que deberíamos tener presentes son básicamente dos:
  - la estructura del proyecto debería informarmos inmediatamente de las cosas que contiene con solo mirarlo (esto se conoce como "screaming architectura").
  - no debería haber carpetas anidadas con muchos niveles de profundidad (es decir, muchas carpetas dentro de carpetas dentro de carpetas). Es preferible una estructura más horizontal (todo a un mismo nivel) que vertical (o jerárquica).
  
Todos los archivos con los que trabajemos van a ir dentro de la carpeta **src**. Dentro de esta, una estructura mínima podría tener las carpetas:
  
  - components
  - assets
  - styles
  
En `components` van a ir todos nuestros componentes, en `assets` aquellos archivos como ímagenes, íconos, fuentes, etc (podemos subdividirlos en categorías, y si tenemos muchas, subdivir estas) y en `styles` todos aquellos estilos genéricos que no pertenezcan a un componente en particular.

Dentro de `components` vamos a crear una carpeta por cada componente, con nombre en `PascalCase`, como el mismo nombre del componente, y dentro de esto vamos a crear al menos un archivo `.js` para el componente, y un archivo `.scss` para los estilos, también con el mismo nombre. Por ejemplo, si nuestro componente se llama `Comment`, nos quedaría:

 - src
    - components
        - Comment
            - Comment.js
            - Comment.scss
  
En primera instancia ponemos todos los componentes bajo la carpeta `components`, a un mismo nivel. En caso de que luego la cantidad de componentes empiece a crecer, podemos empezar a agruparlos dentro de subcarpetas. Básicamente, los criterios para agrupar componentes son dos:

  - Componentes de un mismo tipo: buttons, modals, popups, menus
  - Componentes que pertenezcan a una misma unidad o componente mayor: CommentText, CommentDate, CommentUsername, etc
  
Dentro de `styles` vamos a incluir archivos `.scss` que van a ser luego importados en los archivos `.scss` de los componentes. Estos archivos tienen por lo general estilos generales o variables que se utilizan en todo el proyecto. Como tienen que ser importados, son *archivos parciales*, por lo que tienen que comenzar su nombre con guión bajo. Es preferible tener varios archivos (por ejemplo, uno para variables de colores, otro para variables de fuentes, etc), a un único archivo gigante. A su vez, en el caso de tener varios, también podemos agruparlos en subcarpetas si lo consideramos necesario.   


## Export e import

Una de las nuevas versiones de JS (ES6) trae la posibilidad de exportar e importar código de un archivo a otro. Esto significa que desde un archivo X podemos exportar (o poner pública, o a disponibilidad) un valor Y, y cuando un archivo Z importa ese archivo X, puede obtener y trabajar con el valor Y.

Hay dos formas de exportar, **nombrada** y **por defecto**, y cada una tiene su sintaxis. En un archivo se pueden tener varias exportaciones de forma nombrada, pero una sola por defecto.

*Siempre que tenemos un componente de React necesitamos tener un export por default, pero esto no implica que en todo archivo .js necesitemos tener un export por default* 

### Export por default

Para exportar por default un valor, simplemente tenemos que poner las palabras reservadas `export default` y el valor que queramos exportar. Se pueden exportar todo tipo de valores (números, strings, booleanos, arrays, objetos, funciones)

```javascript
export default 3
```

También podemos exportar una variable que (que en el export se reemplaza por el valor que contiene):

```javascript
const miVariable = 3
export default miVariable
```

O directamente todo en una misma línea:

```javascript
export default miVariable = 3
```

Cuando hacemos esto último no hay que declarar la palabra `const` o `let` de la variable. En el caso de un componente, podemos crear el componente y luego exportarlo:

```javascript
const MiComponente = () => (<></>)

export default MiComponente
```

o hacer todo junto:

```javascript
export default MiComponente = () => (<></>)
```

Para importar el valor exportado en otro archivo, necesitamos poner:

```javascript
import miVariable from 'ruta/al/archivo'
```

Y ya podemos usar `miVariable` dentro de dicho archivo, que contendrá el valor exportado por defecto. Como el export por default no tiene un nombre (puede ser solo un valor) y es único por archivo, podemos al importarlo asignarlo en una variable con otro nombre, pero a la hora de trabajar con componentes en React esto no es una práctica recomendable.


La ruta al archivo puede ser una ruta absoluta (dependiendo de cómo tengamos configurado nuestro entorno), o una ruta relativa, donde `./` indica la posición en la carpeta actual, y `..` se utiliza para subir un nivel hacia arriba. Cuando importamos archivos .js, **no hace falta incluir la extensión del mismo** 

<br />

### Export con nombre

Para exportar un valor con nombre, tenemos que declararlo en una misma línea, junto a la declaración de la variable donde va a estar contenido, por ejemplo:

```javascript
export const miVariable = 3
export const miOtraVariable = 5
```

O podemos utilizar `destructuring` (ver más adelante) para exportarlos en otra línea:

```javascript
const miVariable = 3
const miOtraVariable = 5
export {miVariable, miOtraVariable}
```

A la hora de importar, tenemos dos opciones:

1. Importar todo en una variable, y acceder a las distintas variables como propiedades del objeto:

```javascript
import * as misVariables from 'ruta/al/archivo'

console.log(misVariables.miOtraVariable)
```

2. Utilizar destructuring, para declarar las cosas que necesitamos importar y utilizarlas directamente

```javascript
import {miVariable, miOtraVariable} from 'ruta/al/archivo'

console.log(miVariable)
```

Para esto necesitamos utilizar exactamente el mismo nombre con el que exportamos el valor, pero podemos ponerle un *alias*:

```javascript
import {miVariable as miVariableConAlias} from 'ruta/al/archivo'

console.log(miVariableConAlias)
```

A su vez, podemos importar los valores por default y con nombre en una misma línea:

```javascript
import ValorPorDefault, {miVariable, miOtraVariable} from 'ruta/al/archivo'
```

En cuanto al orden de las importaciones, es preferible importar todo en la parte superior del archivo. Primero se importan las librerías externas o de terceros, luego los componentes externos o de terceros, luego los componentes propios, y luego otros archivos que necesitemos (estilos, etc).


## Componentes funcionales

Un componente no es más que una función que devuelve un conjunto de elementos HTML (u otros componentes que a su vez devuelven elementos HTML). Para definir un componente necesitamos:

1. Importar la librería React
2. Crear nuestra función/componente con nombre **comenzando en mayúscula** (PascalCase), ya que React lo necesita para diferenciar componentes de elementos HTML (`Button` es distinto de `button`)
3. Dicho componente debe devolver **al menos un elemento** HTML (u otro componente)
4. Los componentes u elementos que devuelva deben estar englobados en un **único elemento padre/raíz**
5. Exportar como **default** nuestro componente

Lo mínimo e indispensable que necesitamos para tener un componente funcional en React es lo siguiente:

```javascript
import React from 'react'

const MiComponente = () => (
  <></>
)

export default MiComponente
```


## JSX

JSX (JavaScript extension) es una extensión del lenguaje JavaScript. Cuando escribimos JSX, si bien parece que estamos escribiendo HTML, en realidad es código JSX, que se traduce luego en JS. Por ejemplo, el código:

```javascript
<div>Hola JSX</div>
```

se termina traduciendo en

```javascript
React.createElement('div', {}, 'Hola JSX')
```

Por esto es que necesitamos importar la librería React en la constante `React` cuando utilizamos JSX en un archivo.

Si bien JSX es muy similar en sintaxis a HTML, hay algunas diferencias:

1. Principalmente, podemos usar etiquetas que no corresponden a elementos HTML, sino a componentes de React. Estos se distinguen de los primeros en que su nombre comienza con letra mayúscula, por ejemplo, `<Button />` representa un componente de React mientras que `<button />` representa el elemento HTML. 
2. Las etiquetas pueden ser abiertas (`<div />`) o cerradas (`<div></div>`)
3. Los atributos van en *camelCase*, por ejemplo `<div onClick />`
4. Algunos atributos tienen nombre distinto al de que se utiliza en HTML, por ejemplo `className` en vez de `class`
5. Como eso se compila a código JS, cada línea se termina autocompletando con un punto y coma, por lo tanto, si ponemos:

```javascript
return
  <div />
```

esto se termina transformando en:


```javascript
return;
  React.createElement('div', {}, null);
```

por lo que la instrucción de crear el elemento de React no se ejecutaría nunca, ya que el `return` corta el flujo de la instrucción. Por eso si queremos utilizarlo en una nueva línea, tenemos que encerrarlo dentro de un paréntesis, de esta forma, el compilador sabe que tiene que esperar a cerrar el paréntesis para poner el punto y coma:

```javascript
return (
  <div />
)
```

Como el compilador tiene que saber diferenciar código JS de JSX, tenemos que identificar de alguna forma cuándo estamos escribiendo uno u otro código. Para esto, utilizamos las llaves `{}` para definir que lo que va dentro de estas es código JS **cuando estamos dentro de código JSX**. Siempre es importante fijarse dentro de qué contexto estamos, porque dentro de contexto de código JS, `{}` representan un objeto. Por ejemplo:

```javascript
<div>
  {
      // Contexto JS
  }
</div>
```

Dentro de las llaves, podemos poner toda instrucción válida de JS que *devuelva* un valor, por ejemplo, un valor puro (un número, un booleano, un array, un objto), una variable, una función, un operador ternario, etc. A su vez, dentro de un contexto JS, podemos anidar elementos JSX que a su vez tengan llaves y generen un nuevo contexto JS, por ejemplo:

```javascript
<div>
  {
      // Contexto JS
      <div index={1} />
  }
</div>
```

Una expresión muy utilizada, y que suele confundir, es cuando usamos un objeto dentro de una expresión JSX. En ese caso, se utiliza doble llaves, donde las primeras llaves indican que lo que viene a continuación es código JS, y las segundas llaves representan el objeto en sí:

```javascript
<div style={{ color: 'red' }} />
```
Toda expresión JSX debe necesariamente tener un único elemento raíz o padre, es decir, estar contenida dentro de un elemento (o componente) principal. Por ejemplo, esto no es código JSX válido:

```javascript
<div />
<div />
```

mientras que esto sí:


```javascript
<div>
  <div />
  <div />
<div />
```

dado que los `divs` están encerrados dentro de un único elemento `div`. Para evitar estar poniendo (y renderizando) `divs` innecesarios, React ofrece un componente llamando `Fragment`, que se utiliza para estos caso (encerrar un conjunto de elementos o componentes hermanos), y que no se renderiza en el DOM. Se puede utilizar de forma explícita:

```javascript
<React.Fragment>
  <div />
  <div />
</React.Fragment>
```

o en su forma abreviada, utlizando las etiquetas de apertura y cierre `<></>`

```javascript
<>
  <div />
  <div />
</>
```


## Estilando un componente

React no determina una única forma de darle estilos a un componente, por lo que existen varias técnicas y herramientas para hacerlo. En nuestro caso, vamos a usar dos: una para estilos más "estáticos" o fijos, y otra para estilos que tengan que ser calculados dinámicamente.

Para aquellos estilos que no varíen (más allá de alguna transición, animación o cambio de clase), es decir, para estilos más fijos, vamos a declararlos en un archivo `.scss` propio del componente, que está dentro de la misma carpeta y tiene el mismo nombre que el componente.

Por ejemplo:


```javascript
// MiComponente.js
import React from 'react'
import './MiComponente.scss'

const MiComponente = () => (
  <div className='mi-clase' />
)

export default MiComponente
```

```scss
/** MiComponente.scss */
.mi-clase {
  /** Estilos particulares del componente */
}
```


Dentro del `.scss`, podemos aprovechar todas las ventajas que nos ofrece Sass, por ejemplo, selectores anidados y variables:



```javascript
// Comment.js
import React from 'react'
import './Comment.scss'

const Comment = () => (
  <div className='comment'>
     <p className='comment-title'></p>
     <p className='comment-text'></p>
  </div>
)

export default Comment
```

```scss
/** _variables.scss */
$main-color: black;
$secondary-color: grey;
```

```scss
/** Comment.scss */
@import 'styles/variables'

.comment {
  .comment-title {
    color: $main-color;
  }
  
  .comment-text {
    color: $secondary-color;  
  }
}
```


**Es buena práctica que los nombres de las clases de un componente sean lo más específicas posible, para evitar conflictos con otros estilos.** Por ejemplo, es mejor esto:



```javascript
// Comment.js
import React from 'react'
import './Comment.scss'

const Comment = () => (
  <div className='comment'>
     <p className='comment-title'></p>
     <p className='comment-text'></p>
  </div>
)

export default Comment
```

a esto:

```javascript
// Comment.js
import React from 'react'
import './Comment.scss'

const Comment = () => (
  <div className='comment'>
     <p className='title'></p>
     <p className='text'></p>
  </div>
)

export default Comment
```


Podemos aprovechar una ventaja interesante de Sass, los **mixins** que son porciones de estilos reutilizable, para darles estilos que se utilizan en nuestra aplicación seguido a distintos componentes. El mixin se declara con la siguiente sintaxis:



```scss
/** _buttons.scss */
@mixin primary-button {
  background-color: 'green',
  border-radius: 5px;
}
```

y se utiliza así:

```scss
/** MiComponente.scss */
@import 'styles/buttons';

.mi-boton {
  @include primary-button;
}
```



Por otro lado, dentro de nuestro componente, podemos definirle *a un elemento HTML* estilos con JS. Para esto, utilizamos la propiedad `style`, que toma como valor un objeto, cuyas propiedades son las propiedades de CSS en `camelCase`. Por ejemplo:

```javascript
const Comment = () => (
  <div className='comment' style={{ backgroundColor: 'red' }}/>
)
```

Como un componente es un conjunto de elementos HTML y/o otros componentes, aplicarle un estilo a un componente en general no tiene sentido, porque React no sabe a cuál de todos los que lo componen debería asignarle esos estilos. Por lo tanto, los estilos sólo se aplican a elementos HTML. Definir los estilos dentro de nuestro componente permite que, dado que es código JS, podamos utilizar expresiones y variables para calcular el valor de una propiedad. 



## Props

Un componente es una función que devuelve un conjunto de elementos HTML u otros componentes. Ahora bien, si tenemos una función que devuelve siempre el mismo valor, en la mayoría de los casos no resulta una función muy útil. Además, necesitaríamos una función por cada valor distinto que queremos obtener. Por ejemplo, para obtener los múltiplos de 2, necesitaríamos:

```javascript
const multiplicar2Por3 = () => 2 * 3
const multiplicar2Por4 = () => 2 * 4
// etc
```

La idea de una función es que *le pasemos* ciertos valores (mediante parámetros), con esos valores *realice ciertas operaciones*, y devuelva un resultado. De esta forma la función se vuelve genérica y reutilizable, puede aceptar diferentes valores y devolver un resultado dependiendo de estos. Refactorizando los ejemplos anteriores, nos queda siguiente la función, que ahora acepta cualquier número y nos devuelve su doble:

```javascript
const multiplicarPor2 = x => x * 2
```

Lo mismo pasa con los componentes. En vez de tener dos componentes distintos:

```javascript
const BotonEnviar = () => (
  <button>Enviar</button>
)

const BotonCancelar = () => (
  <button>Cancelar</button>
)
```

dado que son iguales (devuelven un elemento `button`) y lo único que cambia es un cierto valor, podemos abstraer este valor en una variable, y tendríamos algo así como:

```javascript
const Boton = () => (
  <button>{texto}</button>
)
```

es decir, un componente que devuelve un elemento `button` con un cierto texto X, independientemente de cuál sea este texto. Ahora bien, de dónde podemos obtener esta variable `texto`?

React permite hacer esto mediante las **props.**

Las props son todos aquellos "atributos" custom que se pueden definir en un componente. Por ejemplo, para el siguiente componente:

```javascript
<Usuario nombre='Ada' apellido='Lovelace' pais='Inglaterra' />
```

sus props son `nombre`, `apellido` y `pais`. Dentro de un componente, podemos definir los props que querramos (aunque cuanto menos, mejor), ponerle los nombres que deseemos (mientras esten en `camelCase`) y a estos asignarle cualquier valor que necesitemos.

React arma con todos estos "atributos" un objeto, donde cada uno de ellos es una propiedad, y lo pasa como parámetro de la función/componente que invoca. Es decir, para el caso anterior, React armaría un objeto así:

```javascript
const props = {
  nombre: 'Ada',
  apellido: 'Lovelace',
  pais: 'Inglaterra'
}
```

y llamaría internamente a la función/componente con ese objeto como parámetro, algo así:

```javascript
Usuario(props)
```

Por lo tanto, si queremos dentro de un componente acceder a esos valores, necesitamos declarar el parámetro primero para que este pueda ser "inyectado". Luego, teniendo en cuenta que es un objeto, dentro de nuestro componente podemos acceder y hacer uso de sus propiedades para lo que necesitemos:

```javascript
const Usuario = props => (
  <div>
    <h1>{props.nombre}</h1>
    <h2>{props.apellido}</h2>
    <small>{props.pais}</small>
  </div>
)
```

De esta manera, nos queda un componente genérico que acepta una serie de propiedades como parámetros, y devuelve un conjunto de elementos HTML con los valores de estas propiedades. Así, podemos reutilizar el componente, pasándole distintos valores:


```javascript
<Usuario nombre='Ada' apellido='Lovelace' pais='Inglaterra' />
<Usuario nombre='Grace' apellido='Hopper' pais='EEUU' />
```

Como los props son valores que se guardan dentro de las propiedades de un objeto, podemos utilizarlos no solo para definir el contenido, sino para otras cosas que nos permiten configurar y personalizar aún más cada componente, como determinar estilos, seleccionar opciones, definir comportamientos, etc.

Por otro lado, para dentro del componente evitar estar escribiendo `props` todo el tiempo, podemos aprovechar las ventajas de *destructuring* para hacer lo siguiente:

```javascript
const Usuario = ({nombre, apellido, pais}) => (
  <div>
    <h1>{nombre}</h1>
    <h2>{apellido}</h2>
    <small>{pais}</small>
  </div>
)
```
Así "desarmamos" el objeto **props** es sus distintas propiedades. Esto limpia mucho el código, y además nos permite reconocer inmediatamente cuáles son las propiedades que nuestro componente utiliza, en vez de tener que estar buscándolas por todo el código.

**No existe una cantidad máxima de props que puede tener un componente, pero en líneas generales, más de 5 props se considera mucho y llegado ese caso es recomendable analizar si ese componente no puede ser dividido en otros más pequeños.**



## Componentes hijos

Cuando definimos un componente, tenemos la posibilidad de hacerlo de forma abierta o cerrada. Si lo hacemos con las etiquetas de apertura y cierre, podemos anidar componentes adentro. Por ejemplo:

```javascript
<Header>
  <Title />
  <Subtitle />
  <Date />
</Header>
```

Ahora bien, dónde se renderizan estos componentes anidados? Un componente es un conjunto de elementos y/o componentes (por más que esté compuesto por uno, se considera un conjunto), por lo tanto React no sabe ni puede decidir dentro de cuál de todos esos elementos debe insertarlos.

Para que dichos componentes se muestren, debemos especificar en qué lugar queremos que se renderizen. Los componentes anidados o hijos, se pasan al elemento padre de la propiedad `children` de los `props`, por lo tanto, dentro del elemento padre, tenemos que definir dónde queremos que se incluyan:

```javascript
const Header = ({children}) => (
  <div className='header'>
    <Logo />
    {children}
  </div>
)
```

Esta técnica permite también tener componentes más reutilizables, ya que pueden contener distintas combinaciones de otros componentes, y no siempre la misma.


## Listas de componentes

Algo muy usual en toda interfaz en tener una lista de componentes del mismo tipo. Una lista no necesariamente tiene que representarse visualmente como tal, pero conceptualmente y jerárquicamente, dichos componentes están agrupados y en el mismo nivel, uno seguido de otro. Puede ser una lista de botones, de comentarios, de noticias, de submenus, de imagenes, de emojis, etc. Se pueden mostrar de forma horizontal, vertical, en grilla, o de cualquier otra disposición. Una lista sigue siendo una lista independientemente de cómo se visualice.

En React una lista es un **array de componentes del mismo tipo**. Por lo general, la cantidad de componentes de una lista es dinámica, es decir, no sabemos de antemano cuántos componentes va a tener, e incluso puede ir variando. Si tenemos una cantidad de componentes fijos y conocemos su cantidad, a menos que sean muchos, conviene ponerlos de forma explícita normalmente como haríamos con cualquier componente, por más que estén repetidos.

Una tarea usual que se necesita hacer en casi toda aplicación, es generar una cantidad de componentes X dada cierta información (que puede venir de una base de datos, de una API, que la ingresó el usuario, etc). Por ejemplo, si tenemos un anotador, a medida que el usuario va agregando notas, la información de esas notas se va guardando, y luego con esos datos, tenemos que generar un componente de nota por cada uno de ellos.

Dicha información, como estamos hablando de un conjunto de cosas, es común encontrarla en la forma de un array de objetos, donde cada objeto representa el conjunto de datos de uno de los elementos en particular. Por cada uno de estos objetos, tenemos que generar un nuevo componente con dicha información.

Dado que tenemos un array, y por cada ítem tenemos que generar un componente, podemos pensarlo como que tenemos que *mapear* cada ítem (objeto) a un componente que contenga dicha información en específico. Por lo tanto, podemos utilizar para esto el método `map`.

```javascript
const ListaComentarios = ({comentarios}) => (
  <>
  {
    comentarios.map(comentario => 
      <Comentario 
        texto={comentario.texto}
        usuario={comentario.usuario}
        fecha={comentario.fecha}
      />)
  }
  </>
)
```

De esta forma, con `map` recorremos el array de objetos, y por cada objeto, ejecutamos un callback. Dicho callback toma como parámetro el objeto que se está recorriendo, y devuelve un componente con la información de dicho objeto. Ese componente se guarda en un nuevo array, por lo que al finalizar su recorrido, `map` devuelve un array de componentes.

Cuando laburamos con listas de componentes (arrays de componentes, y en la mayoría de los casos, cuando los generamos mediante esta técnica utilizando `map`), tenemos que definirle a los ítems de la lista una propiedad propia de React, llamada `key`. Esta propiedad React la utiliza para diferenciar los elementos de una lista, y cuando esta lista cambia, saber de todos esos elementos cuál se modificó. Por lo tanto, la key tiene que ser un identificador único y diferente al de los demás elementos de la lista. Por lo general se utiliza algún `id` que venga ya en la información: 

```javascript
const ListaComentarios = ({comentarios}) => (
  <>
  {
    comentarios.map(comentario => 
      <Comentario
        key={cometario.id}
        texto={comentario.texto}
        usuario={comentario.usuario}
        fecha={comentario.fecha}
      />)
  }
  </>
)
```

*En última instancia podemos utilizar un índice*, aunque esto está desanconsejado, es preferible a no declarar ninguna `key` cuando no tenemos otro dato.

```javascript
const ListaComentarios = ({comentarios}) => (
  <>
  {
    comentarios.map((comentario, index) => 
      <Comentario
        key={index}
        texto={comentario.texto}
        usuario={comentario.usuario}
        fecha={comentario.fecha}
      />)
  }
  </>
)
```

## Condicionales en JSX

Dado que dentro de código JSX podemos insertar expresiones de JS (dentro de llaves), podemos aprovecharlo para definir el valor de los props de componentes y elementos HTML. Para esto se utiliza el **operador ternario**, ya que la ventaja que tiene es que *devuelve* un valor, es decir, toda la expresión se reemplaza por el valor devuelto dependiendo de cómo se valide la condición.

La sintaxis del operador ternario es la siguiente:

```javascript
condicion ? valorSiTrue : valorSiFalse
```

Donde `condicion` es una expresión de JS que devuelve un *booleano* (por ejemplo una comparación). En el siguiente caso:

```javascript
3 > 5 ? 'Es mayor' : 'Es menor'
```

el resultado devuelto es `'Es menor'` porque la condición resuelve en `false`.

Esto podemos usarlo de la siguientes formas:

1. Para dar estilos:

```javascript
const PopUp = ({tipo}) => (
  <div style={{ backgroundColor: tipo === 'importante' ? 'red' : 'gray' }}/>
)
```

Acá le estamos asignando al `div` un `backgroundColor` dependiendo del valor que tenga el `prop` `tipo`, si es `'importante'`, será `'red', si es cualquier otro valor (no importante), será `'gray'`.


2. Para agregar o definir clases:


```javascript
const Boton = ({tipo}) => (
  <div className={tipo === 'importante' ? 'button-important' : 'button-normal'} />
)
```

En este caso, si la `prop` `tipo` tiene el valor `'importante'`, al `div` se le agregará la clase `'button-important'`, sino, la clase `'button-normal'`


## Personalizando un componente

Si bien no es una regla estricta (y depende mucho de cada componente), por lo general es una buena idea y una buena práctica darle a quien lo usa (ya seamos nosotras o alguien más) la posibilidad de personalizar o customizar un componente. Esto incrementa su reusabilidad y su vida de duración, ya que podemos reutilizar un mismo componente en distintos lugares especificándole las opciones que nos interesan, en vez de tener que estar creando nuevos componentes para cada versión distinta que necesitemos.

Como la mayoría de las cosas en React, no hay una línea fija en la que trazar hasta cuánto debería poder ser personalizable un componente y qué tanto debería poder cambiar, pero esto debería tenerlo en cuenta con el consejo de que no es lo ideal tener muchos props. Por lo tanto, no dependiendo del componente, no deberíamos tener más de 2 o 3 opciones para personalizarlo, si ya necesitamos, es probable que es componente esté intentando abarcar muchas cosas y pueda ser dividido en subcomponentes.

Un componente puede ser personalizado de varias formas: forma, estructura, comportamiento, disposición, etc. En este caso vamos a enfocarnos exclusivamente en los estilos.

## Clases personalizadas

Una opción posible, la más flexible y laxa, es darle la posibilidad a quien usa nuestro componente de agregarle clases extras. De esta forma, puede darle los estilos que desee y sobreescribir los propios del componente si lo considera necesario.

Si tenemos un componente y le agregamos una clase, por ejemplo así:

```javascript
<PopUp className='importante' />
```

En React esto no tiene ningún efecto. Un componente es un *conjunto* de elementos y/o componentes, por lo que aplicarle una clase a un conjunto no tiene sentido. React no sabe a cuál de todos esos componentes queremos agregarle dicha clase, y no puede decidir por nosotras. Por lo tanto, **en un componente el `className` se pasa como prop, y no se asigna como clase**. Solo a elementos HTML podemos asignarle `className`. Lo mismo pasa con la propiedad `style`.

No obstante, podemos aprovechar que eso se pasa como prop, y concatenarlo o agregarlo a algún elemento HTML de nuestro componente, por ejemplo:

```javascript
const PopUp = ({className}) => (
  <div className={`pop-up ${className}`} />
)
```

De esta forma, le estamos agregando la clase extra que se le defina al componente. El problema con esto es que si no se le define una clase, `className` llega como `undefined`, por lo tanto nuestro `div` nos quedaría con las clases `pop-up undefined`. Para evitar esto, podemos aprovechar los **parámetros por default**, una *feature* de JS ES6, que permite asignar un valor a un parámetro cuando este no está definido. Esto se declara asignándole al parametro el valor por default que queremos que tenga, mediante el símbolo `=`:

```javascript
const PopUp = ({
  className = ''
}) => (
  <div className={`pop-up ${className}`} />
)
```

Si el componente `PopUp` se le declara una `className`, se agregará dicha clase a la clase `pop-up` del div, si no, no se le agregará nada.

## Opciones personalizadas

Si tenemos ciertos estilos y variantes que se repiten mucho, y queremos evitar estar teniendo que declarar una clase extra cada vez que necesitamos modificarlos, o no queremos darle tanto control sobre los estilos de nuestro componente a quien lo usa, podemos definir un conjunto de opciones de las cuales escoger personalizar el componente.

La idea es básicamente la siguiente:

  1. Se define un objeto de configuración cuyas propiedades son las posibles opciones que se pueden elegir (el objeto representa la categoría de esas opciones, puede que necesitemos varios objetos)
  2. Cada propiedad/opción tiene asignado un valor que representa una clase
  3. Se exporta, junto con el componente, dicho objeto de configuración
  4. Cuando se utiliza el componente, se define algún prop cuyo valor sea alguna propiedad de este objeto
  5. En nuestro componente, agregamos el valor de dicho prop como clase extra
  6. Se definen estilos generales para nuestro componente y los estilos que pertenecen a cada clase/variación posible
  
```javascript
export const TIPO_BOTON = {
  PRINCIPAL: 'primary',
  SECUNDARIO: 'secondary',
  DEFAULT: 'default',
  IMPORTANTE: 'important'
}

const Boton = ({
  tipo = TIPO_BOTTON.DEFAULT
}) => ( 
  <div className={`button ${tipo}-button`} />
)

export default Boton
```
```javascript
import Boton, {TIPO_BOTON} from 'boton'

<Boton tipo={TIPO_BOTON.PRIMARIO} />
```
```scss
.button {
  width: 200px;
  height: 100px;
  
  &.button-primary {
    background-color: green
  }
  &.button-secondary {
    background-color: blue
  }
  &.button-default {
    background-color: gray
  }
  &.button-important {
    background-color: red
  }
}

```

De esta forma, al utilizar las propiedades de un objeto, tenemos el autocomplete de nuestro editor que nos indica cuáles son las opciones disponibles, y es mucho menos probable que le erremos, además de evitarnos tener que buscar en el código cuáles son las posibles.

La sintaxis con mayúscula y guión bajo que usamos para el objeto de configuración no es obligatoria, pero es una convención que se utiliza para indicar que dicho objeto es de solo lectura, es decir, que no debe ser modificado. En JS no podemos hacer esto fácilmente, por lo tanto esto es una forma de advertirlo de manera visual, además de ser una convención que se utiliza en otros lenguajes. A este tipo de objetos de solo lectura se los conoce como **enums**.

En el `sass` aprovechamos el signo `&`, que indica el elemento padre o raíz, para definirle estilos cuando las dos clases se apliquen a un elemento.  


## Iconos

Para utilizar íconos de FontAwesome al estilo de React, tenemos que instalar las siguientes dependencias:

```terminal
npm install @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons @fortawesome/free-regular-svg-icons @fortawesome/react-fontawesome
```

Cada vez que queramos mostrar un ícono, tenemos que utilizar el componente `FontAwesomeIcon`

```javascript
import React from 'react'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'

// ... en alguna parte de algún componente
<FontAwesomeIcon icon={icon} />
// ...
```

El componente tiene una propiedad `icon` obligatoria, en la cual tenemos que declarar que ícono va a estar mostrando. Este ícono tenemos que importarlo de la librería `@fortawesome/free-solid-svg-icons` (íconos sólidos) o de `@fortawesome/free-regular-svg-icons` (íconos normales). No todos los íconos están en ambas. Los nombres de los íconos empiezan con fa (Font Awesome) más el nombre (que se puede buscar en la página de Font Awesome), todo en `camelCase`.  

```javascript
import React from 'react'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { faCoffee, faHome } from '@fortawesome/free-solid-svg-icons'

// ... en alguna parte de algún componente
<FontAwesomeIcon icon={faCoffee} />
<FontAwesomeIcon icon={faHome} />
// ...
```

Si tenemos algún componente que tenga un ícono de Font Awesome, pero queremos que ese ícono sea personalizable, podemos pasarlo como prop de ese componente:

```javascript
import React from 'react'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'

const Button = ({icon, text}) => (
  <FontAwesomeIcon icon={icon} />
  {text}
)
```

y en algún lado donde necesitemos invocar ese componente:

```javascript
import React from 'react'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'

<Button icon={faCoffe} />
```


## Renderizado condicional

El renderizado condicional es una técnica que permite mostrar componentes dependiendo de una cierta condición (por lo general, una comparación, o el valor de una variable). La diferencia entre esto y ocultarlo mediante estilos, es que el elemento *no se renderiza*, es decir, directamente no se crea en el DOM, lo que aumenta la performance. En cambio si lo hacemos con estilos va a seguir existiendo, nada más que escondido. Existen varias estrategias para realizarlo:

## if/else

Util para cuando queremos evitar que se renderize un o elemento/componente. La idea es en el cuerpo de la función, chequear una cierta condición. Si se cumple, devolvemos `null`, lo que impide que se siga ejecutando la función. De lo contrario, continúa con su flujo normal y termina devolviendo el componente a renderizar.

Se puede usar if/else if/else para ir chequeando y mostrando diferentes componentes según ciertas condiciones, pero *esto no es recomendable* porque la sintaxis se vuelve muy densa y complicada de leer, además de que se oscurece la lógica del componente. Para estos casos, es mejor usar la técnica del enum.

```javascript
const Modal = ({isVisible}) => {
  if (!isVisible)
    return null
    
  return (
    <div className='modal' />
  )  
}
```

## operador ternario

Util cuando queremos alternar entre dos componentes dependiendo de si se cumple una condición. La sintaxis es:

`condicion ? <ComponenteSiSeCumple /> : <ComponenteSiNoSeCumple />`

como esto es código JS, tenemos que encerrarlo entre llaves si lo estamos incluyendo dentro de código JSX. Por ejemplo:

```javascript
const Modal = ({isUser}) => (
    <div className='modal'>
    {
      isUser ?
      <LogInForm /> :
      <SignInForm />
    }
    </div>
)
```

## operadores lógicos

Cuando JS encuentra una serie de operadores lógicos, no lee toda la expresión entera. Por ejemplo, si tiene la siguiente expresión:

```javascript
valor1 && valor2 && valor3 && valor4
```

si `valor1` es `true`, va a pasar a evaluar `valor2`. Pero si esta es `false`, como toda la expresión resulta falsa independientemente de los valores que tengan las otras variables, estos no pasan a evaluarse.

Si en vez de tener variables, tenemos funciones:

```javascript
condicion && miFuncion()
```

`miFuncion` se ejecuta solo si `condicion` es verdadera, porque si es falsa, no importa el valor que tenga el resto de la expresión, va a seguir siendo falsa y JS no lo evalúa.

De la misma manera, si pensamos que internamente cuando estamos escribiendo JSX en realidad estamos llamando a funciones, si tenemos:

```javascript
condicion && <MiComponente />
```

`MiComponente` sólo va a mostrarse (ejecutarse) si `condicion` es válida. Por lo tanto, podemos utilizar esto dentro del JSX de un componente:

```javascript
const Banner = ({isSubscribed}) => (
  <Container>
    <Imagen />
    <Form />
    {
      isSubscribed &&
      <BannerDiscount />
    }
  </Container>
)
```

En este caso, `BannerDiscount` sólo va a renderizarse si `isSubscribed` es verdadero.

## enums

La idea es usar un objeto de configuración (enum) donde cada propiedad tiene como valor un componente JSX, y poder elegir (por lo general, mediante props) cuál de todas esas opciones utilizar, y renderizar el componente que contenga.

```javascript
const NOTIFICATION_STATES = {
  info: <Info />,
  warning: <Warning />,
  error: <Error />,
};

const Notification = ({ state }) => (
  <div>
    {NOTIFICATION_STATES[state]}
  </div>
);
```

aprovechando la notación de corchetes, podemos elegir la propiedad del objeto de forma dinámica, y dependiendo del valor que tenga el prop `state`, obtener el componente que corresponde y renderizarlo. Esto se usaría de la siguiente manera:

```javascript
<Notification state="warning" />
```


## Componentes de clase


Los componentes de clase son otra forma de declarar componentes en React. Nos permiten generan componentes más dinámicos que pueden cambiar y manipular su estado interno. Un componente puede ser funcional o de clase, no puede ser los dos a la vez. Lo mejor es siempre empezar con un componente funcional (porque es más simple y liviano), y si lo necesitamos, convertirlo a componente de clase.

Los pasos para pasar un componente funcional a uno de clase son los siguientes:

1. Crear una clase con el nombre del componente
2. Extender dicha clase de la clase `React.Component`
3. Agregarle un método `render` a la clase
4. Dentro de dicho método, poner un `return()`
5. Dentro de los paréntesis del `return` poner todo el JSX que estaba en el componente funcional

Por ejemplo, supongamos que tenemos el siguiente componente:

```javascript
import React from 'react'
// ...otros imports

const PopUpSubscription = () => (
  <PopUp>
    <EmailForm />
    <Button type="submit" />
  </PopUP>
)

export default PopUpSubscription
```

y queremos pasarlo a un componente de clase. 

#### 1. Crear una clase con el nombre del componente

```javascript
class PopUpSubscription {

}
```

#### 2. Extender dicha clase de la clase `React.Component`


```javascript
class PopUpSubscription extends React.Component {

}
```

#### 3. Agregarle un método `render` a la clase


```javascript
class PopUpSubscription extends React.Component {
  render(){
  
  }
}
```

#### 4. Dentro de dicho método, poner un `return()`

```javascript
class PopUpSubscription extends React.Component {
  render(){
    return(
    
    )
  }
}
```

#### 5. Dentro de los paréntesis del `return` poner todo el JSX que estaba en el componente funcional

```javascript
class PopUpSubscription extends React.Component {
  render(){
    return(
      <PopUp>
        <EmailForm />
        <Button type="submit" />
      </PopUP>
    )
  }
}
```

Esto finalmente nos quedaría así:

```javascript
import React from 'react'
// ... otros imports

class PopUpSubscription extends React.Component {
  render(){
    return(
      <PopUp>
        <EmailForm />
        <Button type="submit" />
      </PopUP>
    )
  }
}

export default PopUpSubscription
```

Esto es lo mínimo que necesitamos para tener un componente de clase funcionando. Un componente de clase tiene que tener sí o sí un método `render`, y dicho método tiene que devolver JSX válido.


## props

Los props son pasados internamente por React cuando crea un componente funcional, por lo tanto, en cualquier parte de la clase tenemos acceso a estos mediante `this.props`. Si queremos usarlo por ejemplo en el método `render`, por ejemplo, podríamos hacer esto:

```javascript
class Button extends React.Component {
  render(){
    return(
      <div>{this.props.text}</div>
    )
  }
}
```

También podemos *desestructurar* los props para que queden de forma más visible y sean fácilmente reconocibles, además de permitirnos usarlos sin `this.props`

```javascript
class Comment extends React.Component {
  render(){
    const {title, text, username, date} = this.props
    return (
      <div className='comment'>
        <p>{title}</p>
        <p>{text}</p>
        <p>{username}</p>
        <p>{date}</p>
      </div>
    )
  }
}
```


## Métodos y eventos


### Métodos

Al igual que un objeto (una clase no es más que un objeto en última instancia) una clase puede tener métodos, es decir, acciones que puede realizar. Ya vimos un método que necesita todo componente, `render`, y que se encarga de devolver la lista de elementos que el componente tiene que renderizar. La sintaxis para otros métodos es la misma, dentro de las llaves de la clase, se escribe el nombre del método, paréntesis, donde van los parámentros, en caso de que los tenga, y luego llaves, donde va el cuerpo de instrucciones que se ejecuta al llamar el método:

```javascript
class MiComponente extends React.Component {
  miMetodo() {
    // acciones del metodo
  }
  render() {
    // ...
  }
}
```

A diferencia de un objeto, los métodos de una clase no se separan con coma. Esta sintaxis tiene un problema, y es que no asegura cuando se hace uso de un método, dentro de ese método, `this` refiera a la clase.

Por ejemplo, 

```javascript
class MiComponente extends React.Component {
  miMetodo() {
    console.log(this.props.name)
  }
  render() {
    return (
      <div onClick={this.miMetodo} />
    )
  }
}
```

si hacemos click en el `div de este componente, nos va a tirar que `props` no es una propiedad de `undefined`. Esto es porque el `this` del método `render` sí refiere a la clase, mientras que el del `console.log`. El problema es que `this` es una palabra **contextual**, su valor depende de dónde está siendo llamada.

Para evitar esto, tenemos que usar en los métodos la sintaxis de propiedad de clase, junto a la de funciones flecha. Es decir, asignarle una propiedad de la clase una función, es decir:


```javascript
class MiComponente extends React.Component {
  miMetodo = () => {
    console.log(this.props.name)
  }
  render() {
    return (
      <div onClick={this.miMetodo} />
    )
  }
}
```

De esta forma, `this` siempre refiere a la clase. Esto se debe a que las funciones flecha no crean su propio contexto, por lo que siempre utilizan la referncia al `this` del contexto superior desde donde son llamadas.

El método `render` no lo ponemos de esta forma y sí lo usamos con la sintaxis normal por una cuestión de performance. Este método, dependiendo el caso, puede llegar a tener que ejecutarse muchas veces. La función flecha es una función que se crea cada vez que se ejecuta, por lo tanto es una pequeña porción de memoria que ocupa y se va acumulando en caso de que haya muchas. En la práctica esto es pocas veces evidente, pero es una buena costumbre ya prevenir esto simplemente teniendo en cuenta este detalle. De esta forma, tenemos lo mejor de ambos mundos: `this` no contextual y performance.


### Eventos

Un evento es algo que se ejecuta cuando sucede una cierta acción o pasas algo. Es la forma mediante la cual el usuario interactúa con la interfaz: con clicks, touchs, movimientos de mouse, teclas presionadas, etc. Podemos definir un evento un callback que va ejecutarse dentro de elementos HTML. Los nombres por lo general son bastante similares a los que ya conocemos desde HTML, solo que en `camelCase`:

  * onClick
  * onMouseOver
  * onKeyPressDown
  * onLoad
  * etc

Para utilizarlos, tenemos que asignarlos a un elemento HTML. **Asignarlos a un componente directamente no tiene efecto**, puesto que un componente es un conjunto de elementos, React no sabe a cuál de todos esos tiene que asociar dicho evento. El callback que definimos tiene que ser la referencia a un método, o puede ser la declaración de una función en línea. Volviendo al caso anterior:


```javascript
class MiComponente extends React.Component {
  miMetodo = () => {
    console.log(this.props.name)
  }
  render() {
    return (
      <div onClick={this.miMetodo} />
    )
  }
}
```

llamamos al `this.miMetodo` como callback del `onClick` del `div`. Si queremos pasarle un parámetro, no podemos hacer lo siguiente:

```javascript
class MiComponente extends React.Component {
  miMetodo = (parametro) => {
    console.log(parametro)
  }
  render() {
    return (
      <div onClick={this.miMetodo("valor")} />
    )
  }
}
```

Porque al hacer `this.miMetodo("valor")` estamos invocando directamente la función, y esta va a ejecutarse ni bien se interpreta. Lo que tenemos que hacer es definir un callback, es decir, dar la definición de un función. Como nuestro método ya está definido, necesitamos una función que llame al mismo con su parámentro:

```javascript
class MiComponente extends React.Component {
  miMetodo = (parametro) => {
    console.log(parametro)
  }
  render() {
    return (
      <div onClick={() => this.miMetodo("valor")} />
    )
  }
}
```
Por lo tanto, esto lo hacemos solamente cuando necesitamos que nuestro callback tenga uno o más parámetros.



## Estado


En React el estado es uno de los conceptos más importantes que tenemos que manejar. De hecho, es el núcleo y la razón por la que React existe: *facilitar el manejo del estado de una aplicación*. Podemos entender al estado como el **conjunto de propiedades, valores, y datos que algo (un componente, una aplicación, etc) contiene *en un momento dado***. El estado es lo que hace *dinámica* a una aplicación, es aquello que cambia: algo está en un estado, y pasa a estarlo en otro. Si no hay cambios de estado, es porque nada se modifica.

Todo aquello que cambie en un componente, o que pueda ser alterado mediante alguna acción o interacción, forma parte del estado de ese componente. Por ejemplo, en un botón que al hacer click cambia de color, se puede decir que pasa de un estado no clickeado a uno en el que sí lo está. Cualquier cosa que necesitemos modificar dentro de un componente tiene que ser parte de su estado (u del estado de otros, como ya veremos). En React no está permitido modificar otras cosas dentro de un componente, como los props. Por ejemplo, lo siguiente no debe hacerse:

```javascript
class Modal extends React.Component {
  close = () => {
    this.props.visible = false // MAL!
  }
  render(){
    return(
        this.props.visible &&
        <div className='modal' onClick={this.close} />        
    )
  }
}
```

Cuando queremos definir un cierto estado, tenemos que agregarlo a la propiedad `state` del componente. `state` es un objeto donde cada propiedad va a representar un estado. Siguiendo el caso anterior:


```javascript
class Modal extends React.Component {
  state = {
    visible: true
  }
  close = () => {
    // ...
  }
  render(){
    return(
        this.state.visible &&
        <div className='modal' onClick={this.close} />        
    )
  }
}
```

Dentro de `state` podemos definir todas las propiedades/estados que querramos, con los nombres que querramos y los valores que deseemos. No es más que un objeto de JS.


## Inmutabilidad

Otro concepto de suma importancia a la hora de trabajar con React y con estados, es el de **inmutabilidad**. Inmutable es aquello que no puede cambiar, que no puede ser alterado. En React **el estado es inmutable**. ¿Qué significa esto? Que *no podemos modificar el estado*. En ningún sentido: no podemos sumarle un número, ni cambiarle un valor, ni agregarle una letra, ni pushear un ítem a un array, ni modificar la propiedad de un objeto. Nada. Es decir, en el caso anterior, por ejemplo, no podemos hacer lo siguiente:

```javascript
class Modal extends React.Component {
  state = {
    visible: true
  }
  close = () => {
    this.state.visible = false // MAL!
  }
  render(){
    return(
        this.state.visible &&
        <div className='modal' onClick={this.close} />        
    )
  }
}
```

¿Cómo se concilia esto con que el estado de un componente cambia y es lo que permite que sea dinámico, si no podemos alterar o mutar el estado? La regla es que **no podemos mutar el estado de un componente *directamente***. Para hacerlo, *tenemos que avisarle a React que queremos manipular el estado*. Para esto, tenemos que usar el método `setState`.

## setState

`setState` es un método de la clase `React.Component`, y nuestro componente, al extender de esta, también lo hereda. Por lo tanto, podemos accederlo como `this.setState()` en cualquier parte de nuestra clase. Este método toma como parámetro un objeto, donde sus propiedades son los nombres de aquellas propiedades del estado que queremos cambiar, y sus valores aquellos valores que queremos asignarle a dicho estado. Volviendo a nuestro caso, nos quedaría de la siguiente forma:

```javascript
class Modal extends React.Component {
  state = {
    visible: true
  }
  close = () => {
    this.setState({ visible: false })
  }
  render(){
    return(
        this.state.visible &&
        <div className='modal' onClick={this.close} />        
    )
  }
}
```

## Por qué no puedo modificar el estado directamente?

Dos razones: performance y coherencia.

  * **perfomance**: un array tiene una *id* que es lo que se conoce como **referencia**, que es la dirección en la que está guardado en la memoria. Si tenemos un array de 10000 items, React para saber si ese array fue modificado o no y actualizar el componente, tiene que recorrer los 10000 ítems y chequearlos con los valores anteriores, uno por uno. En cambio, si prohíbe hacer cambios a un array, la única forma de hacer un cambio es creando un array nuevo, por lo tanto, React sólo tiene que comparar la referencia de un array con el nuevo para saber si es el mismo array o no. Si no coinciden, es porque hubo algún cambio, y tiene que renderizar las cosas. Comparar una referencia es muchísimo más rápido que comparar todos los ítems uno por uno. 
  * **estabilidad**: al tener el control de cuándo el estado cambia, React puede actualizar los componentes que corresponden en el momento dado. En cambio, tener cosas que cambian por fuera del control hace que React tenga que estar chequeando todo el tiempo todo para ver si cambió algo (en vez de ya saber qué tiene que cambiar), y que pueda haber un desfasaje entre cosas que cambiaron y todavía no se actualizaron.

### Resumiendo

  * Todo aquello que puede cambiar en un componente, es parte del estado
  * El estado es una propiedad del componente/clase
  * El estado es un objeto cuyas propiedad son las "variables" que queremos tener
  * Al ser un objeto, sus propiedades pueden tener cualquier nombre y tener cualquier valor
  * El estado **nunca jamás se muta directamente**
  * Para modificar un estado necesitamos usar el método heredado de `React.Component`, `setState`
  * `setState` acepta como parámetro un objeto
  * Dicho objeto debe tener como propiedad los nombres de aquellas propiedades que queremos cambiar, y los nuevos valores que queremos que tengan
  
  
 
 ## setState
 
 Trabajando con arrays y objetos

Trabajar con arrays y objetos como estado tiene la peculiaridad en React de que cada vez que queremos hacer alguna modificación a alguno de estos, tenemos que *proporcionar un nuevo array u objeto*. Para esto, podemos aprovechar varios métodos y técnicas, como todos los métodos que devuelvan un nuevo elemento (`map`, `filter`, `reduce`, `slice`) y el `spread operator`

### Spread operator

El `spread operator` es una característica de ES6. La sintaxis es la siguiente:

```javascript
const numeros = [1, 2, 3, 4]
console.log(...numeros) //
```

El `spread operator` se escribe con 3 puntos (`...`) seguido del nombre del array (u objeto), y contiene todos los elementos (u propiedades) del mismo. En principio esto no parece muy útil, pero nos permite hacer cosas como la siguiente:

```javascript
const frutas = ['kiwi', 'uva']
const frutasYVerduras = ['lechuga', ...frutas, 'repollo']
console.log(frutasYVerduras) // ['lechuga', 'kiwi', 'uva', 'repollo']
```

Lo mismo con un objeto:

```javascript
const persona = {
  nombre: 'Ada',
  apellido: 'Lovelace'
}

const empleada = {
  puesto: 'desarrolladora',
  infoPersonal: {
    ...persona
  }
}

console.log(empleada) /** {
  puesto: 'desarrolladora',
  infoPersonal: {
    nombre: 'Ada',
    apellido: 'Lovelace'
  }
} */
```



### Trabajando con arrays

#### Vaciando un array

Reemplazamos el array anterior por uno nuevo y vacío

```javascript
this.setState({ frutas: [] })
```

#### Concatenando arrays

Creamos un nuevo array que contenga todos los ítems de distintos arrays

```javascript
this.setState({ comidas: [...saladas, ...dulces, ...snacks] })
```

#### Agregando un item

Creamos un nuevo array que contenga todos los ítems del array original, más el nuevo

```javascript
this.setState({ frutas: [...this.state.frutas, 'Pera'] })
```

#### Eliminando un item

Filtramos el array original buscando sólo aquellos elementos que no queremos eliminar (es decir, los que son distintos al que queremos sacer) y obtenemos un nuevo array con todos los ítems menos el que no necesitamos (se pueden eliminar más ítems agregando más condiciones)

```javascript
this.setState({ frutas: this.state.frutas.filter(fruta => fruta !== 'Manzana') })
```

También podemos utilizar *slice* si sabemos el índice del elemento que queremos extraer

```javascript
this.setState({ frutas: this.state.frutas.slice(3, 5) })
```

#### Reemplazando un item

Obtenemos un nuevo array resultado de mapear todos los elementos del array a sí mismo, excepto el que queremos cambiar, que se mapea a uno nuevo

```javascript
this.setState({ frutas: this.state.frutas.map[(fruta => fruta === 'Manzana' ? 'Pera' : fruta) })
```



### Trabajando con objetos

#### Agregando una propiedad

Creamos un nuevo objeto que contenga todos las propiedades del objeto original, más la nueva

```javascript
this.setState({ animal: {...this.state.animal, tipo: 'Perro'} })
```

#### Modificando una propiedad

Creamos un nuevo objeto que contenga todos las propiedades del objeto original, y agregamos la que queremos modificar, que si ya estaba en las anteriores se sobreescribe

```javascript
this.setState({ animal: {...this.state.animal, tipo: 'Perro'} })
```

#### Eliminando una propiedad

Desestructuramos el objeto en la propiedad que queremos eliminar y las demás, y creamos un objeto nuevo con las demás

```javascript
const {propToDelete, ...others} = this.state.myObject
this.setState({ myObject: others })
```

#### Uniendo las propiedades de varios objetos

Creamos un nuevo objeto que contenga todos las propiedades de distintos objetos

```javascript
this.setState({ usuario: {...infoPersonal, ...infoDeContanto, ...infoLaboral} })
```

<br />

### Trabajando con propiedades anidades

Si bien React no está pensado para utilizar estado anidado, y por lo tanto no es recomendable hacerlo y siempre que se pueda hay que evitarlo, hay veces que no tenemos muchas alternativas. Por lo tanto, si necesitamos actualizar alguna propiedades anidada, podemos utilizar las técnicas anteriores para ir reconstruyendo el objeto:

```javascript
this.setState({ usuario: 
  {
    ...this.state.usuario,
    personalInfo: {
      ...this.state.usuario.personalInfo,
      nombre: 'Ada'
    }
  }
})
```
 
 

## Elevando el estado

Hay veces que necesitamos "compartir" el estado de un componente, ya sea los mismo valores o los métodos que lo modifican. Cuando pasa esto, tenemos que hacer algo que en React se conoce como "elevar el estado". La idea básica es sacar el estado y los métodos de un componente, y "subirlos" a un componente superior o padre. De esta forma, este componente padre puede ahora "compartir" el estado con otros componentes hijos. Esto respeta dos filosofías muy importantes de React:

  * **Una única fuente de la verdad**: Significa que el estado de una cierta sección de una aplicación debería estar guardado en único lugar (un componente), y esa "verdad" sólo puede ser modificada desde la fuente. De esta forma, se evitan conflictos de discrepancias que pueden darse al tener distintas fuentes para una única "verdad".
  * **Flujo de datos de arriba a hacia abajo**: Implica que la información (el estado) se distribuye de los componentes hacia sus hijos. Es decir, tiene una única dirección. Esto facilita mucho a la hora de pensar en la estructura de la aplicación.
  
Entonces, un estado se encuentra en *un* componente. Cualquier cambio de ese estado sólo puede afectar a sus componentes hijos. Modificar el estado de ese componente nunca afectará a sus componentes padres o hermanos, *únicamente a sus hijos*.

Por eso, cuando necesitamos que el estado de un componente afecte a un componente hermano, tenemos que subirlo al componente padre. Ese componente puede luego pasar el estado a sus componentes hijos, por más que no haga uso propio del mismo.

Supongamos que tenemos una aplicación con los siguientes componentes

```
   .--A--. 
   |     |
.--B--.  C
|     |
D     E
```

Componente A es el componente raíz, y tiene dos hijos, B y C. B a su vez tiene dos hijos más, D y E.

Sigamos suponiendo que tenemos un cierto estado X en el componente E, del cual el componente D necesita también saber del mismo. Deberíamos elevar el estado X al componente B, porque es *el ancestro común más inmediato* entre D y E. Subirlo a A sería innecesario, por C no tiene que saber al respecto.

```
   .--A--. 
   |     |
   |     C
.--B--. <------- X debería estar acá, si
|     |    
D     E      .-- sólo D y E tienen que saber de este
^     ^      |
|_____|______|
```

No es necesario que B use el estado X, sólo necesita poder compartirlo a sus componentes hijos.

Si en cambio el componente C y el componente D necesitan saber de dicho estado, tendríamos que elevarlo al componente A.

```
   .--A--. <---- X debería estar acá, si
   |     |   
   |     C <---- sólo C y D necesitan saber de este
.--B--.      |
|     |      |
D     E      | 
^            |
|____________|
```

En este caso, A sólo derivaría el estado a sus componentes hijos, y si bien B lo recibiría, sólo se encargaría de pasarlo a el componente D.




## Elevando el estado ejemplo


Supongamos que tenemos un modal que puede abrirse y cerrarse. El componente sería más o menos así:

```javascript
class Modal extends React.Component {
  state = {
    visible: true
  }
  open = () => {
    this.setState({ visible: true })
  }
  close = () => {
    this.setState({ visible: false })
  }
  render(){
    return (
      visible &&
      <div className='modal'>
        <button onClick={this.close}>X</button>
      </div>
    )
  }
} 
```

`Modal` tiene un estado `visible` que determina si el componente se renderiza o no. A su vez, tiene los métodos `open` y `close` que controlan el cambio de estado. 

El problema que tenemos es que un modal no tiene forma de mostrarse a si mismo. No tiene ningún botón ni nada que permita ejecutar el método `open`, porque para eso, *ya tendría que estar mostrándose*.

Entonces, necesitamos que algo más, es decir, otro componente, pueda abrir nuestro modal. Supongamos que nuestra aplicación tiene la siguiente estructura:

```
        App 
         |     
         |     
   .--Container--. 
   |             |
 Modal          Button
```

En nuestro caso, queremos que el componente `Button` pueda abrir nuestro modal. Como ya mencionamos, el estado tiene que encontrarse en un único lugar, es decir, tiene que haber una *única fuente de la verdad*, y si queremos que distintos métodos compartan un estado y las acciones que lo modifican, tenemos que nuclearlo en un único componente y compartir los métodos que cada uno necesitan.

Para eso, tenemos que **elevar el estado** del componente `Modal`. A dónde lo elevamos? **Al ancestro común entre los componentes que necesitan compartir el estado más inmediato**. En este caso, tanto `Modal` como `Button` son hermanos, por lo que su ancestro común más inmediato es `Container`. Por lo tanto, tenemos que elevar el estado de `Modal` a `Container`. 

Cómo hacemos esto? Primero tenemos que *extraer* el estado y los métodos que lo modifican, y cambiarlo de componente. En nuestro caso:

```javascript
class Container extends React.Component {
  state = {
    visible: true
  }
  open = () => {
    this.setState({ visible: true })
  }
  close = () => {
    this.setState({ visible: false })
  }
  render(){
    return (
      <>
        <Button />
        <Modal />
      </>
    )
  }
} 
```

```javascript
class Modal extends React.Component {
  render(){
    return(
      visible &&
      <div className='modal'>
        <button onClick={this.close}>X</button>
      </div>
    )
  }
} 
```

`Container` posee ahora el estado `visible` y los métodos `open` y `close`. Podemos renombrarlos a algo un poco más significativo:

```javascript
class Container extends React.Component {
  state = {
    modalVisible: true
  }
  openModal = () => {
    this.setState({ modalVisible: true })
  }
  closeModal = () => {
    this.setState({ modalVisible: false })
  }
  render(){
    return (
      <>
        <Button />
        <Modal />
      </>
    )
  }
} 
```

Cómo hacemos ahora para compartir este estado y estos métodos? Mediante **props**. Empecemos por compartirle el estado `modalVisible` a nuestro componente `Modal`:

```javascript
class Container extends React.Component {
  state = {
    modalVisible: true
  }
  openModal = () => {
    this.setState({ modalVisible: true })
  }
  closeModal = () => {
    this.setState({ modalVisible: false })
  }
  render(){
    return (
      <>
        <Button />
        <Modal visible={this.state.modalVisible}/>
      </>
    )
  }
} 
```

Ahora `Modal` tiene un `prop` `visible` que va a depender del estado `modalVisible`. Dentro de `Modal` tenemos que obtenerlo de los `props`.

```javascript
class Modal extends React.Component {
  render(){
    const {visible} = this.props
    return (
      visible &&
      <div className='modal'>
        <button onClick={this.close}>X</button>
      </div>
    )
  }
} 
```

El siguiente paso es pasarle el método `closeModal` al componente `Modal`. Esto también lo hacemos mediante `props`:

```javascript
class Container extends React.Component {
  state = {
    modalVisible: true
  }
  openModal = () => {
    this.setState({ modalVisible: true })
  }
  closeModal = () => {
    this.setState({ modalVisible: false })
  }
  render(){
    return (
      <>
        <Button />
        <Modal 
          visible={this.state.modalVisible}
          onClose={this.closeModal}
        />
      </>
    )
  }
} 
```

Es importante destacar que lo que estamos pasándole a `Modal` en el prop `onClose` es una *referencia* al método `closeModal`. Entonces, dentro de nuestro `Modal`, podemos utilizarla así:

```javascript
class Modal extends React.Component {
  render(){
    const {visible, onClose} = this.props
    return (
      visible &&
      <div className='modal'>
        <button onClick={onClose}>X</button>
      </div>
    )
  }
} 
```

Dentro del prop `onClose` está la referencia al método `closeModal`. Cuando se haga click en el botón y se ejecute el callback que tiene asignado, React encuentra que tiene una referencia a otro método, va a buscar ese método, y luego lo ejecuta. Por lo tanto, al hacer click en el botón en el componente `Modal` estamos ejecutando el método `closeModal` del componente `Container`.

Si nos fijamos, ahora nuestro componente Modal ya no tiene un estado interno propio, por lo que podemos refactorizarlo en un componente funcional:

```javascript
const Modal = ({visible, onClose}) => (
    visible &&
    <div className='modal'>
      <button onClick={onClose}>X</button>
    </div>
)
```

Ahora sólo nos queda pasar de la misma forma el método `openModal` a nuestro componente `Button`:

```javascript
class Container extends React.Component {
  state = {
    modalVisible: true
  }
  openModal = () => {
    this.setState({ modalVisible: true })
  }
  closeModal = () => {
    this.setState({ modalVisible: false })
  }
  render(){
    return (
      <>
        <Button onClick={this.openModal}/>
        <Modal 
          visible={this.state.modalVisible}
          onClose={this.closeModal}
        />
      </>
    )
  }
} 
```

y en el componente `Button`, usar dicho prop con el método asignado para algo:

```javascript
const Button = ({onClick}) => (
  <button onClick={onClick} />
)
```

Si bien `onClick={onClick}` puede prestarse a confusión, es importante entender qué significa cada cosa. El primer `onClick` representa un evento del elemento `button`, el segundo es un prop del componente `Button` que contiene el método que se le asignó cuando se creó el componente.

## Resumiendo

* Teníamos dos componentes, `Modal` y `Button`, que necesitaban compartir ciertas acciones que modificasen un estado en particular.
* Para esto buscamos el ancestro *común más inmediato*, el componente `Container`, de los cuales `Modal` y `Button` son componentes hijos.
* "Elevamos" el estado y las acciones que modifican dicho estado a dicho componente.
* Pasamos el estado y los métodos a cada componente que lo necesita mediante props.
* Dentro de cada componente, utilizamos esos props (que incluyen referencias a métodos del componente padre) para asignarlos a los elementos que corresponden.
* Refactorizamos los componentes de clase que ya no tienen estado interno a componentes funcionales.

Hecho esto, es importante destacar un par de cosas:

* Hay una única "fuente de la verdad" del estado `modalVisible`, que reside en el componente `Container`.
* El flujo de la información es de arriba hacia abajo, `Container` pasa el estado y los métodos a sus componentes hijos, pero no a la inversa.
* `Container` en ningún momento hace uso de dicho estado o de dichos métodos, simplemente se encarga de compartirlos entre sus componentes hijos (esto no es un requisito, puede que tenga que hacer uso de ellos dependiendo de la ocasión).



## Inputs controlados


Supongamos que queremos tener un `input` que, a medida que se va escribiendo algo dentro de este, se vaya mostrando a su vez lo que se escribre en un elemento `p`, y que tengamos un botón para borrar todo escrito. 

Nuestro componente inicial sería algo así:

```javascript
const LiveText = () => (
  <>
    <input type='text' />
    <p></p>
    <button>Borrar</button> 
  </>
)
```

Como nuestro componente va a tener algo que va a ir cambiando (el texto que se muestra), y en React todo lo que cambia tiene que ser parte del estado de un componente, necesitamos refactorizarlo a un componente de clase:

```javascript
class LiveText extends React.Component {
  render() {
    return (
      <>
        <input type='text' />
        <p></p>
        <button>Borrar</button> 
      </>
    )
  }
}
```

El siguiente paso es agregarle nuestro estado:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  render() {
    return (
      <>
        <input type='text' />
        <p>{this.state.text}</p>
        <button>Borrar</button> 
      </>
    )
  }
}
```

Y ahora necesitamos que a medida que ingresamos algo en el `input`, este vaya modificando el estado `text`. Para esto, podemos utilizar el evento `onChange`, que ejecuta un callback cada vez que un elemento con `value` (aquellos que se utilizan para formularios) cambia su valor.

Por lo tanto, tenemos que definir un método y asignarlo como callback al `onChange` del `input`:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  updateText = () => {
    this.setState({ text: ??? })
  }
  render() {
    return (
      <>
        <input 
          type='text'
          onChange={this.updateText}
        />
        <p>{this.state.text}</p>
        <button>Borrar</button> 
      </>
    )
  }
}
```

El problema con el que nos encontramos es que no tenemos una forma (sencilla, al menos) de obtener el `value` del `input`. En React no es recomendable trabajar con los elementos del DOM directamente, a menos que no quede otra opción. 

Por suerte, para este caso, podemos contar con una cierta particularidad de React. Cuando a un método que actúa como callback de un evento le definimos un parámetro, como ser:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  updateText = event => {
    this.setState({ text: ??? })
  }
  render() {
    return (
      <>
        <input 
          type='text'
          onChange={this.updateText}
        />
        <p>{this.state.text}</p>
        <button>Borrar</button> 
      </>
    )
  }
}
```

ese parámetro React lo llena con un objeto que contiene mucha información sobre el evento generado. Lo bueno de este objeto es que tiene una propiedad, `target`, que tiene asignado el elemento donde se generó el evento (en este caso, el `input`), y dentro de ese `target` podemos acceder al `value` de dicho elemento.

Por lo tanto, podemos hacer lo siguiente:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  updateText = event => {
    this.setState({ text: event.targe.value })
  }
  render() {
    return (
      <>
        <input 
          type='text'
          onChange={this.updateText}
        />
        <p>{this.state.text}</p>
        <button>Borrar</button> 
      </>
    )
  }
}
```

De esta forma, a medida que escribimos en el `input`, se dispara el método `updateText`, desde el cual obtenemos el `value` del `input` (el `target` del evento) y actualizamos el estado `text`. Como el elemento `p` muestra el contenido de dicho estado, se va a ir actualizando en vivo.

Agreguemos ahora un método para borrar el estado:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  updateText = event => {
    this.setState({ text: event.targe.value })
  }
  deleteText = () => {
    this.setState({ text: '' })
  }
  render() {
    return (
      <>
        <input 
          type='text'
          onChange={this.updateText}
        />
        <p>{this.state.text}</p>
        <button onClick={this.deleteText}>Borrar</button> 
      </>
    )
  }
}
```

Si hacemos esto, nos vamos a encontrar con que cuando escribimos algo, se actualiza el contenido del texto, y cuando hacemos click, este se borra, pero en nuestro `input` va a seguir quedando lo que tenemos escrito. Por qué sucede esto? El problema acá es que el `value` del input no depende del estado `text`. Esto es lo que se conoce como un **input no controlado**, es decir, un input cuyo estado (lo que está mostrando), no lo controla React, sino el navegador. 

Para solucionar esto, tenemos que hacer que el `value` del `input` dependa del estado del componente. Esto lo hacemos de la siguiente manera:

```javascript
class LiveText extends React.Component {
  state = {
    text: ''
  }
  updateText = event => {
    this.setState({ text: event.targe.value })
  }
  deleteText = () => {
    this.setState({ text: '' })
  }
  render() {
    return (
      <>
        <input 
          type='text'
          value={this.state.text}
          onChange={this.updateText}
        />
        <p>{this.state.text}</p>
        <button onClick={this.deleteText}>Borrar</button> 
      </>
    )
  }
}
```

De esta forma, el input se vuelve **controlado**. Cuando hay un cambio en el `value` del `input`, se dispara el callback del evento `onChange`, `updateText`, que actualiza el estado `text`. Al hacerlo, se actualiza también el `value` del `input` con el valor asignado en dicho estado, que, hasta el momento, es lo que acabamos de escribir.

Parece un paso extra e innecesario, pero lo bueno de controlar el `value` del `input` es que cuando modificamos el estado mediante algún otro elemento u componente (en este caso, el botón), el `input` también se actualiza. De esta forma, no hay discordancia entre el estado del input y el estado que manipula.

## Resumiendo

Para tener un input controlado necesitamos:

* Un estado donde ir guardando el `value` del `input`
* Un método que cambie dicho estado
* Asignar dicho método al `onChange` del `input`
* En dicho método, declararle un parámetro (por lo general, se llama event) y acceder al valor del input mediante `event.target.value`
* Asignarle al `value` del input dicho estado



## Pop drilling

Cuando necesitamos que dos o más componentes compartan un mismo estado o acciones que modifiquen un mismo estado, vimos que teníamos que *elevar* el estado y sus métodos al *componente ancestro común más inmediato* en la jerarquía, y pasar luego dicho estado y métodos mediante props. Esto sucede porque el flujo del estado en una aplicación de React es desde arriba hacia abajo (top down), es decir, el estado sólo puede ser compartido a componentes hijos.

En React se llama a este fénomeno `props drilling` (taladrado de props) porque los props van pasando por todas los componentes (capas) de nuestra aplicación como si fuera un taladro. El `prop drilling` no es de por sí algo malo o que haya que evitar a toda costa. De hecho, es parte integral del funcionamiento de React. Pero sí puede volverse bastante engorroso, molesto, difícil de seguir, de pensar en forma lógica y de mantener. Dependiendo de que tan alejados estén dichos componentes en la estructura de nuestra app, es probable que tengamos que pasar esos estados por un montón de componentes intermedios para llegar al que necesitamos.

Otro problema que tiene el `prop drilling` es que genera componentes muy "acoplados" o dependientes entre sí. Si tenemos un conjunto de varios componentes anidados que se van pasando props entre sí, nos queda una jerarquía muy rígida. No podemos sacar ninguno de esos componentes porque se corta la cadena, y si lo hacemos tenemos que modificar los que ya tenemos. Tampoco podemos agregar fácilmente un componente entre medio de estos, si lo hacemos, tenemos que tomar los props del componente padre y pasarlo a los hijos para no cortar el flujo del "taladrado".

Pasar props entre dos o tres componentes es aceptable y no trae demasiados incovenientes, pero cuando ya nos excedemos de esta cantidad se empieza a volver inmanejable.

Para solucionar esto, existen un par de técnicas al respecto.

<br />

## Composición (vs anidado)

La composición es una técnica o patrón muy utilizada en React, que nos permite construir componentes de forma modular, compuestos o integrados por otros componentes combinados de forma diversa. De esta forma, tenemos un componente más genérico y reutilizable, que no contiene siempre los mismos componentes hijos, sino que puede aceptar numerosos variaciones.

Primero veamoslo a nivel de sintaxis, y después analicemos cómo lograrlo. Un componente anidado nos queda de la siguiente forma:

```javascript
const SocialCard = () => (
  <Card>
    <Avatar />
    <Username />
    <Date />
    <Text />
  </Card>
)
```

Este componente tiene siempre los mismos componentes hijos. Si quisieramos obtener variaciones de este componente tendríamos que o crear otros componentes, por ejemplo:

```javascript
const SocialCardWithouAvatar = () => (
  <Card>
    <Username />
    <Date />
    <Text />
  </Card>
)
```

O personalizarlo mediante props

```javascript
const SocialCard = ({hasAvatar, hasDate}) => (
  <Card>
    {hasAvatar && <Avatar />}
    <Username />
    {hasDate && <Date />}
    <Text />
  </Card>
)
```

De todas formas, si queremos por ejemplo cambiar el orden de los componentes, se vuelve más complicado.

En cambio, un componente que utiliza composición, podemos usarlo de la siguiente forma:

```javascript
// ... (en algún JSX válido)

<SocialCard>
    <Avatar />
    <Username />
    <Date />
    <Text />
</SocialCard>

<SocialCard>
    <Avatar />
    <Username />
    <Text />
</SocialCard>

<SocialCard>
    <Username />
    <Text />
    <Date />
</SocialCard>

// ...
```

Como ves, cada vez que utilizamos el componente `SocialCard` definimos qué componentes queremos que lo compongan, e incluso el orden en que queremos que aparezcan. De esta forma, el componente se vuelve mucho más reutilizable, ya que nos permite crear múltiples variaciones y "armarlo" con los componentes que deseemos de la forma que deseemos.

La "contra" que tiene es que hace el código donde tenemos que utilizar el componente un poco más verborrágico. **Esto no implica que siempre hay que usar composición y nunca anidar componentes**. Como todo, son estrategias y herramientas que tenemos a nuestro alcance, ninguna de ellas es mejor o peor, sino que cada una tiene su utilidad, sus ventajas y sus desventajas según el caso de uso. Todo depende de qué tanta reutilización y personalización queremos darle a nuestro componente. Por lo general esto es siempre algo deseable, pero muchas veces tenemos componentes que no necesitamos o no queremos que varíen tanto.

Cómo soluciona la composición el problema del `prop drilling`? Veamoslo con otro ejemplo. Supongamos que tenemos la siguiente jerarquía de componentes:

```javascript
const App = () => (
  <Nav />
)

const Nav = () => (
  <UserInfo />
)

const UserInfo = () => (
  <Username />
)

const Username = ({username}) => (
  <p>{username}</p>
)
```      

Si tuviésemos la info del username en nuestra `App` (porque la obtenemos de un `fetch`, por ejemplo), tendríamos que pasarla como prop por toda la jerarquía para que llegue a `Username`:


```javascript
const App = () => (
  <Nav username='Ada' />
)

const Nav = ({username}) => (
  <UserInfo username={username} />
)

const UserInfo = ({username}) => (
  <Username username={username} />
)

const Username = ({username}) => (
  <p>{username}</p>
)
```          

En cambio, si lo hiciésemos mediante composición, nos quedaría así:


```javascript
const App = () => (
  <Nav>
    <UserInfo>
      <Username username='Ada' />
    </UserInfo>
  </Nav>
)
```

Como ves, no hizo falta que pasásemos username por toda la jerarquía. De esta forma, nos evitamos el inconveniente del `prop drilling`.

Cómo hacemos esto?

La técnica es muy sencilla. Todo componente tiene un prop propio de React, llamado `children` (hijos). En ese prop están los **componentes hijos inmediatos** que se le anidan cuando se utiliza el componente. Por lo tanto, volviendo a nuestro ejemplo anterior: 

```javascript
const SocialCard = props => (
  <Card>
    {props.children}
  </Card>
)

// o con destructuring

const SocialCard = ({children}) => (
  <Card>
    {children}
  </Card>
)
```

En `props.children` van todos los componentes que anidamos cuando utilizamos nuestro componente con **etiquetas cerradas**. Por lo tanto, si en algún lado usamos:

```javascript
// ... algún JSX válido
<SocialCard>
    <Avatar />
    <Username />
    <Date />
    <Text />
</SocialCard>

<SocialCard>
    <Avatar />
    <Username />
    <Text />
</SocialCard>
// ...
```

En el primer caso, `props.children` contiene los componentes `<Avatar />`, `<Username />`, `<Date />`, `<Text />`; en el segundo, `<Avatar />`, `<Username />`, `<Text />`.

<br />

## Componentes como props

En vez de agrupar todos los componentes que se incluyan dentro de `props.children`, quizás lo que queremos es tener *cierta* personalización de lo que el componente puede aceptar, y tener el control como para poder distribuirlos dentro de nuestro componente. Para esto, lo que podemos hacer es pasar componentes como props. Supongamos lo siguiente:

```javascript
const FormControl = ({header, input, button}) => (
  <div className='form'>
    {header}
    <div className='input-container'>
      {input}
      {button}
    </div>
  </div>
)
```

Acá tenemos un componente `FormControl` que acepta tres props, `header`, `input`, `button`. Esos props después los distribuye dentro de sí mismo como mejor le conviene o considera necesario.

Ahora podemos utilizar este componente de la siguiente forma:

```javascript
const Modal = () => (
  <FormControl 
    header={<Label>Username</Label>}
    input={<TextInput placeholder={Ingrese su usuario} />}
    button={<Button type='primary'>Ingresar</Button>}
  />
)
```

Y en otro lado:

```javascript
const Modal = () => (
  <FormControl 
    header={<Text type='important'>Ingrese su fecha de nacimiento</Text>}
    input={<DatePicker />}
    button={<Button type='warning'>Ingresar</Button>}
  />
)
```

De esta forma, podemos reutilizar el mismo componente, pasándole distintos componentes como props, lo que nos permite personalizarlo en la medida que el componente nos deja hacerlo (no podemos agregar más componentes de los que nos indica, ni modificar el orden en que se renderizan).

La estrategia es sencilla. Definimos un prop dentro de nuestro componente, ese prop lo incluimos donde deseemos hacerlo, y cuando utilicemos dicho componente, como valor de dicho prop, le pasamos otro componente.

Por qué esto también soluciona el problema de `props drilling`?

Al especificar el componente mediante props, lo estamos definiendo en el mismo componente, de una forma a como lo hacíamos con la composición, solo que de manera un poco más limitada (o específica). En el caso anterior, por ejemplo, si hubiéramos querido definir desde `Modal` el texto de `Button`, tendríamos que haberlo pasado como prop a `FormControl`, y este tendría que haberlo pasado a su vez a `Button`. En cambio, al definirlo todo desde `Modal`, podemos saltarnos el paso intermedio y definirlo directamente en el componente.

<br />

## Resumiendo

Cuando tenemos que compartir un valor de un componente a otro y estos están muy distanciados en la jerarquía de componentes, tenemos que pasar ese valor mediante props por todos los componentes intermedios. Si bien con uno o dos componentes no es demasiado problema (y hasta es deseable), cuando ya son más la situación se empieza a complicar:

  * El código se vuelve muy verborrágico y sucio
  * La lectura del mismo se dificulta
  * Seguir el camino de props es engorroso y una pérdida de tiempo
  * Dificulta pensar la lógica
  * Los componentes se enteran de datos que no les interesan y con los que no hacen nada más que pasarlos
  * La cantidad de props por componente se incrementa mucho
  * Los componentes quedan muy acoplados y con una jerarquía muy rígida, por lo que sacarlos o incluir nuevos en el medio se vuelve difícil
  
Para solucionar esto, tenemos algunas técnicas:
  
La primera se llama **composición**, y consiste en utilizar el prop `children`. Este es un prop específico de React, que se llena con todos los componentes hijos que se incluyen dentro de las etiquetas de apertura y cierre del componente. De esta forma, podemos darle la opción a quien utiliza nuestro componente de "componerlo" con otros componentes anidados, cualesquieran sean y de la cantidad que sean.

La otra opción es una forma más limitada de composición, que consiste en pasar componentes como props. De esta forma, ya pasamos el componente con sus propios props, y nos ahorramos tener que hacer el puente de dichos props entre uno y otro componente.
  
  
## Contexto


Existen ocasiones en que las estrategias de composición se quedan cortas a la hora de solucionar el problema de `prop drilling` especialmente cuando los componentes están muy alejados en la jerarquía de nuestra aplicación. Para evitar este inconveniente, tenemos un recursos a nuestro disposición: el **contexto**.

*Un contexto es una forma de compartir información entre componentes sin tener que estar pasándoselos mediantes props, saltando todos los componentes intermedios en la jerarquía*. 

Se puede pensar al contexto como una especia de estado "global" disponible para cualquier componente *en una cierta jerarquía o árbol de componentes*. Esto es fundamental entenderlo, y ya lo veremos más en detalle, pero un concepto importante del contexto es que ese estado que comparte está disponible para un *subconjunto* de componentes (los que definamos), no *necesariamente* para todos los componentes de la aplicación, a menos que así lo deseemos.

Para crear un contexto, simple tenemos que utilizar el método de React, `createContext`, de la siguiente forma:

```javascript
const MyContext = React.createContext()
```

`MyContext` es un objeto que nos provee de dos propiedades, el `Provider` (proveedor) y el `Consumer` (consumidor). El `Provider` es el que comparte la información dentro de los componentes incluido dentro del contexto, es decir, pone a disposición o provee la información a ser compartida. Los `Consumer` son aquellos que acceden a esa información dispuesta por el proveedor. 

**Con el Provider definimos la información a compartir y con los Consumer accedemos a la misma.**

Una vez que tenemos nuestro contexto, podemos utilizar nuestro `Provider` en cualquier componente que deseemos colocarlo. Simplemente, lo que tenemos que hacer es declararlo y anidar dentro suyo los componentes que van a estar incluidos dentro del contexto:

```javascript
const MyContext = React.createContext()

const MyComponent = () => (
  <>
    <MyContext.Provider>
      {/** Componentes que pueden acceder al contexto */}
    </MyContext.Provider>
    // Componentes que no pueden acceder al contexto
    <OtherComponent />
  </>
)  
```

Es importante notar que sólo aquellos componentes que estén dentro de las etiquetas de apertura y cierre del `Provider` van a tener acceso a los valores que comparta, así como también sus componentes hijos (dado que lo que accede un componente lo puede pasar a sus componentes hijos, el flujo de datos en una aplicación de React es top-down, de arriba a abajo). Por lo tanto, un `Provider` contiene *una cierta jerarquía de componentes*, aquellos que queden fuera o no estén incluidos dentro de dicha jerarquía no podrán acceder a los valores que el `Provider` disponga. Dentro del `Provider` se pueden incluir todos los componentes que deseemos.

Por su parte, para declarar (al menos) un `Consumer`, tenemos que hacerlo en algún componente *que esté incluido dentro del contexto, es decir, de la jerarquía que contiene el `Provider`. Si incluimos un `Consumer` en un componente que no se encuentra dentro de esta jerarquía, no podremos acceder a la información compartida. Para hacer esto, de la misma forma que usamos el `Provider`, creamos un `Consumer` en un componente. 

```javascript
const MyContext = React.createContext()

const MyComponent = () => (
    <MyContext.Provider>
      <MyChildComponent />
    </MyContext.Provider>
)

const MyChildComponent = () => (
  <MyGrandChildComponent />
) 

const MyGrandChildComponent = () => (
  <MyContext.Consumer>
    {/** Aca accedemos a los datos que comparte el Provider */}
  </MyContext.Consumer>
)
```

**Dentro de la jerarquía que contiene un Provider podemos tener múltiples Consumer**

Una vez que ya tenemos un `Provider` y al menos un `Consumer`, ya podemos compartir información entre estos sin tener que pasar por todos los componentes intermedios que hay entre ambos.

Para hacer eso, tenemos que declarar qué información queremos compartir. Esto lo hacemos en el `Provider`. El `Provider` tiene una prop especial llamada `value`, cuyo valor será aquello que el `Provider` ofrecerá a los `Consumer` para que puedan utilizarlo. Como toda prop, el valor puede ser cualquier tipo de dato válido de JS: número, string, booleano, array, objeto, función. Esto se escribe de la siguiente forma:

```javascript
<Context.Provider value={/** valor a compartir */}>
  {/** componentes dentro del contexto */}
</Context.Provider>
```

Por ejemplo, si quisiéramos compartir una string, tendríamos que hacer lo siguiente:

```javascript
const MyContext = React.createContext()

const MyComponent = () => (
    <MyContext.Provider value='Ada Lovelace'>
      <MyChildComponent />
    </MyContext.Provider>
)

const MyChildComponent = () => (
  <MyGrandChildComponent />
) 

const MyGrandChildComponent = () => (
  <MyContext.Consumer>
    {/** Aca accedemos a los datos que comparte el Provider */}
  </MyContext.Consumer>
)
```

Ahora nuestro `Provider` ya está compartiendo un valor mediante el prop `value`. Para acceder a este valor, tenemos que hacerlo desde algún `Consumer`. El `Consumer` dentro suyo toma un callback. Como esto es sintaxis de JS embebida en sintaxis JSX, necesitamos separarla mediante llaves:

```javascript
<Context.Consumer>
  {() => ()}
</Context.Consumer>
```

Dicho callback acepta un parámetro, que va a contener el valor compartido en el prop `value` del `Provider`. Por lo tanto, si definimos


```javascript
<Context.Consumer>
  {value => ()}
</Context.Consumer>
```

El parámetro `value` va a tener aquello que el `Provider` provea mediante su prop `value`. Dentro de dicho callback, luego, podemos hacer lo que queramos con esa información, por ejemplo, renderizar un componente pasándole dicho value como prop.

Volviendo a nuestro ejemplo anterior:

```javascript
const MyContext = React.createContext()

const MyComponent = () => (
    <MyContext.Provider value='Ada Lovelace'>
      <MyChildComponent />
    </MyContext.Provider>
)

const MyChildComponent = () => (
  <MyGrandChildComponent />
) 

const MyGrandChildComponent = () => (
  <MyContext.Consumer>
    {value => (
      <p>{value}</p>
    )}  
  </MyContext.Consumer>
)
```

Aquí estamos accediendo desde nuestro `Consumer` al `value` que comparte el `Provider`, y utilizándolo para renderizar un elemento `p` con el valor de dicho `value` como texto. En este ejemplo se puede ver bien que el componente intermedio `MyChildComponent` no necesita pasar el prop, sino que accedemos directamente al mismo desde el `Consumer`.

<br />

## Dónde colocar el Provider?

El criterio para determinar esto es el mismo que utilizamos a la hora de "elevar el estado". De hecho, podemos pensar al contexto como una forma de elevar el estado de un componente para que otros componentes puedan accederlo, con la ventaja de que no tenemos que pasar los props por todos los componentes intermedios en la jerarquía.

Por lo tanto, lo que tenemos que considerar es cuál es el *componente ancestro común más inmediato*, es decir, el componente que contiene a todos aquellos componentes que necesitan compartir un cierto estado, y que está primero en la jerarquía. La idea de esto es que el `Provider` sólo contenga dentro de su jerarquía a aquellos componentes que van a tener que estar al tanto del estado compartido, y dejar afuera a aquellos componentes que no les interesa ni les incumbe dicho estado. De esta forma, separamos responsabilidades. Para esto entonces tenemos que:

  * identificar cuáles van a ser aquellos componentes que necesiten compartir un cierto estado. Estos componentes van a contener los `Consumer`.
  * una vez hecho esto, tenemos que empezar a ascender en la jerarquía y ver cuál es el primer componente que encontramos que contiene a todos estos. Este componente será el que contenga al `Provider`.
  
<br />

## Resumiendo

  **Contexto**

  * Un contexto es una forma de compartir estados entre dos o más componentes, sin tener que estar pasando props por todos los componentes intermedios de la jerarquía
  * Un contexto se crea con el método `React.createContext()`
  * Un contexto contiene a un subárbol o subjerarquía de componentes. Sólo los componentes que estén dentro de esta jerarquía podrán acceder a los estados compartidos
  * Podemos tener muchos contextos en una aplicación
  * Cada Contexto debería referir a una cierta cosa en específico, a un tipo de estado, feature, o característica de la aplicación que necesite ser compartido
  * Podemos anidar contextos dentro de otros en cualquier momento de la jerarquía
  * Un contexto ofrece dos propiedades: el `Provider` y el `Consumer`.
  * Podemos anidar distintos `Providers` y `Consumers` entre sí si es necesario 

  **Provider**  
  
  * Sólo puede haber un `Provider` por Contexto, pero muchos `Consumer`.
  * El `Provider` es el que provee o distribuye el estado a ser compartido
  * Para declarar un `Provider` simplemente necesitamos usar el componente `<Context.Provider></Context.Provider>`
  * Dentro del `Provider` incluimos aquellos componentes que queramos que tengan acceso al estado compartido
  * Como el flujo de datos en React es top-down (de arriba hacia abajo), aquellos componentes hijos de los componentes que estén declarados dentro de las etiquetas del `Provider` también tendran acceso a lo que el `Provider` comparta
  * Para compartir un valor en el `Provider` tenemos que declarar la prop `value y pasarle el valor que deseemos
  * El criterio que utilizamos para colocar a nuestro `Provider` es el mismo que utilizamos a la hora de elevar el estado de un componente, es decir, buscando el *componente ancestro común más inmediato* de los componentes que necesitan compartir el estado
  
  **Consumer**
  
  * El `Consumer` es el que accede al estado que el `Provider` comparte
  * El `Consumer` debe estar en un componente que se encuentre dentro de la jerarquía del `Provider`
  * Para declarar un `Consumer` en un componente que necesita acceder al estado que el `Provider` comparte, tenemos que utilizar el componente `<Context.Consumer></Context.Consumer>`
  * Dentro del `Consumer` va un callback
  * Dicho callback toma un parámetro, ese parámetro va a contener el valor que el `Provider` ofrezca
  * Con ese parámetro luego podemos hacer lo que deseemos, como utilizarlo para algún componente
  * Lo que el callback incluido dentro del `Consumer` devuelva va a ser renderizado, si devuelve un componente este va a mostrarse
  
  
## Contexto patrón

Un patrón en desarrollo es una solución común a un problema recurrente, que ya fue probada y utilizada en muchísimos casos y se ha encontrado que es útil a la hora de resolverlo. Conocer (y aplicar) distintos patrones acelera nuestra velocidad de desarrollo, ya que no tenemos que estar pensando e implementando soluciones desde cero, con todo el tiempo y el testeo y debuggeo que eso implica.

Con respecto a contextos en React, hay un patrón muy utilizado que trae varias ventajas. Para construirlo, necesitamos seguir los siguientes pasos:

<br />

## Pasos para crear el patrón

### 1. Crear un archivo separado

Por lo general, el archivo va a llamarse `nombre del contexto + Context`. Por ejemplo:

  - AppContext
  - LoginContext
  - AdminContext
  - etc
  
Estos archivos se guardan dentro de una carpeta `Contexts` dentro de `components`. 

<br />

### 2. Crear un contexto

Dentro de nuestro archivo, tenemos que crear nuestro contexto:

```javascript
import React from 'react'

const MiContexto = React.createContext()
```

<br />

### 3. Crear un componente de clase

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  render(){
    return ()
  }
}

export default MiProvider
```

Si bien este componente tiene en su nombre `Provider`, no hay que dejarse confundir por el mismo (tranquilamente podría tener otro nombre). El `Provider` real es el que sale de `MiContexto.Provider`, ese es el que ofrece la posibilidad de compartir estados. Nuestro componente (`MiProvider`) es un componente contenedor (o *wrapper*, en inglés). ¿Qué significa esto? Que *lo único que hace es contener (o envolver) a otro componente*. Esa es toda su responsabilidad y función, no renderiza nada, simplemente contiene a otro componente. Ya veremos las ventajas de esto, pero el componente contenedor es un patrón también muy utilizado dentro de React.

<br />

### 4. Agregar el `Provider` dentro del método `render`

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  render(){
    return (
      <MiContexto.Provider></MiContexto.Provider>
    )
  }
}

export default MiProvider
```

De nuevo, `MiContexto.Provider` es nuestro `Provider` real. Si nos fijamos bien, `MiProvider` *contiene* a `MiContexto.Provider`

<br />

### 5. Agregar el `this.props.children` dentro del `Provider`

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  render(){
    return (
      <MiContexto.Provider>
        {this.props.children}
      </MiContexto.Provider>
    )
  }
}

export default MiProvider
```

Al hacer esto lo que permitimos es que los componentes hijos que anidemos en nuestro componente `MiProvider`, cuando lo utilicemos, pasen a incluirse realmente en el `Provider` verdadero. De esta forma, "tomamos" los componentes hijos del componente contenedor y los "inyectamos" en el `Provider`, que es lo que va a permitir compartir estados en estos componentes.

<br />

### 6. Definir los estados y métodos que vamos a necesitar

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  state = {
    algunEstado: 'algun valor'
  }
  cambiarEstado = nuevoValor => {
    this.setState({ algunEstado: nuevoValor })
  }
  render(){
    return (
      <MiContexto.Provider>
        {this.props.children}
      </MiContexto.Provider>
    )
  }
}

export default MiProvider
```

Todos los estados y métodos que necesitemos que nuestro `Provider` comparta necesitamos definirlo en nuestro componente contenedor (`MiProvider`). Es importante notar que si bien tenemos que definir todo lo que queremos compartir, no todo lo que definamos necesitamos compartirlo. Es posible que necesitemos estados o métodos propios a nuestro contenedor que no deseamos que los demás componentes accedan, y esto es perfectamente válido.

<br />

### 7. Definir el prop `value` en el `Provider` y pasarle un objeto

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  state = {
    algunEstado: 'algun valor'
  }
  cambiarEstado = nuevoValor => {
    this.setState({ algunEstado: nuevoValor })
  }
  render(){
    return (
      <MiContexto.Provider
        value={{}}
      >
        {this.props.children}
      </MiContexto.Provider>
    )
  }
}

export default MiProvider
```

<br />

### 8. Definir como propiedades del objeto del `value` aquellas cosas que queremos compartir

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  state = {
    algunEstado: 'algun valor'
  }
  cambiarEstado = nuevoValor => {
    this.setState({ algunEstado: nuevoValor })
  }
  render(){
    return (
      <MiContexto.Provider
        value={{
          algunEstado: this.state.algunEstado
          cambiarEstado: this.cambiarEstado
        }}
      >
        {this.props.children}
      </MiContexto.Provider>
    )
  }
}

export default MiProvider
```

Este paso es fundamental. No basta con declarar los estados y los métodos que queremos compartir en nuestro componente, también *tenemos que indicar que queremos compartirlo*. Como quien pone a disposición de los `Consumer` los estados, tenemos que definirlo dentro de nuestro `Provider` real (`MiContexto.Provider`). Para compartir un valor, necesitábamos declarar el prop `value`.

Ahora bien, como por lo general tenemos muchas cosas que compartir (muchos estados y métodos), y a `value` sólo podemos pasarle un valor, para solucionar esto lo que se hace es pasarle un objeto. Un objeto, al ser una estructura de datos, puede contener muchos datos, que es algo que nos viene bien para esta situación. Por lo tanto, la estrategia es asignarle una propiedad al objeto por cada cosa que queremos compartir, y al valor de dicha propiedad le asignamos aquello que queremos compartir (un estado, un método, un valor).

Las propiedades no tienen necesariamente que tener los mismos nombres que aquello que comparten, pero hacerlo nos evita tener que estar pensando en nombres alternativos, y se presta un poco menos a confusión.

Es importante entender que como este objeto es el que comparte el `Provider`, cuando accedemos desde el `Consumer` a lo que hay en el `value`, esto va a ser un objeto, y por lo tanto, cuando queramos usar cosas de ese objeto, **tenemos que usar los nombres de las propiedades del objeto**, y no los nombres de las cosas en la clase.

Por ejemplo, si tenemos lo anterior pero con otro nombre:

```javascript
<MiContexto.Provider
  value={{
    algunEstado: this.state.algunEstado
    modificarEstado: this.cambiarEstado
  }}
>
```

Desde el `Consumer`, si queremos utilizar el método de la clase `cambiarEstado`, tenemos que accederlo con el nombre de la propiedad del objeto al que está asignado, en este caso, `modificarEstado`.

<br />

### 9. Exportar el `Consumer` como export nombrado 

```javascript
import React from 'react'

const MiContexto = React.createContext()

class MiProvider extends React.Component {
  state = {
    algunEstado: 'algun valor'
  }
  cambiarEstado = nuevoValor => {
    this.setState({ algunEstado: nuevoValor })
  }
  render(){
    return (
      <MiContexto.Provider
        value={{
          algunEstado: this.state.algunEstado
          cambiarEstado: this.cambiarEstado
        }}
      >
        {this.props.children}
      </MiContexto.Provider>
    )
  }
}

export const MiConsumer = MiContexto.Consumer
export default MiProvider
```

<br />

### 10. Importar y utilizar nuestro componente `Provider`

```javascript
import React from 'react'
import MiProvider from 'components/Contexts/MiContexto'

const App = () => (
  <MiProvider>
    <UnComponente />
    <OtroComponente />
  </MiProvider>
)
```

En este paso se ve la función de contenedor o *wrapper* que tiene nuestro componente. Lo que hace `MiProvider` es "envolver" nuestro `Provider` real, ocultarlo de la vista, tomar los componentes que tiene anidados (en este caso `UnComponente` y `OtroComponente`) y pasárselos al `Provider` real mediante `props.children`.

<br />

### 11. Importar y utilizar nuestro componente `Consumer`, desestructurando las propiedades que necesitemos

```javascript
import React from 'react'
import { MiConsumer } from 'components/Contexts/MiContexto'

const AlgunComponente = () => (
  <MiConsumer>
    {
      value => <AlgunOtroComponente algunProp={value.algunEstado} />
    }
  </MiConsumer>
)
```

Esta parte no difiere mucho de cómo usamos el `Consumer` realmente. En el parámetro `value` accedemos a un objeto, por lo tanto podemos utilizar sus propiedades, que declaramos previamente. También, como tenemos un objeto, podemos aprovecharlo y desestructurarlo:

```javascript
import React from 'react'
import { MiConsumer } from 'components/Contexts/MiContexto'

const AlgunComponente = () => (
  <MiConsumer>
    {
      ({algunEstado}) => <AlgunOtroComponente algunProp={algunEstado} />
    }
  </MiConsumer>
)
```

<br />

### Resumiendo

1. Crear un archivo separado
2. Crear un contexto
3. Crear un componente de clase
4. Agregar el `Provider` dentro del método `render`
5. Agregar el `this.props.children` dentro del `Provider`
6. Definir los estados y métodos que vamos a necesitar
7. Definir el prop `value` en el `Provider` y pasarle un objeto
8. Definir como propiedades del objeto del `value` aquellas cosas que queremos compartir
9. Exportar el `Consumer` como export nombrado 
10. Importar y utilizar nuestro componente `Provider`
11. Importar y utilizar nuestro componente `Consumer`, desestructurando las propiedades que necesitemos


<br />

## Ventajas

Si bien parecen muchos pasos y trabajo extra para poder utilizar contextos, este patrón tiene una serie de ventajas que lo compensan:

### Separación de componentes

Al tener toda la lógica (estados y métodos) propios de un contexto en un componente aparte, evitamos la confusión de estar mezclando las cosas propias de un contexto con las cosas propias de un componente (y ni hablar si tuviésemos varios contextos en un mismo componente, entonces la mezcla y la confusión se incrementarían enormemente).

### Separación de responsabilidades

Un componente debería siempre en lo posible tener una única responsabilidad (no hacer una única acción en particular, sino encargarse de una única funcionalidad de la aplicación). Esto es deseable porque facilita las cosas muchísimo a la hora de analizar, pensar, y entender un componente, y reduce enormemente la carga cognitiva y el tiempo que necesitamos para hacerlo. De esta forma, tener UN componente que se encargue de la lógica de UN contexto, es mucho mejor que tener todos los contextos mezclados en un componente.

### Centralización de lógica

Al tener la lógica en un sólo lugar, ya sabemos dónde encontrarla cuando necesitemos hacer una modificación, o cuando necesitamos ver qué cosas está compartiendo.

### Sintaxis más limpia

Por último, y no menos importante, poder utilizar el `Provider` como un único componente, sin necesidad de estar declarando el `value` y los estados y métodos a compartir en el mismo componente, hace que el código quede mucho más reducido, simple y fácil de leer, además de permitir mover con mucha mayor comodidad el `Provider` en caso de que lo necesitemos (si estaría todo junto y mezclado, tendríamos que mover estado y métodos también).


## Ciclo de vida


En React, un componente tiene lo que se conoce como un *ciclo de vida*. Al igual que los seres vivos, que nacen, crecen, se desarrollan y mueren, un componente se monta (nace), se actualiza (crece), y se desmonta (muere). *Montar* se refiere a que un componente se agrega al DOM.

Cada ciclo de vida de un componente tiene un método propio que le corresponde y que se ejecuta cuando el componente llega a esa etapa. Los métodos de ciclos de vida *sólo pueden ser accedidos y utilizados en un componente de clase*, que es otra de las ventajas que tiene sobre los componentes funcionales.

Muchos de estos métodos rara vez los utilizamos, pero es importante saber que existen por si en alguna ocasión necesitamos hacer uso de ellos. La lista de métodos son:

### Montado
  * `constructor()`
  * `componentWillMount()`
  * `render()`
  * `componentDidMount()`

### Actualización
  * `componentWillReceiveProps()`
  * `shouldComponentUpdate()`
  * `componentWillUpdate()`
  * `render()`
  * `componentDidUpdate()`

### Desmontado
  * `componentWillUnmount()`



### Diagrama de flujo del ciclo de vida de un componente

![React Life Cicle](https://miro.medium.com/max/1838/1*u8hTumGAPQMYZIvfgQMfPA.jpeg)



###  constructor

Este método es invocado cuando el componente está siendo creado y *antes* de ser montado (de ser agregado al DOM). Su función primaria es inicializar estado.

```javascript
class Clicker extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
       clicks: 0
    };
  }
  handleClick() {
    this.setState({ 
      clicks: this.state.clicks + 1
    });
  }
  //...
}
```

De todas formas, con la nueva sintaxis de JS llamada propiedades de clase, podemos evitar declarar el constructor para inicializar estado:

```javascript
class Clicker extends React.Component {
  state = {
       clicks: 0
  }
  handleClick() {
    this.setState({ 
      clicks: this.state.clicks + 1
    });
  }
  //...
}
```



### componentWillMount

Este método se ejecuta inmediatamente antes de que un componente es agregado al DOM / renderizado. Se incluye por cuestiones de retrocompatibilidad (para que las versiones viejas de React sigan funcionando), pero no deberíamos tener que utilizarlo nunca, además de que no tiene demasiada utilidad porque el componente todavía no existe en el DOM, por lo tanto no podemos cambios de estado.



### render

El ciclo de vida más utilizado, ya que lo requiere obligatoriamente todo componente de clase. Controla el renderizado de un componente durante la fase de montado y actualización. No se puede cambiar el estado en este método.



### componentDidMount

Uno de los más importantes y utilizados, se ejecuta inmediatamente después de que el componente se agrega al DOM / renderiza. Aquí es dónde hacemos llamadas mediante `fetch`, agregamos eventos, o modificamos cosas que requieran del DOM. Cualquier modificación del estado con `setState` hará que el componente se vuelva a renderizar.

```javascript
class Username extends React.Component {
  state = {
    username: ''
  }
  componentDidMount = () => {
    fetch('https://api.com')
    .then(response => response.json())
    .then(data => {
      this.setState({
        user: data.user
      });
    });
  }
}
```



### componentWillReceiveProps

Cuando un componente va a recibir nuevos props de su padre, este método se dispara. Este es un buen lugar para chequear si hay cambios reales en los actuales props y triggerear un cambio de estado basado en los nuevos valores. `componentWillReceiveProps` recibe un parámetro con los nuevos props.

```javascript
componentWillReceiveProps(nextProps) {
  if (this.props.id !== nextProps.id) {
    this.setState({
      feedContent: []
    })
  }
}
```



### shouldComponentUpdate

Este método existe solamente para mejorar la performance en situaciones que lo requieran. Renderizar un componente y chequear la diferencia del shadow DOM con el DOM actual (esto se llama *reconciliacíon*) son operaciones costosas y que consumente bastante. Por defecto, React renderiza todo el componente ante cualquier cambio. Este método da la posibilidad de devolver un booleano `true`/`false` que deteremine si React debería actualizar o no el componente. Si `shouldComponentUpdate()` devuelve `false`, entonces `componentWillUpdate()`, `render()`, y `componentDidUpdate()` no serán invocados.

`shouldComponentUpdate` recibe dos parámetros, el primero con los props a actualizar, y el siguiente con el estado a actualizar.

```javascript
shouldComponentUpdate(nextProps, nextState) {
  return this.props.clicks !== nextProps.clicks;
}
```



### componentWillUpdate

React invoca este método inmediantamente antes de renderizar el componente cuando nuevos props o estado fueron recibidos. No hay mucho para hacer en este método e idealmente debería ser evitado.



### componentDidUpdate

Ejecutado inmediatamente después de que React actualiza (re renderiza) un componente una vez que recibió nuevos props o estados. Este es un buen momento para hacer nuevos `fetch` o interactuar con otras partes de componente que lo requieran.



### componentWillUnmount

Se ejecuta cuando el componente se remueve del DOM (deja de renderizarse). Es un buen momento para limpiar todo aquello que haya quedado asociado al componente (información, eventos, etc)




### Qué hacer y no hacer en cada ciclo de vida

#### constructor
* **SI:** iniciar estado
* **NO:** causar efectos secundarios (llamadas fetch, etc)

#### componentWillMount
* **SI:** actualizar estado con `setState`, realizar mejoras de performance
* **NO:** causar efectos secundarios (llamadas fetch, etc)

#### componentWillReceiveProps(nextProps)
* **SI:** sincronizar estados y props
* **NO:**  causar efectos secundarios (llamadas fetch, etc)

#### shouldComponentUpdate(nextProps, nextState, nextContext)
* **SI:** realizar mejoras de performance
* **NO:** causar efectos secundarios (llamadas fetch, etc), llamar a `setState`

#### componentWillUpdate(nextProps, nextState)
* **SI:** sincronizar estados y props
* **NO:** causar efectos secundarios (llamadas fetch, etc)

### componentDidUpdate(prevProps, prevState, snapshot)
* **SI:** causar efectos secundarios (llamadas fetch, etc)
* **NO:** llamar a `setState`

#### componentDidMount
* **SI:** causar efectos secundarios (llamadas fetch, etc)

#### componentWillUnmount
* **SI:** remover timers, remover eventos
* **NO:** llamar a `setState`, ejecutar eventos, crear timers



## Fetch en React

El problema que tenemos cuando utilizamos `fetch` en React es que `fetch` es una operación asíncrona, es decir, tarda un tiempo en ejecutarse y obtener una respuesta. Si tenemos que esperar a que nos lleguen los datos para setear el estado de un componente, va a haber un momento en que dicho componente no va a tener estado, y nos va a tirar error.

Entonces, la estrategia es la siguiente:

  * Se define un estado vacío
  * Inicialmente, se utiliza ese estado para renderizar el componente
  * Se hace un `fetch` ya sea en un callback de un evento, o el método `componentDidMount`
  * Cuando se obtiene la respuesta del `fetch`, se actualiza el estado
  
## Fetch en un evento

```javascript
class UsersInfo extends React.Component {
  state = {
    users: []
  }
  loadData = () => {
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users }))
  }
  render(){
    return (
      <>
        {
          this.state.users.map(user => <p>{user}</p>)   
        }
        <div onClick={this.loadData}>Cargar</div>
      </>
    )
  }
}
```

Primero definimos un componente. Dicho componente tiene un estado, `users`, que es un array, inicialmente vacío. Ese estado es utilizado para renderizar una serie de elementos `p` con el nombre de cada usuario. Inicialmente, dichos elementos no se muestran porque el array no contiene nada. 

El componente también tiene un `div` que al hacerle click, ejecuta el método `loadData`. Este método hace una llamada a una api, y cuando obtiene la respuesta, actualiza el estado con `setState`, pasándole la información que le llegó desde la api. 

Cuando el estado se actualiza, React detecta que hubo un cambio, y actualiza el componente renderizando ahora sí el listado de nombres de usuarios.



## Fetch al inicializar un componente

Pero qué pasa si queremos que el `fetch` se realice cuando el componente es creado, y no cuando se dispara alguna acción? Para eso necesitamos utilizar el método `componentDidMount`, que se ejecuta inmediatamente después de que el componente se "monta", es decir, se agrega al DOM/renderiza.

Modificando nuestro ejemplo anterior, nos quedaría:

```javascript
class UsersInfo extends React.Component {
  state = {
    users: []
  }
  componentDidMount = () => {
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users }))
  }
  render(){
    return (
        <div>
        {
          this.state.users.map(user => <p>{user}</p>)   
        }
        </div>
    )
  }
}
```

En este caso, apenas el componente se agregue al DOM, no se renderizará nada, porque `users` es un array vacío. Inmediatamente, React ejecuta `componentDidMount` y realiza el `fetch`, cuando recibe la respuesta, actualiza el estado y por lo tanto el componente se rerenderiza.



## Agregando un loader

Qué pasa si queremos agregar un loader mientras se está esperando la respuesta de la api? Para eso necesitamos un estado (por ejemplo, `isLoading`), que pase a ser `true` cuando comienza a realizarse el `fetch` y `false` cuando recibimos la respuesta. Con ese estado luego podemos mostrar/ocultar algo en el componente, cambiar alguna clase o estilo, etc.

```javascript
class UsersInfo extends React.Component {
  state = {
    isLoading: false,
    users: []
  }
  componentDidMount = () => {
    this.setState({ isLoading: true })
    
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users, isLoading: false }))
  }
  render(){
    return (
      <div>
        { 
          this.state.isLoading && <p>Cargando...</p> 
        }
        { 
          this.state.users.map(user => <p>{user}</p>) 
        }
      </div> 
    )
  }
}
```



## Patrón contenedor

Un patrón muy útil a la hora de trabajar con `fetch` en React es el **patrón contenedor**. La idea básica es separar la lógica (la que genera el pedido a la api y tiene el estado que se actualiza) de la representación.

Supongamos que tenemos el siguiente componente:

```javascript
class UsersInfo extends React.Component {
  state = {
    users: []
  }
  componentDidMount = () => {
    this.setState({ isLoading: true })
    
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users }))
  }
  render(){
    return (
      <div> 
      {
       this.state.users.map(user => (
         <Card>
           <Avatar img={user.img}>
           <Link href={user.profileUrl}>
             <Username username={user.username}>
           </Link>
           <p>Registrado desde: <Date date={user.signupDate} /></p>
           <p>Cantidad de posts: {user.posts.length} /></p>
         </Card>
       ))
      }
      </div>
    )
  }
}
```

En este componente, tenemos mezclado la representación (lo que se renderiza) de la lógica que obtiene la info. Para separar esto, necesitamos un componente "contenedor", de la siguiente forma:

```javascript
class UsersInfoContainer extends React.Component {
  state = {
    users: []
  }
  componentDidMount = () => {
    this.setState({ isLoading: true })
    
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users }))
  }
  render(){
    return (
      <div>
      {
        this.state.users.map(user => <UserInfo user={user} />)
      }
      </div>
    )
  }
}
```

Y nuestro componente encargado de la vista:

```javascript
const UserInfo = ({user}) => (
    <Card>
      <Avatar img={user.img}>
      <Link href={user.profileUrl}>
        <Username username={user.username}>
      </Link>
      <p>Registrado desde: <Date date={user.signupDate} /></p>
      <p>Cantidad de posts: {user.posts.length} /></p>
    </Card>
)
```

De esta forma, separamos responsabilidades, nos quedan componentes más pequeños y sencillos de entender, y en el caso de tener que modificar una cosa (lógica o representación), sabemos donde encontrarla. 

Este patrón se puede combinar con el patrón de lista, de la siguiente forma:

```javascript
const UserInfoList = ({users}) => (
   <div>
   {
     users.map(user => <UserInfo user={user} />)
   }
   </div>
)
```

Y en nuestro componente contenedor, modificar lo siguiente:


```javascript
class UsersInfoContainer extends React.Component {
  state = {
    users: []
  }
  componentDidMount = () => {
    this.setState({ isLoading: true })
    
    fetch('https://www.miapi.com')
    .then(response => response.json())
    .then(data => this.setState({ users: data.users }))
  }
  render(){
    return (
      <UserInfoList users={this.state.users} />
    )
  }
}
```

### Videos explicativos

[![Workshop for Beginners. Videos explicativos del coordinador Cristhian Duran](https://pbs.twimg.com/media/EIS97poXkAgJWIB.jpg)](https://www.youtube.com/watch?v=gTlfsOCib0I&list=PLlhKBGO4zX44gXJATxBpNiIjAvOZZPGE8)
[Explicación de Cristhian Duran, coordinador del evento](https://www.youtube.com/watch?v=gTlfsOCib0I&list=PLlhKBGO4zX44gXJATxBpNiIjAvOZZPGE8)


[Créditos: Pablo Hoc](https://twitter.com/pablohoc)
