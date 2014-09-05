# ECMAScript 6 <sup>[git.io/es6features](http://git.io/es6features)</sup>

## Introduction

ECMAScript 6 é a próxima versão do padrão ECMAScript. A norma deverá ser ratificada em Dezembro de 2014. ES6 é um upgrade significativo na linguagem, é a primeira atualização desde que foi padronizada em 2009. Veja a implementação dessas funcionalidades nas principais engines de javascript no link [Tabela de Compatibilidade do ECMAScript 6](http://kangax.github.io/es5-compat-table/es6/).

Veja a [proposta de padrão ES6] (https://people.mozilla.org/ ~ jorendorff/es6-draft.html) para a especificação completa da linguagem ECMAScript 6.

ES6 inclui as seguintes novas funcionalidades:
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

## Funcionalidades ECMAScript 6

### Arrows

_Arrows_ são abreviações de funções usando a sintaxe `=>`. Elas são semelhantes semanticamente  ao recurso equivalente do C#, Java 8 e do CoffeeScript. Elas supotam expressões ou conjuntos de instruções. Ao conrário de _function_, _arrows_ compartilham o mesmo `this` que o do código que o envolve.

```JavaScript
// Expressões
var pares   = evens.map(v => v + 1);
var numeros = evens.map((v, i) => v + i);

// Conjuntos de instruções
numeros.forEach(v => {
  if (v % 5 === 0)
    listaCincos.push(v);
});

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

Classes ES6 são o padrão _prototype_ melhorado. Ter uma única e conveniente forma declarativa fazem os padrões de classes mais fáceis de usar e icentivam a interoperabilidade. Classes suportam herança no modelo _ prototype, _super calls_, instânciamento, métodos estáticos e construtores.

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

### Object Literals aprimorados

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
Template strings tornam muito fácil gerar _strings_. É semelhante à interpolação de _strings_ do Perl, Python e outros. Opcionalmente podemos adicionar uma _tag_ que permitem a construção de uma string personalizada, evitando ataques com inserção de código ou montando estruturas de dados a partir do conteúdo de _strings_.

```JavaScript
// Construção básica de uma string literal
`Em javascript '\n' é uam quebra de linha.`

// Strings multilinha
`Isto não funciona
 em Javascript.`

// Contruindo uma query ao DOM
`Olá ${nome}, como está o ${nome_amigo}?`

// Montar um prefixo de requisição HTTP para interpretar as substituições e construção
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### Destructuring
Destructuring permite vincular usando padrões de expressões regulares, com suporte à _arrays_ e _objects_.  Destructuring é _fail-soft_, semelhante à busca padrão em objetos `foo["bar"]`, retornando o valor  `undefined`  quando não encontrado.

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
Valores padrão nas chamadas de funções.  Transformação de um array em argumentos consecutivos em uma chamada de função, Víncular parametros sequênciais em um _array_. Substitui a necessidade de _arguments_ e  casos mais comuns.

```JavaScript
function f(x, y=12) {
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
// Passa cada emelento do array como um argumento
f(...[1,2,3]) == 6
```

### Let + Const
Blocos com escopo vinculado. `let` é o novo `var`.  `const` é definido uam vez apenas.  Restrições estáticas previnem o uso antes da declaração.


```JavaScript
function f() {
  {
    let x;
    {
      // funciona, nome com escopo definido no bloco
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // erro, já definido no bloco
    let x = "inner";
  }
}
```

### Iterators + For..Of
Objetos Iteratores permitem  iterações como CLR IEnumerable ou Java Iteratable.  Generalizar o `for..in`  para uma iteração customizada com `for..of`.  Não é necessário executar em um _array_, permitindo padrões mais flexíveis, como LINQ.

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

A sintaxe de _Iteration_ é baseada nas interfaces (usando sintaxe do[TypeScript](http://typescriptlang.org) para demonstração apenas):
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
_Generators_ simplificam a criação de iterações usando `function*` e `yield`. Uma função declarada como _function*_ retorna uma instância de um _Generator_. _Generators_ são subtipos de _iterators_ que incluem métodos adicionais,  `next` e `throw`. Eles permitem que valores sejam retornados ao _generator_, então `yield` é uma forma de expressão que retorna um valor.

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

A interface do generator é (usando a sintaxe do  [TypeScript](http://typescriptlang.org) para demonstração apenas):

```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

### Comprehensions
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
Adições retroativas para suporte completo a Unicode, incluindo a nova forma literal do unicode em strings e o novo modo do RegExp `u`, para lidar com pontos no código, assim como novas APIs para processar _strings_ em códigos 21bit. Essas adições auxiliam a contrução de _apps_ globais em Javascript.

```JavaScript
// o mesmo que no ES5.1
"𠮷".length == 2

// novo comportamento RegExp, opcionalopt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// nova sintaxe
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// nova opção para string
"𠮷".codePointAt(0) == 0x20BB7

// for-of itera em pontos de código
for(var c of "𠮷") {
  console.log(c);
}
```

### Modules
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
_Module loaders_ suportam:
- carregamento dinâmico
- isolamento de estado
- isolamento de _namespace_ global
- _hooks_ de compilação
- virtualização aninhada 

O _module loader_ padrão pode ser configurado e novos _loaders_ podem ser contruídos para avaliar e carregar código em contexto isolado ou restrito

```JavaScript
// Carregamento dinamico – ‘System’ é o loader padrão
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Cria sandboxes de execução – novos Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Manipulação do módulo cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
System.set('jquery', Module({$: $})); // WARNING: não finalizado ainda
```

### Map + Set + WeakMap + WeakSet
Estruturas de dados eficientes para algorítimos comuns. _WeakMaps_ fornecem um mapa seguro de <objeto-chave class=""></objeto-chave>

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
_Proxies_ permitem a criação de objetos com todos os comportamentos disponíveis no objeto que o contém, Podem ser usados para interceptação, virtualização de objetos, _logs_, _profiles_, etc.

```JavaScript
// Proxying a normal object
// Proxy em um objeto normal
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
// Proxy em  um objeto função
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

Existem métodos disponíveis para todas as meta-operações em tempo de execução:

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
_Symbols_ permitem controle sobre o estado do objeto. _Symbols_ permitem que propriedades sejam indexadas tanto por _string_ (como no ES5) como _symbol_. _Symbols_ são um novo tipo primitivo. Parâmetro opcional, `name` , usado em _debug_, mas não é parte da identidade. _Symbols_ são únicos (como gensym), mas não são privados já que são expostos em funcionalidades como`Object.getOwnPropertySymbols`.

```JavaScript
(function() {

  // symbol no escopo do módulo
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
No ES6, objetos nativos como `Array`, `Date` e DOM `Element`s podem ter subclasses.

Contrução de objetos para uma função chamada  `Ctor` agora levam duas etapas (ambas disparadas virtualmente):
- Chame `Ctor[@@create]` para alocar o objeto, instalando qualquer comportamento especial
- Chame o construtor ou uma nova instância para inicializar

O simbolo `@@create`  estádisponível través de  `Symbol.create`.  Objetos nativos agora expôem seus métodos `@@create` explicitamente..

```JavaScript
// Pseudo-código de Array
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

// 'new' em duas fases:
// 1) Chame @@create para alocar o obejto
// 2) Chame o construtctor na nova instancia
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### Math + Number + String + Object APIs
Muitas novas adições, incluindo novas _libraries_ de matemática, conversão de _Arrays_ e _Object.assign_ para cópias.

```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".contains("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // retorna um Array verdadeiro
Array.of(1, 2, 3) // Semelhante a new Array(...), mas sem comportamento especial de um argumento
[0, 0, 0].fill(7, 1) // [0,7,7]
[1,2,3].findIndex(x => x == 2) // 1
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

### Binary and Octal Literals
Duas novas formas de numeros literais foram adicionadas, para binários (`b`) e octais (`o`).

```JavaScript
0b111110111 === 503 // true
0o767 === 503 // true
```

### Promises
_Promises_ são uma _library_ para programação assíncrona. _Promises_ são representações de primeira classe de um valor que pode estar disponível no futuro. _Promises_ são usadas em muitas _libraries_ javascript que já existem.

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
A _Reflection API_  expõe as meta-operações em objetos em tempo de execução. De fato isto é o inverso da _Proxy API_, e permite fazer chamadas correspondentes as mesmas meta-operações que os métodos de _proxy_. Muito úteis pata implementação de _proxies_.

```JavaScript
// Sem exemplo
```

### Tail Calls
Chamadas no final são garantidas para não aumentar a fila ilimitadamente.
Fazem algorítimos recursivos  mais seguros para dados inesperados.

```JavaScript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Estouro de memória na maioria das implentações de hoje,
// mas seguro em entradas de dados aleatória no ES6
factorial(100000)
```
