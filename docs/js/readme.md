# Convention Javascript

<!-- MarkdownTOC -->

- [Exemple](#exemple)
- [Convention simplifiée](#convention-simplifiée)
    - [Objets](#objets)
    - [Tableaux](#tableaux)
    - [Strings](#strings)
    - [Fonctions](#fonctions)
    - [Fonctions fléchées](#fonctions-fléchées)
    - [Classes et constructeurs](#classes-et-constructeurs)
    - [Modules](#modules)
    - [Propriétés](#propriétés)
    - [Variables](#variables)
    - [Comparaison](#comparaison)
    - [Blocks](#blocks)
    - [Virgules](#virgules)
    - [Nommage](#nommage)
    - [JQuery](#jquery)
- [Références](#références)

<!-- /MarkdownTOC -->

## Exemple

**Pigeon.js** - *Le nom du fichier est le nom de export par défaut*
```js
export const acceptedFood = ['bread', 'seed'];

export default class Pigeon {
    // Use es6 default parameters, no parameter reassignement
    constructor(name, foods = acceptedFood) {
        this.name = name;
        this.acceptedFoods = foods;
        this.isFree = false;
    }

    setFree() {
        this.isFree = true;

        // By default use const, no var
        const willReturn = new Promise((resolve, reject) => {
            // ...
        });

        willReturn.then(() => {
            this.isFree = false;
        });

        return willReturn;
    }
}
```

**PigeonController.js**
```js
// Place imports at the top
// One import line by imported files
import Pigeon, { acceptedFood } from './Pigeon';
import PigeonStore from './PigeonStore';

export default class PigeonController {
    constructor() {
        this.pigeonStore = new PigeonStore();

        pigeonStore.add(new Pigeon('Bob'));
        pigeonStore.add(new Pigeon('Seraphin', [
            // Create new arrays using the spread operator (same for objects)
            ...acceptedFood,
            'wheat',
        ]));

        this.freePigeons(this.pigeonStore.all())
            .then((pigeons) => {
                console.log('All pigeons came back!');
            }).catch((error) => {
                console.log('We lost some pigeons!', error);
            });
    }

    freePigeons(pigeons) {
        // Group let together
        let flewAway = []; // Use array literal [] not new Array()
        let flewAwayCount = 0;
        // Group const together
        const limit = 3;
        // Save jQuery in a $ variable
        const $body = $('body');

        // Prefer Array functions (map, forEach, some, every, etc...) to iterate
        // instead of Iterators for in, for of, while, etc...
        pigeons.some((pigeon) => { // Prefer () => {} shorthand functions for callbacks
            flewAway.push(pigeon.setFree());
            flewAwayCount++;

            return limit > 0 && flewAwayCount > limit; // Kills the loop when true returned
        });

        // Use string templates to concatenate variables and strings
        $body.append(`${flewAwayCount} out of ${pigeons.length} pigeons flew away`);

        return Promise.all(flewAway);
    }
}

```

## Convention simplifiée

Les points suivants sont extraits de la convention [Airbnb](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6) que nous suivons à l'exception de l'indentation qui est à 4 espaces ici.

Pour une liste exhaustive des recommandations lisez la convention [Airbnb](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6) et aussi les articles référencés dans la convention. Mais prévoyez quelques soirées pour digérer tout çà.

### Objets

<details><summary>[3.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#objects--no-new) Utilisez la déclaration litérale. [no-new-object](http://eslint.org/docs/rules/no-new-object.html)</summary><p>
```js
// Bad
var myObject = new Object();
var myObject = new Object;

// Good
var myObject = new CustomObject();
var myObject = {};
```
</p></details>

<details><summary>[3.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-object-shorthand) [3.4](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-object-concise) Utilisez l'assignation raccourcie des variables et des propriétés d'objets. [object-shorthand](http://eslint.org/docs/rules/object-shorthand.html)</summary><p>
```js
// Bad
var foo = {
    x: x,
    y: y,
    z: z,
    a: function() {},
    b: function() {}
};

// Good
var foo = {
    x,
    y,
    z,
    a() {},
    b() {}
};
```
</p></details>

<details><summary>[3.6](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#objects--quoted-props) Ne mettez des noms de propriétés entre guillemets qui si elles sont invalides sinon. [quote-props](http://eslint.org/docs/rules/quote-props.html)</summary><p>
```js
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```
</p></details>

<details><summary>[3.8](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#objects--rest-spread) Préférez l'opérateur de décomposition {...obj} à Object.assign()</summary><p>
```js
// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```
</p></details>

### Tableaux

<details><summary>[4.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#arrays--literals) Utilisez la déclaration litérale. [no-array-constructor](http://eslint.org/docs/rules/no-array-constructor.html)</summary><p>
```js
// bad
const items = new Array();

// good
const items = [];
```
</p></details>

<details><summary>[4.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-array-spreads) Utilisez l'opérateur de décomposition pour copier des tableaux</summary><p>
```js
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
</p></details>

### Strings

<details><summary>[6.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#strings--quotes) Utilisez des guillemets simples. [quotes](http://eslint.org/docs/rules/quotes)</summary><p>
```js
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```
</p></details>

<details><summary>[6.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-template-literals) Utilisez des templates pour concatener des variables et des strings. [prefer-template template-curly-spacing](http://eslint.org/docs/rules/template-curly-spacing)</summary><p>
```js
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```
</p></details>

<details><summary>[6.4](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#strings--eval) N'utilisez pas eval(). [no-eval](http://eslint.org/docs/rules/no-eval)</summary><p>
**Bad**
```js
var obj = { x: "foo" },
    key = "x",
    value = eval("obj." + key);

(0, eval)("var a = 0");

var foo = eval;
foo("var a = 0");

// This `this` is the global object.
this.eval("var a = 0");
```
</p></details>

### Fonctions

<details><summary>[7.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#functions--declarations) Utilisez des expressions de fonctions nommées au lieu de la déclaration function () {...}. [func-style](http://eslint.org/docs/rules/func-style), [no-use-before-define](http://eslint.org/docs/rules/no-use-before-define)</summary><p>
> Les déclarations function() {...} sont automatiquement remontées et déclarée en haut de leur scope ce qui signifie qu'elles peuvent être appelées avant d'être déclarée. C'est mauvais pour la lisibilité. Aussi si votre fonction devient importante et nuit à la lisibilité vous pouvez la placer dans un module dédié et l'importer.

```js
foo();
// Throws error foo is not defined

foobar();
// Throws error foobar is not defined

foobarInner();
// Throws error foobarInner is not defined

teapot();
// teapot

// Good
const foo = function() {
  console.log('foo');
  console.thisisanerror();
}

// Good
const foobar = function foobarInner() {
  console.log('foobar');
  console.thisisanerror();
}

// Bad
function teapot() {
  console.log('teapot');
}
```

Notez qu'avec l'utilisation de `var` au lieu de `const` ou `let` l'erreur serait différent. Nous aurions `xxx is not a function`.
Voir [Why is typeof no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)
</p></details>

<details><summary>[7.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#functions--in-blocks) Ne déclarez jamais une fonction dans un bloc autre qu'une fonction (ie: if, while, for, etc...). [no-loop-func](http://eslint.org/docs/rules/no-loop-func.html)</summary><p>
> C'est techniquement possible mais les navigateurs l'interprètent différemment.

</p></details>

<details><summary>[7.6](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-rest) Préférez le paramètre du reste `...` à arguments. [prefer-rest-params](http://eslint.org/docs/rules/prefer-rest-params)</summary><p>
> Pourquoi ? ... est plus explicite et renvoie un objet Array et non un Array-like comme `arguments`.
```js
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```
</p></details>

<details><summary>[7.7](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#es6-default-parameters) [7.12](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#functions--mutate-params) [7.13](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#functions--reassign-params) Utilisez des paramètres par défaut plutôt que de réassigner une valeur sur un paramètre. [no-param-reassign](http://eslint.org/docs/rules/no-param-reassign)</summary><p>
```js
// really bad
function handleThings(opts) {
  // No! We shouldn't mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```
</p></details>

<details><summary>[7.11](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#functions--signature-spacing) Ajoutez un espace après `function` et après les parenthèses des arguments. [space-before-function-paren](http://eslint.org/docs/rules/space-before-function-paren) [space-before-blocks](http://eslint.org/docs/rules/space-before-blocks)</summary><p>
```js
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```
</p></details>

### Fonctions fléchées

<details><summary>[8.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#arrows--use-them) Quand une fonction doit-être utilisée en paramètre, préférez les fonctions fléchées. [prefer-arrow-callback](http://eslint.org/docs/rules/prefer-arrow-callback.html)</summary><p>
```js
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```
</p></details>

### Classes et constructeurs

<details><summary>[9.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#constructors--use-class) Utilisez toujours des classes, évitez de travailler avec `prototype`.</summary><p>
```js
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};


// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```
</p></details>

### Modules

<details><summary>[10.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#modules--use-them) Utilisez toujours `import/export` contre n'importe quel autre système de module (ex: `modules.exports`)</summary><p>
> Pourquoi ? Les modules sont le futurs et sont standards
```js
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```
</p></details>

<details><summary>[10.4](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#modules--no-duplicate-imports) Une ligne d'import par fichier, combinez les importe. [no-duplicate-imports](http://eslint.org/docs/rules/no-duplicate-imports)</summary><p>
```js
// bad
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo';

// good
import foo, { named1, named2 } from 'foo';

// good
import foo, {
named1,
named2,
} from 'foo';
```
</p></details>

<details><summary>[10.6](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#modules--prefer-default-export) Dans des modules n'ayant qu'un seul export, préférez l'export `default` plutôt qu'un export nommé. [import/prefer-default-export](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)</summary><p>
```js
// bad
export function foo() {}

// good
export default function foo() {}
```
</p></details>

<details><summary>[10.7](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#modules--imports-first) Placez tous les import en haut du fichier. [import/first](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)</summary><p>
```js
// bad
import foo from 'foo';
foo.init();

import bar from 'bar';

// good
import foo from 'foo';
import bar from 'bar';

foo.init();
```
</p></details>

<details><summary>[10.9](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#modules--no-webpack-loader-syntax) N'utilisez pas la syntaxe des loaders webpack dans les imports. [import/no-webpack-loader-syntax.md](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)</summary><p>
```js
// bad
import fooSass from 'css!sass!foo.scss';
import barCss from 'style!css!bar.css';

// good
import fooSass from 'foo.scss';
import barCss from 'bar.css';
```
</p></details>

### Propriétés

<details><summary>[12.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#properties--dot) Utilisez la notation avec point pour accéder aux propriétés d'objets. [dot-notation](http://eslint.org/docs/rules/dot-notation.html)</summary><p>
```js
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```
</p></details>

### Variables

<details><summary>[13.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#variables--const) Utilisez toujours `const` d'abord, `let` si la variable a une réassignation après la déclaration. [no-undef](http://eslint.org/docs/rules/no-undef), [prefer-const](http://eslint.org/docs/rules/prefer-const)</summary><p>
```js
// Bad
// it's initialized and never reassigned. Use const.
let a = 3;
console.log(a);

// Good
let a;
a = 0;
console.log(a);

// Bad, `i` is redefined (not reassigned) on each loop step.
for (let i in [1, 2, 3]) {
    console.log(i);
}

// Bad, `a` is redefined (not reassigned) on each loop step.
for (let a of [1, 2, 3]) {
    console.log(a);
}

// using const.
const a = 0;

// it's never initialized.
let a;
console.log(a);

// it's reassigned after initialized.
let a;
a = 0;
a = 1;
console.log(a);

// it's initialized in a different block from the declaration.
let a;
if (true) {
    a = 0;
}
console.log(a);

// it's initialized at a place that we cannot write a variable declaration.
let a;
if (true) a = 0;
console.log(a);

// `i` gets a new binding each iteration
for (const i in [1, 2, 3]) {
  console.log(i);
}

// `a` gets a new binding each iteration
for (const a of [1, 2, 3]) {
  console.log(a);
}

// `end` is never reassigned, but we cannot separate the declarations without modifying the scope.
for (let i = 0, end = 10; i < end; ++i) {
    console.log(a);
}

// Never use var
var b = 3;
console.log(b);
```
</p></details>

<details><summary>[13.2](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#variables--one-const) Utilisez un const / let par variable. [one-var](http://eslint.org/docs/rules/one-var.html)</summary><p>
```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```
</p></details>

<details><summary>[13.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#variables--const-let-group) Groupez tous vos `const` et groupez tous vos `let`</summary><p>
```js
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```
</p></details>

<details><summary>N'utilisez jamais `var`. [no-var](http://eslint.org/docs/rules/no-var)</summary><p>
> Pourquoi ?<br>
> var ne respecte pas les scope de if, while, for, etc... Une variable déclarée dans un if se retrouve présente au niveau du scope de fonction parent. let et const respecte tous les scope.

```js
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```
</p></details>

### Comparaison

<details><summary>[15.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#comparison--eqeqeq) Utilisez === et !== au lieu de == et !=. [eqeqeq](http://eslint.org/docs/rules/eqeqeq.html)</summary><p>
Voir [Truth, Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)
</p></details>

### Blocks

<details><summary>[16.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#blocks--braces) Utilisez des accolades pour tous les blocks multi-ligne.</summary><p>
```js
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```
</p></details>

<details><summary>[16.2](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#blocks--cuddled-elses) else sur la même ligne que la fermeture du if. [brace-style](http://eslint.org/docs/rules/brace-style.html)</summary><p>
```js
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```
</p></details>

### Virgules

<details><summary>[19.2](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#commas--dangling) Ajoutez une virgule au dernier éléments de vos objets, array et paramètres de fonctions. [comma-dangle](http://eslint.org/docs/rules/comma-dangle.html)</summary><p>
```js
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully'
};

const heroes = [
  'Batman',
  'Superman'
];

// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
};

const heroes = [
  'Batman',
  'Superman',
];

// bad
function createHero(
  firstName,
  lastName,
  inventorOf
) {
  // does nothing
}

// good
function createHero(
  firstName,
  lastName,
  inventorOf,
) {
  // does nothing
}

// good (note that a comma must not appear after a "rest" element)
function createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
) {
  // does nothing
}

// bad
createHero(
  firstName,
  lastName,
  inventorOf
);

// good
createHero(
  firstName,
  lastName,
  inventorOf,
);

// good (note that a comma must not appear after a "rest" element)
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
);
```
</p></details>

### Nommage

<details><summary>[22.2](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#naming--camelCase) Utilisez du camelCase pour les noms de fonctions. [camelcase](http://eslint.org/docs/rules/camelcase.html)</summary><p>
```js
function makeStyleGuide() {
  // ...
}

export default makeStyleGuide;
```
</p></details>

<details><summary>[22.3](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#naming--PascalCase) Utilisez du PascalCase pour exporter des constructeurs, classes, singleton, objets ou librairies. [new-cap](http://eslint.org/docs/rules/new-cap)</summary><p>
```js
const AirbnbStyleGuide = {
  es6: {
  },
};

export default AirbnbStyleGuide;
```
</p></details>

<details><summary>[22.5](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#naming--self-this) Ne pas sauvegarder de référence à this. Utilisez les fonctions fléchées (() => {})</summary><p>
```js
// bad
export function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// bad
export function foo() {
  const that = this;
  return function () {
    console.log(that);
  };
}

// good
export function foo() {
  return () => {
    console.log(this);
  };
}
```
</p></details>

<details><summary>[22.6](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#naming--filename-matches-export) Le nom d'un fichier doit correspondre exactement au nom de son export par défaut</summary><p>
```js
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```
</p></details>

### JQuery

<details><summary>[25.1](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#jquery--dollar-prefix) Préfixez les objets jquery de `$`</summary><p>
```js
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```
</p></details>

<details><summary>[25.2](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6#jquery--cache) Sauvegardez les appels à jQuery</summary><p>
```js
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...

  $('.sidebar').css({
    'background-color': 'pink',
  });
}

// good
function setSidebar() {
  const $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...

  $sidebar.css({
    'background-color': 'pink',
  });
}
```
</p></details>

## Références

- [Airbnb Javascript Style Guide](https://github.com/airbnb/javascript/tree/7b7ffc533d9b509384a0c9923a03544003c9cde6)
- [Why typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)