# ECMAScript 6 <sup>[git.io/es6features](http://git.io/es6features)</sup>

## Introduction
ECMAScript 6 is the upcoming version of the ECMAScript standard.  This standard is targeting ratification in December 2014.  ES6 is a significant update to the language, and the first update to the language since ES5 was standardized in 2009.    Implementation of these features in major JavaScript engines is [underway now](http://kangax.github.io/es5-compat-table/es6/).

ECMAScript 6 é a próxima versão do padrão ECMAScript. A norma deverá ser ratificada em Dezembro de 2014. ES6 é um upgrade significativo na linguagem, e a primeira atualização desde que foi padronizada em 2009. A Veja a implementação dessas funcionalidades noas principais engines de javascript no link [Tabela de Compatibilidade do ECMAScript 6](http://kangax.github.io/es5-compat-table/es6/).

See the [draft ES6 standard](https://people.mozilla.org/~jorendorff/es6-draft.html) for full specification of the ECMAScript 6 language.

Veja o [proposta de padrão ES6] (https://people.mozilla.org/ ~ jorendorff/es6-draft.html) para a especificação completa da linguagem ECMAScript 6.

ES6 includes the following new features:
ES inclui as seguintes novas funcionalidades:
- [arrows](#arrows)
- [classes](#classes)
- [enhanced object literals](#enhanced-object-literals)
- [template strings](#template-strings)
- [destructuring](#destructuring)
- [default + rest + spread](#default--rest--spread)
- [let + const](#let--const)
- [iterators + for..of](#iterators--forof)
- [generators](#generators)
- [comprehensions](#comprehensions)
- [unicode](#unicode)
- [modules](#modules)
- [module loaders](#module-loaders)
- [map + set + weakmap + weakset](#map--set--weakmap--weakset)
- [proxies](#proxies)
- [symbols](#symbols)
- [subclassable built-ins](#subclassable-built-ins)
- [promises](#promises)
- [math + number + string + object APIs](#math--number--string--object-apis)
- [binary and octal literals](#binary-and-octal-literals)
- [reflect api](#reflect-api)
- [tail calls](#tail-calls)

## ECMAScript 6 Features
## Funcionalidades ECMAScript 6

### Arrows
Arrows are a function shorthand using the `=>` syntax.  They are syntactically similar to the related feature in C#, Java 8 and CoffeeScript.  They support both expression and statement bodies.  Unlike functions, arrows share the same lexical `this` as their surrounding code.

_Arrows_ são abreviações de func'ões usando a sintaxe `=>`. Elas são semelhantes semanticamente  ao recurso equivalente do C#, Java 8 e do CoffeeScript. Elas supotam expressões ou conjuntos de instruções. Ao conrário de _function_, _arrows_ compartilham o mesmo `this` que o do código que o envolve.

```JavaScript
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
// Expressões
var oares   = evens.map(v => v + 1);
var numeros = evens.map((v, i) => v + i);

// Statement bodies
// Conjuntos de instruções
numeros.forEach(v => {
  if (v % 5 === 0)
    listaCincos.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
// "this" léxico
var joao = {
  _nome: "João",
  _amigos: [],
  listaAmigos() {
    this._amigos.forEach(f =>
      console.log( this._nome + " conhece " + f) );
  }
}
```

### Classes

ES6 classes are a simple sugar over the prototype-based OO pattern.  Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability.  Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

Classes ES6 são o padrão _prototype_ melhorado. Ter uma única e conveniente forma declarativa fazem os padrões de classes mais fáceis de usar e icentivam a interoperabilidade. Classes suportam herança no modelo _ prototype, _super calls_, instanciamento, métodos estáticos e construtores.

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### Enhanced Object Literals
### Object Literals aprimorados

Object literals are extended to support setting the prototype at construction, shorthand for `foo: foo` assignments, defining methods and making super calls.  Together, these also bring object literals and class declarations closer together, and let object-based design benefit from some of the same conveniences.

_Object literals_  foram extendidos para facilitar definir o _prototype_ construção, uma abreviação para definições `foo: foo` , definir métodos e fazer _super calls_. Juntos, eles também trazem literais de objeto e declarações de classe mais próximos, e deixam  benefício design baseado em objeto de algumas das mesmas conveniências.

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

### Template Strings
Template strings provide syntactic sugar for constructing strings.  This is similar to string interpolation features in Perl, Python and more.  Optionally, a tag can be added to allow the string construction to be customized, avoiding injection attacks or constructing higher level data structures from string contents.
Template strings tornam muito fácil gerar _strings_. Ë semelhante à interpolação de _strings_ do Perl, Python e outros. Opcionalmente podmeos adicionar uma _tag_ que permitem a construção de uma string personalizada, evitando ataques com inserção de código ou montando estruturas de dados a partir do conteúdo de _strings_.

```JavaScript
// Basic literal string creation
`In JavaScript '\n' is a line-feed.`
// Construção básica de uma string literal
`Em javascript '\n' é uam quebra de linha.`

// Multiline strings
`In JavaScript this is
 not legal.`
// Strings multilinha
`Isto não funciona
 em Javascript.`

// Construct a DOM query
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
// Contruindo uma query ao DOM
`Olá ${nome}, como está o ${nome_amigo}?`

// Montar um prefixo de requisição HTTP para interpretar as substituições e construção
// Construct an HTTP request prefix is used to interpret the replacements and construction
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### Destructuring
Destructuring allows binding using pattern matching, with support for matching arrays and objects.  Destructuring is fail-soft, similar to standard object lookup `foo["bar"]`, producing `undefined` values when not found.
Destructuring permite vincular usando padrões de expressões regulares, com suporte à _arrays_ e _objects_.  Destructuring é _fail-soft_, semelhante à busca padrão em objetos `foo["bar"]`, retornando o valor  `undefined`  quando não encontra.

```JavaScript
// list matching
var [a, , b] = [1,2,3];

// object matching
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;
```

### Default + Rest + Spread
Callee-evaluated default parameter values.  Turn an array into consecutive arguments in a function call.  Bind trailing parameters to an array.  Rest replaces the need for `arguments` and addresses common cases more directly.
Valores padrão nas chamadas de funções.  Transformação de um array em argumentos consegutivos em uma chamada de função, Vincular parametros sequenciais em um _array_. Substitui a necessidade de _arguments_ e  casos mais comuns.

```JavaScript
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  // y vale 12 se não for passado (ou passado como undefined)
  return x + y;
}
f(3) == 15
```
```JavaScript
function f(x, ...y) {
  // y é um Array
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// Passa cada emelento do array como um argumentoargument
f(...[1,2,3]) == 6
```

### Let + Const
Block-scoped binding constructs.  `let` is the new `var`.  `const` is single-assignment.  Static restrictions prevent use before assignment.
Blocos com escopo vinculado. `let` é o novo `var`.  `const` é definido uam evz apenas.  Restrições estáticas previnem o uso antes da declaração.


```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      // funciona, nome com escopo definido no bloco
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    // erro, já definido no bloco
    let x = "inner";
  }
}
```

### Iterators + For..Of
Iterator objects enable custom iteration like CLR IEnumerable or Java Iteratable.  Generalize `for..in` to custom iterator-based iteration with `for..of`.  Don’t require realizing an array, enabling lazy design patterns like LINQ.
Objetos Iterator permitem  iterações como CLR IEnumerable ou Java Iteratable.  Generalizar o `for..in`  para uma iteração customizada com `for..of`.  Não é necessário executar em um _array_, permitindo padrões mais flexíveis, como LINQ.

```JavaScript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  // para a sequencia em 1000
  if (n > 1000)
    break;
  print(n);
}
```

Iteration is based on these duck-typed interfaces (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):
A sintaxe de _Iteration_ é baseada nas interfaces (usando sintaxe do[TypeScript] (http://typescriptlang.org) para demonstração apenas):
```TypeScript
interface IteratorResult {
  done: boolean;
  value: any;
}
interface Iterator {
  next(): IteratorResult;
}
interface Iterable {
  [Symbol.iterator](): Iterator
}
```

### Generators
Generators simplify iterator-authoring using `function*` and `yield`.  A function declared as function* returns a Generator instance.  Generators are subtypes of iterators which include additional  `next` and `throw`.  These enable values to flow back into the generator, so `yield` is an expression form which returns a value (or throws).
_Generators_ simplificam a criação de iterações usando `function*` e `yield`. Uma func'ão declarada como _funcion*_ retorna uma instancia de um _Generator_. _Generators_ são subtipos de _iterators_ que incluem métodos adicionais,  `next` e `throw`. Eles permitem que valores sejam retornados ao _generator_, então `yield` é uma forma de expressão que retorna um valor.

Note: Can also be used to enable ‘await’-like async programming, see also ES7 `await` proposal.
Nota: Também pode ser usados para permitir 'esperar' como em programação assíncrona, veja também a proposta do ES7, `await`.

```JavaScript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  print(n);
}
```

The generator interface is (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):
A interface do generator é (usando a sintaxe do  [TypeScript](http://typescriptlang.org) para demonstração apenas):

```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

### Comprehensions
Array and generator comprehensions provide simple declarative list processing similar as used in many functional programming patterns.
_Comprehensions_ de arrays e _generator_ fornecem um processamento declarativo simples, semelhante ao usado em muitos padrões de programação funcional.

```JavaScript
// Array comprehensions
var results = [
  for(c of customers)
    if (c.city == "Seattle")
      { name: c.name, age: c.age }
]

// Generator comprehensions
var results = (
  for(c of customers)
    if (c.city == "Seattle")
      { name: c.name, age: c.age }
)
```

### Unicode
Non-breaking additions to support full Unicode, including new unicode literal form in strings and new RegExp `u` mode to handle code points, as well as new APIs to process strings at the 21bit code points level.  These additions support building global apps in JavaScript.
Adições retroativas para suporte completo a Unicode, incluindo a nova forma literal do unicode em strings e o novo modo do RegExp `u`, para lidar com pontos no código, assim como novas APIs para processar _strings_ em códigos 21bit. Essas adições auxiliam a contrução de _apps_ globais em Javascript.

```JavaScript
// same as ES5.1
// o mesmo que no ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
// novo comportamento RegExp, opcionalopt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
// nova sintaxe
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// new String ops
// nova opção para string
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
// for-of itera em pontos de código
for(var c of "𠮷") {
  console.log(c);
}
```

### Modules
Language-level support for modules for component definition.  Codifies patterns from popular JavaScript module loaders (AMD, CommonJS). Runtime behaviour defined by a host-defined default loader.  Implicitly async model – no code executes until requested modules are available and processed.
Suporte nativo para módulos e definição de componentes. Códifica padrões de carregamento de módulo populares (AMD, CommonJS). Comportamento de carregamento em tempo de execução definido por um padrão no _host_. Modelos implícitos assíncronos , nenhum código é executado até que os módulos requisitados estejam disponíveis e processados.

```JavaScript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```
```JavaScript
// app.js
module math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```
```JavaScript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

Some additional features include `export default` and `export *`:
Algumas funcionalidades adicionais incluem `export default` e `export *`:

```JavaScript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.exp(x);
}
```
```JavaScript
// app.js
module math from "lib/mathplusplus";
import exp from "lib/mathplusplus";
alert("2π = " + exp(math.pi, math.e));
```

### Module Loaders
Module loaders support:
- Dynamic loading
- State isolation
- Global namespace isolation
- Compilation hooks
- Nested virtualization
_Module loaders_ suportam:
- carregamento dinâmico
- isolamento de estado
- isolamento de _namespace_ global
- _hooks_ de compilação
- virtualização aninhada 

The default module loader can be configured, and new loaders can be constructed to evaluated and load code in isolated or constrained contexts.
O _module loader_ padrão pode ser configurado e novos _loaders_ podem ser contruídos para avaliar e carregar código em contexto isolado ou restrito

```JavaScript
// Dynamic loading – ‘System’ is default loader
// Carregamento dinamico – ‘System’ é o loader padrão
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
// Cria sandboxes de execução – novos Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
// Manipulação do módulo cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
System.set('jquery', Module({$: $})); // WARNING: não finalizado ainda
```

### Map + Set + WeakMap + WeakSet
Efficient data structures for common algorithms.  WeakMaps provides leak-free object-key’d side tables.
Estruturas de dados eficientes para algorítimos comuns. _WeakMaps_ fornecem um mapa seguro de objeto-chave.

```JavaScript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// Because the added object has no other references, it will not be held in the set
```

### Proxies
Proxies enable creation of objects with the full range of behaviors available to host objects.  Can be used for interception, object virtualization, logging/profiling, etc.

```JavaScript
// Proxying a normal object
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';
```

```JavaScript
// Proxying a function object
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

There are traps available for all of the runtime-level meta-operations:

```JavaScript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

### Symbols
Symbols enable access control for object state.  Symbols allow properties to be keyed by either `string` (as in ES5) or `symbol`.  Symbols are a new primitive type. Optional `name` parameter used in debugging - but is not part of identity.  Symbols are unique (like gensym), but not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.


```JavaScript
(function() {

  // module scoped symbol
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

})();

var c = new MyClass("hello")
c["key"] === undefined
```

### Subclassable Built-ins
In ES6, built-ins like `Array`, `Date` and DOM `Element`s can be subclassed.

Object construction for a function named `Ctor` now uses two-phases (both virtually dispatched):
- Call `Ctor[@@create]` to allocate the object, installing any special behavior
- Invoke constructor on new instance to initialize

The known `@@create` symbol is available via `Symbol.create`.  Built-ins now expose their `@@create` explicitly.

```JavaScript
// Pseudo-code of Array
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// User code of Array subclass
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// Two-phase 'new':
// 1) Call @@create to allocate object
// 2) Invoke constructor on new instance
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### Math + Number + String + Object APIs
Many new library additions, including core Math libraries, Array conversion helpers, and Object.assign for copying.

```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".contains("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1,2,3].findIndex(x => x == 2) // 1
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

### Binary and Octal Literals
Two new numeric literal forms are addded for binary (`b`) and octal (`o`).

```JavaScript
0b111110111 === 503 // true
0o767 === 503 // true
```

### Promises
Promises are a library for asynchronous programming.  Promises are a first class representation of a value that may be made available in the future.  Promises are used in many existing JavaScript libraries.

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

### Reflect API
Full reflection API exposing the runtime-level meta-operations on objects.  This is effectively the inverse of the Proxy API, and allows making calls corresponding to the same meta-operations as the proxy traps.  Especially useful for implementing proxies.

```JavaScript
// No sample yet
```

### Tail Calls
Calls in tail-position are guaranteed to not grow the stack unboundedly.  Makes recursive algorithms safe in the face of unbounded inputs.

```JavaScript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Stack overflow in most implementations today,
// but safe on arbitrary inputs in eS6
factorial(100000)
```
