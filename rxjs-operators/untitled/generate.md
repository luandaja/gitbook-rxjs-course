---
description: Summary from on-site sessions
---

# generate

## 🔑 Signature

From official documentation:

```javascript
generate<T, S>(initialStateOrOptions: S | GenerateOptions<T, S>, condition?: ConditionFunc<S>, iterate?: IterateFunc<S>, resultSelectorOrObservable?: (ResultFunc<S, T>) | SchedulerLike, scheduler?: SchedulerLike): Observable<T>
```

Friendly signature:

```javascript
generate(initialState, condition, iterator, resultSelector?, scheduler?  )
//how to make it cristal clear?

for(let i = 0; i < 10 ; i++) { }
for(initialState; condition; iterator)

//will be the same as:
generate(0, i => i < 10, i => i + 1)
```

## 📖 Important notes

{% hint style="success" %}
This operator is designed to generate a sequence of items based on a custom iterator.
{% endhint %}

{% hint style="info" %}
The resultSelector is used to manipulate the emitted value.
{% endhint %}

## 🤔 How to use it?

The generate is very similar a for loop, but with more flexibility.

```javascript
import { generate } from 'rxjs';
const source = generate(0, i => i < 10, i => i + 1);
const subscribe = source.subscribe(val => console.log(val));

//0,1,2,3,4,5,6,7,8,9
```

If we want to manipulate the output we can use the selector param: 

```javascript
import { generate } from 'rxjs';
const source = generate(0, i => i < 10, i => i + 1, x => x * 10);
const subscribe = source.subscribe(val => console.log(val));

//0,10,20,30,40,50,60,70,80,90
```

