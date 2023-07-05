# TWP Style Guide

This is a guide for writing consistent and aesthetically pleasing code.

This guide was created by [Felix Geisendörfer](http://felixge.de/) and is
licensed under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## Content

- [General Rules](#general-rules)
- [Javascript](#javascript)
- [CSS](#css)

---

## General Rules

- 2 spaces for indentation
- 120 characters per line
- newline at the end of a file (UNIX-style `\n`)
- no trailing whitespace

---

## Javascript

### Use semicolons

### Use single quotes

```js
// Right
var foo = 'bar';

// Wrong
var foo = "bar";
```

### Opening braces go on the same line

```js
// Right
if (true) {
  console.log('winning');
}

// Wrong
if (true)
{
  console.log('losing');
}
```

### Declare one variable per statement

```js
// Right
var keys = ['foo', 'bar'];
let values = [23, 42];

const object = {};
while (keys.length) {
  var key = keys.pop();
  object[key] = values.pop();
}

// Wrong
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (keys.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

### Use lowerCamelCase for variables, properties and function names

```js
// Right
var adminUser = db.query('SELECT * FROM users ...');

// Wrong
var admin_user = db.query('SELECT * FROM users ...');
```

### Use UpperCamelCase for class names

```js
// Right
class BankAccount {}
const BankAccount = (props) => {}

// Wrong
class bank_Account() {}
const bank_Account = (props) => {}
```

### Use SCREAMING_SNAKE_CASE for constants

```js
// Right
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;

// Wrong
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

### Object / Array declaration

```js
// Right
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};

// Wrong
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

### Use the strict equality operator

```js
// Right
var a = 0;
if (a !== '') {
}

// Wrong
var a = 0;
if (a == '') {
}
```

### Ternary operator (???)

> Should this be a hard rule?

Use single-line ternary operators for short expressions only. Split it up into multiple lines otherwise.

```js
// Right
var foo = (a === b) ? 1 : 2;
var bar = (a === b)
  ? 1
  : 2;

var daz = (a === b)
  ? anIncrediblyLong.map((expression) => expression.whichWillTake === aHugeSpace)
  : false;

// Wrong
var bar = (a === b) ? anIncrediblyLong.map((expression) => expression.whichWillTake === aHugeSpace) : false;
```

### Do not extend built-in prototypes (???)

> Should this even be here? Who does this nowadays?

```js
// Right
var a = [];
if (!a.length) {
}

// Wrong
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
}
```

### Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

```js
// Right
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);
if (isValidPassword) {
}

// Wrong
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
}
```

### Write small functions (???)

> This can be moved to a "general principles" doc.
> It shouldn't be a hard rule.

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

### Be a "Never Nester"

> This can be moved to a "general principles" doc.

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

```js
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
// Right
req.on('end', function onEnd() {
});
```

*Wrong:*

```js
// Wrong
req.on('end', function() {
});
```

### No nested closures (???)

> Are are keeping this? I don't remember seeing any of our code using this rule.

Use closures, but don't nest them. Otherwise your code will become a mess.

```js
// Right
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}

// Wrong
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

### Use slashes for comments (???)

> Is this how we want comments?
> This can be moved to a "general principles" doc.

Use slashes for both single line and multi line comments. Try to write
comments that explain higher level mechanisms or clarify difficult
segments of your code. Don't use comments to restate trivial things.

```js
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
```

### `Object.freeze`, `Object.preventExtensions`, `Object.seal`, `with`, `eval` (???)

> Apart from eval, is this still valid in 2023? Eg: Functional Programming uses `Object.freeze`

Crazy shit that you will probably never need. Stay away from it.

### Getters and setters (???)

> Is this still valid in 2023?

Do not use setters, they cause more problems for people who try to use your
software than they can solve.

Feel free to use getters that are free from [side effects][sideeffect], like
providing a length property for a collection class.

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)

---

## CSS

### Class names should be specific

Use class names that are as short as possible but as long as necessary.

```css
/* Right */
.header {
}

.author {
}

.info-box {
}

/* Wrong */
.hd {
}

.atr {
}

.ib {
}
```

### Classes are all lower case

*Right:*

```css
.headline {
}

.head-line {
}
```

*Wrong:*

```css
.headLine {
}

.Headline {
}
```

### Use dash for chaining words in classes

```css
.box-header {
}

.a-really-long-class-name {
}
```

### Prefer data-attribute as selectors in JS

```html
<!-- HTML -->
<button class="button button--primary" data-toggle-button>
  Open menu
</button>
```

```css
/* CSS */
.button {
}

.button--primary {
}
```

```js
// JS
const toggleButton = container.querySelector('[data-toggle-button]');
```
