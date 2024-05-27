# TWP Style Guide

This is a guide for writing consistent and aesthetically pleasing code.

## Content

1. Core Principles
2. [Javascript](/JAVASCRIPT.md)
3. [CSS & SASS](/CSS-SASS.md)
4. [Liquid & HTML](/LIQUID-HTML.md)

<br>
<br>

---

## Core principles

Keep in mind that rules are a _guiding north_, not a purpose in itself. Some times rules will be broken, and that is ok. The important thing is that, by knowing these principle, you know _why_ you have to make an exception.

One good example is when working with an old project, or with code handed over by another agency: by strictly enforcing our styles, we can cause unnecessary confusion. Our number one principle is then to **be consistent**.

<br>
<br>

### 1. Be consistent

Humans are fabulous pattern detectors. Since the beginning of our time we understood crop seasons, were guided by patterns in the night sky, and it couldn't be different when coding.

The more we stick to coding patterns, we train ourselves to read and understand the code written from our fellow team devs – as well as write faster code by removing tiny question on _how_ to write it.

```css
/* Inconsistency leads to madness */
.my-block {
}

.WildNewBlock {
}

.WHATISTHIS {
}
```

This principle applies to _everything, everywhere... all at once_. No matter if you're writing javascript, css, documentation or theme settings, keep it consistent – even if that means to break the rules.

For example, if the codebase is using a different naming convention, try to keep it:

```css
.TheirClasses__WereNamed--LikeThis {
}

/* This becomes right */
.SoWeWrite__NewOnes--LikeThat {
}

/* This becomes wrong */
.even-if__we-write-it--like-this {
}
```

Take that with a grain of salt, however. If the codebase inherited does everything in jQuery or abbreviates all their variable names, _you probably should not keep following that pattern_. More on that in the following sections.

```js
// This is what we got...
const lgd = (a) => a.filter(u => u.l);

// ...but we will be better than that
const filterLoggedInUsers = (userArray) => userArray.filter(user => user.loggedIn);
```

Another example of consistency

```js
// Just being silly
let myList = [], my_Obj = {}
   var me_numeroUno      = 1;


// Better now... and considering these variables should not change
const myList = [];
const myObject = {};
const myNumber = 1;
```

<br>
<br>

### 2. Be obvious

Some times we get addicted to achieving the smallest code possible – and we often tap ourselves in the back when we get to really short code or even one-liners. However, think that once the code is passed to a fellow developer hands, they don't share your brain and thoughts, and may likely take a long time to decrypt your code. Leave minification and code mangling to bundlers!

Generally speaking, the more _obvious_ you make it for someone else, the easier it will be to work with it. That said, it might be possible to be overly verbose, and there is certainly a fine line between too little and too much.

#### When is too little?

- Be descriptive with non-trivial conditions

  ```js
  // Wrong
  if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  }

  // Right
  const isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);
  if (isValidPassword) {
  }
  ```

- When naming things, avoid abbreviation. Write as little as possible, but as long as necessary

  ```js
  // Wrong
  const dS = 10;
  const atr = 'William Shakespeare';

  // Right
  const delaySeconds = 10;
  const author = 'William Shakespeare';
  ```

- If an argument expects a specific unit, be clear

  ```js
  // Wrong
  function sleep(delay = 1) {
  }

  // Right
  function sleep(delaySeconds = 1) {
  }
  ```

#### When is too much?

- Avoid comments that tells exactly what the code is already saying

  ```js
  // If the user is logged in, do this
  if (user.isLoggedIn) {
    doThis();
  }
  ```

- If your name gets too long, maybe there is something wrong with the code

  ```js
  // Wrong
  function updateTheUserStatusThenReloadThePage() {
  }

  // Right
  function updateUserStatus() {
  }

  function reloadPage() {
  }
  ```

<br>
<br>

### 3. Be a never nester

Well, it may be impossible to never nest, but there is a certain threshold to how many levels deep we can go before getting completely lost. For Never nesters, that threshold could be as low as 2-3 levels. There are some easy strategies you can apply to achieve that:

- In **javascript**, you can use guard clauses to exit early, or extract blocks into their own functions.

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

  // Better
  function isPercentage(val) {
    if (val < 0) {
      return false;
    }

    if (val > 100) {
      return false;
    }

    return true;
  }

  // Great
  function isPercentage(val) {
    var isInRange = (val >= 0 && val <= 100);
    return isInRange;
  }
  ```

- In **liquid**, you may capture blocks into liquid variables.
- In **HTML**, ask yourself if all those nested `<div>`s are really necessary.
- In **SASS**, try not to nest rules just because you can (use **BEM** and your nesting needs will drastically reduce)

  ```scss
  // Wrong
  .block {
    .element {
      .modifier {
        &:hover {
        }
      }

      .other-modifier {
      }
    }

    .other-element {
    }
  }

  // Right
  .block {
  }

  .block__element {
  }

  .block__other-element {
  }

  .block__element--modifier {
  }

  .block__element--other-modifier {
  }
  ```

No matter the language, the idea is the same: keep nesting to a minimum, making it easier to keep track of what is going on.

<br>
<br>

---

## Code formatting

- Use 2 spaces for indentation (don't use tabs)
- Aim for 120 characters per line
- Add a newline at the end of a file (UNIX-style `\n`)
- Don't leave trailing whitespace

<br>
<br>

---

## Help text, theme editor settings and other text

- Use `Sentence case` instead of `Title Case` or `UPPER CASE`
- Aim for consistent language when writing help text: The same setting in different liquid sections should _probably_ have the same text.
