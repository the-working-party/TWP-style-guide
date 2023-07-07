# Javascript

Work in progress

## Content

- [Basic rules](#basic-rules)
- [Other style guides](#other-style-guides)

<br>
<br>

---

## Basic rules

### Use semicolons

```js
// Wrong
var foo = 'bar

// Right
var foo = 'bar';
```

### Use single quotes

```js
// Wrong
var foo = "bar";

// Right
var foo = 'bar';
```

### Opening braces go on the same line

```js
// Wrong
if (true)
{
  console.log('losing');
}

// Right
if (true) {
  console.log('winning');
}
```

### Declare one variable per statement

```js
// Wrong
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (keys.length) {
  key = keys.pop();
  object[key] = values.pop();
}

// Right
var keys = ['foo', 'bar'];
let values = [23, 42];

const object = {};
while (keys.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

### Use lowerCamelCase for variables, properties and function names

```js
// Wrong
var admin_user = db.query('SELECT * FROM users ...');

// Right
var adminUser = db.query('SELECT * FROM users ...');
```

### Use UpperCamelCase for class names

```js
// Wrong
class bank_Account() {}
const bank_Account = (props) => {}

// Right
class BankAccount {}
const BankAccount = (props) => {}
```

### Use SCREAMING_SNAKE_CASE for constants

```js
// Wrong
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;

// Right
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

### Object / Array declaration

```js
// Wrong
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };

// Right
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```

### Use the strict equality operator

```js
// Wrong
var a = 0;
if (a == '') {
}

// Right
var a = 0;
if (a !== '') {
}
```

### Ternary operator (???)

> Should this be a hard rule?

Use single-line ternary operators for short expressions only. Split it up into multiple lines otherwise.

```js
// Wrong
var bar = (a === b) ? anIncrediblyLong.map((expression) => expression.whichWillTake === aHugeSpace) : false;

// Right
var foo = (a === b) ? 1 : 2;
var bar = (a === b)
  ? anIncrediblyLong.map((expression) => expression.whichWillTake === aHugeSpace)
  : false;
```

### Do not extend built-in prototypes (???)

> Should this even be here? Who does this nowadays?

```js
// Wrong
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
}

// Right
var a = [];
if (!a.length) {
}
```

### Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

```js
// Wrong
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
}

// Right
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);
if (isValidPassword) {
}
```

### Return early / Use guard clauses

To avoid deep nesting of if-statements, always return a function's value as early as possible.

```js
// Wrong
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}

// Right
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
// Great
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

### Name your closures (???)

> Are are keeping this? I don't remember seeing any of our code using this rule.

Feel free to give your closures a name. It shows that you care about them, and
will produce better stack traces, heap and cpu profiles.

```js
// Wrong
req.on('end', function() {
});

// Right
req.on('end', function onEnd() {
});
```

### No nested closures (???)

> Are are keeping this? I don't remember seeing any of our code using this rule.

Use closures, but don't nest them. Otherwise your code will become a mess.

```js
// Wrong
setTimeout(() => {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);

// Right
function afterConnect() {
  console.log('winning');
}

setTimeout(() => {
  client.connect(afterConnect);
}, 1000);
```

### Use slashes for comments (???)

> Is this how we want comments?
> This can be moved to a "general principles" doc.

Use slashes for both single line and multi line comments. Try to write
comments that explain higher level mechanisms or clarify difficult
segments of your code. Don't use comments to restate trivial things.

```js
// Wrong

/* Execute a regex */
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}


// Right

// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}

```

### `Object.freeze`, `Object.preventExtensions`, `Object.seal`, `with`, `eval` (???)

> Apart from eval, is this still a good idea in 2023? Eg: Functional Programming make use of `Object.freeze`

Crazy shit that you will probably never need. Stay away from it.

### Getters and setters (???)

> Is this still a good idea in 2023?

Do not use setters, they cause more problems for people who try to use your
software than they can solve.

Feel free to use getters that are free from [side effects][sideeffect], like
providing a length property for a collection class.

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)

<br>
<br>

---

## Other style guides

1. [Core Principles](/README.md)
2. Javascript
3. [CSS & SASS](/CSS-SASS.md)
4. [Liquid & HTML](/LIQUID-HTML.md)
