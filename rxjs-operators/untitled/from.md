---
description: Summary from on-site sessions
---

# from

## 🔑 Signature

From official documentation:

```javascript
from<T>(input: ObservableInput<T>, scheduler?: SchedulerLike): Observable<T>
```

Friendly signature:

```javascript
from(input, scheduler?)
//input : Array | Iterator | Promise | Observable
```

## 📖 Important notes

{% hint style="success" %}
It's used to emit the values from arrays, iterators, and promises.
{% endhint %}

{% hint style="info" %}
This operator perform a flattering process.
{% endhint %}

{% hint style="info" %}
It automatically complete the subscription when all the items were resolved 
{% endhint %}

## 🤔 How to use it?

The _from_ operator could be used for arrays, iterators and promises. Here is an example for each one.

The next code will be shared for all the examples

```javascript
import { from } from 'rxjs';

const observer = {
  next: val => console.log(val),
  error: err => console.log('error', err),
  complete: () => console.log('complete!')
};

```

### From array

```javascript
from([1, 2, 3, 4, [5]]).subscribe(console.log);
//output: 1, 2, 3, 4, [5]
from('RxJs').subscribe(console.log);
//output: 'R', 'x', 'J', 's'
```

### From promise

```javascript

from(fetch('https://api.github.com/users/luandaja')).subscribe(console.log);
//output: Response {type: "cors", url: "https://api.github.com/users/luandaja", redirected: false, status: 200…}

const promise = new Promise((resolve, reject)=> {
  resolve("Capitan Fantastic is an awesome moview");
});
from(promise).subscribe(console.log);
//output: Capitan Fantastic is an awesome moview
```

### From iterator

```javascript
function* randomPeruvianArtistsGenerator() {
    yield 'Daniel F';
    yield 'NoRecomendable';
    yield 'Barrio Calavera';
};

const randomPeruvianArtistsIterator = randomPeruvianArtistsGenerator();
from(randomPeruvianArtistsIterator).subscribe(console.log);
//output: 'Daniel F', 'NoRecomendable', 'Barrio Calavera'
```

## 🕵 Common questions

### When using static data

{% tabs %}
{% tab title="Question" %}
#### Why this thows and error?

```javascript
from(42)
```

#### And why it doesn’t 

```javascript
from('luis')
```
{% endtab %}

{% tab title="Answer" %}
The _from_ operator is prepared to receive two params. A valid source and a scheduler. The valid source could be and Array, Iterator, Promise or other Observable. 

For the second case, it works because the string is an array of characters.
{% endtab %}
{% endtabs %}



