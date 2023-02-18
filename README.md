# Weird Javascript

Based on [Javascript is Weird (EXTREME EDITION)](https://www.youtube.com/watch?v=sRWE5tnaxlI) from [Low Byte Productions](https://www.youtube.com/watch?v=sRWE5tnaxlI).
The premise of this program is simple, using only '[', ']', '{', '}', '(', ')', '+', '!', '=' and '>', create any javascript program. We do this by building up to every character, until eventually we have everything. Here, I'd like to outline how I did this.

## Numbers

### 0

By using `+[]`, we can get zero. Yes you read that right `+[]` evaluates to zero in Javascript. It works because if you add a `+` to anything you can cast it to a number, and casting an empty array to a number, results in 0.

```javascript
const zero = `+[]`;
```

### 1

In somewhat of the same vein, we can also get 1 by converting true to a number so: `+true`, however, true is not allowed in the limited set of characters that we have, so we have to get the true value somehow. We can, again, look at how we created 0: maybe we can cast an empty array to a boolean, and we can by using `!`. However `![]` result in false, but we can fix that by inversing it again, which will result in the total expression `+!![]`.

```javascript
const one = `+!![]`;
```

### Every other number

To get any other number, we can create a simple helper function by just adding one until we have the correct number.

```javascript
const number = n => {
    if (n === 0) return zero;
    return Array.from({length: n}, () => one).join(' + ');
};
```

## First characters

### Wishlist

The characters we want to get are (not in order): fromChadetSingcsup\[space][backslash], these characters will help us be able to get every other character using functions we can then create.
First, we'll create a map object that stores all characters we've gotten so far.

```javascript
const map = {}
```

### a

First, let's get 'a', to get a we have to get some constant in Javascript which we can convert to a string, one constant which has an a in it is NaN. To get NaN, we can use `+{}`, which creates an object and then converts that object to a number, which will result in NaN. Now that we have the constant, we have to convert it to a string by using `+[]`, because that for some reason converts in to a string. Then we can index by using our `number` function.

```javascript

```