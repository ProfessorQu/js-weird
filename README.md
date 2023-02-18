# Weird Javascript

Based on [Javascript is Weird (EXTREME EDITION)](https://www.youtube.com/watch?v=sRWE5tnaxlI) from [Low Byte Productions](https://www.youtube.com/watch?v=sRWE5tnaxlI).
The premise of this program is simple, using only '[', ']', '{', '}', '(', ')', '+', '-', '!', '=', '>', '/' and '\', create any javascript program. We do this by building up to every character, until eventually we have everything. Here, I'd like to outline how I did this.

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

### NaN

First, let's get 'a', to get a we have to get some constant in Javascript which we can convert to a String, one constant which has an a in it is NaN. To get NaN, we can use `+{}`, which creates an object and then converts that object to a number, which will result in NaN. Now that we have the constant, we have to convert it to a String by using `+[]`, because that for some reason converts in to a string. Then we can index by using our `number` function.

```javascript
map.a = `(+{}+[])[${number(1)}]`;
```

### [object Object]

We can get a bunch of other letters by also using objects, but without casting them to integers. If you create an object using `{}`, and then convert it to a String, you'll get '[object Object]', from this we can get b, o, e, c, t and \[space].

```javascript
map.b = `({}+[])[${number(2)}]`;
map.o = `({}+[])[${number(1)}]`;
map.e = `({}+[])[${number(4)}]`;
map.c = `({}+[])[${number(5)}]`;
map.t = `({}+[])[${number(6)}]`;
map[' '] = `({}+[])[${number(7)}]`;
```

### false

We can also get a few letters by converting them to Strings using `+[]`, if we do this we can get f and s.

```javascript
map.f = `(![]+[])[${number(0)}]`;
map.s = `(![]+[])[${number(3)}]`;
```

### true

Then we just not that and we can get r and u.

```javascript
map.r = `(!![]+[])[${number(1)}]`;
map.u = `(!![]+[])[${number(2)}]`;
```

### Infinity

We can get another few letters by abusing another constant, Infinity, we can get this by dividing one, or any number, by zero. Then we can get i and n.

```javascript
map.i = `((+!![]/+[])+[])[${number(3)}]`;
map.n = `((+!![]/+[])+[])[${number(4)}]`;
```

## constructor

Now that we have all the letters to spell out constructor, we can use that to get a few things from some objects. But first, we'll create a function to use all characters we've collected to convert normal Strings to our special strings. We can do this with `fromString`, which will take in a String and convert it to our special string.

```javascript
const fromString = s => s.split('').map(x => {
    return map[x];
}).join('+');
```


### String

First, let's use the String constructor, to do that we have to first create an empty String, which we can do of course with `[]+[]`, which will create an empty string. Then we want to get a constructor out of that by indexing, you can index by String instead of accessing a property of an object `([]+[])[${fromString('constructor')}]`. Then we have to convert that to a string by adding `+[]` and then index that String to get S and g.

```javascript
map.S = `([]+([]+[])[${fromString('constructor')}])[${number(9)}]`;
map.g = `([]+([]+[])[${fromString('constructor')}])[${number(14)}]`;
```

### RegExp

This is basically the same as with the String, only now where using a regular expression. Using the constructor we can get p, we can also get backslash by using a different regular expression.

```javascript
map.p = `([]+(/-/)[${fromString('constructor')}])[${number(14)}]`;
map['\\'] = `(/\\\\/+[])[${number(1)}]`
```

### Different number systems

To get the second to last few letters, we can use differnet number systems. With the `toString` method called on a number, we can pass in a number as an argument to determine what base we want. So we can just use our `number` and `fromString` methods to create it.

```javascript
map.d = `(${number(13)})[${fromString('toString')}](${number(14)})`;
map.h = `(${number(17)})[${fromString('toString')}](${number(18)})`;
map.m = `(${number(22)})[${fromString('toString')}](${number(23)})`;
```

### C

So to get the last character needed to get every single other character, we have to think outside the box a bit, because this one is going to be a bit harder. We can start by creating a function using `()=>{}` and then getting a constructor from that, once we have a constructor we can create the body of the function using `(${fromString(return escape)})`, when we call that function, we get the escape function which allows us to get the escaped code from some characters. The one we are interested in is the escape code of '\', which will give us %5C. Then we index it to get C.

```javascript
map.C = `((()=>{})[${fromString('constructor')}](${fromString('return escape')})()(${map['\\']}))[${number(2)}]`;
```

## Every other character

Now we can finally get every other character by using the String constructor again. First we create a String `[]+[]` then we get it's constructor, then we get the function `fromCharCode` and we pass in the charcode of the character we want (by passing it through number first of couse).

```javascript
const fromString = s => s.split('').map(x => {
    if (x in map) return map[x];

    const charCode = x.charCodeAt(0);
    return `([]+[])[${fromString('constructor')}][${fromString('fromCharCode')}](${number(charCode)})`;
}).join('+');
```

## Compile

And now we finally have everything to be able to compile anything by using a function constructor and then running any arbitrary code in it.

```javascript
const compile = code => `(()=>{})[${fromString('constructor')}](${fromString(code)})()`;
```


