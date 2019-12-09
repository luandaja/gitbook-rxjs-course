---
description: Summary from on-site sessions
---

# of

## 🔑 Signature

From official documentation:

```javascript
of<T>(...args: Array<T | SchedulerLike>): Observable<T>
```

Friendly signature:

```javascript
of(...itemsToStream, scheduler?)
```

## 📖 Important notes

{% hint style="success" %}
It's used to emit values from static data..
{% endhint %}

{% hint style="info" %}
It emits directly the method arguments.
{% endhint %}

{% hint style="warning" %}
This operator does not perform any type of flattering.
{% endhint %}

{% hint style="info" %}
It automatically complete the subscription when all the items were emitted 
{% endhint %}

## 🤔 How to use it?

```javascript
import { of } from 'rxjs';

const observer = {
    next: val => console.log('next', val),
    error: err => console.log('error', err),
    complete: () => console.log('complete!')
};

const source$ = of(1,2,3,4,5);

source$.subscribe(observer);
```

![](../../.gitbook/assets/foo_example.svg)

## 🕵 Common questions

### When using arrays

{% tabs %}
{% tab title="Question" %}
#### What will be the output?

```javascript
import { of } from 'rxjs';

const observer = {
    next: val => console.log('next', val),
    error: err => console.log('error', err),
    complete: () => console.log('complete!')
};

const source$ = of([1,2,3,4,5]);

source$.subscribe(observer);
```
{% endtab %}

{% tab title="Answer" %}
```text
next ▶[1, 2, 3, 4, 5]
complete!
```
{% endtab %}

{% tab title="Why" %}
The _of_ operator does not have flattering. So, it would emit the array as an array. The same thing happens with the strings.

![](../../.gitbook/assets/foo_example.png)
{% endtab %}
{% endtabs %}

### When using multiple input types

{% tabs %}
{% tab title="Question" %}
#### What will be the output?

```javascript
import { of } from 'rxjs';

const observer = {
    next: val => console.log('next', val),
    error: err => console.log('error', err),
    complete: () => console.log('complete!')
};

const source$ = of("Perú", [28, 7, 1821], {independent: true}, function(){return "In Lima"});

source$.subscribe(observer);
```
{% endtab %}

{% tab title="Answer" %}
```text
next Perú
next ▶[28, 7, 1821]
next ▶{independent: true}
next ▶ƒ ()
complete!
```
{% endtab %}

{% tab title="Why?" %}
The _of_ operator will emit the same static data that we use as param. No matters the type.
{% endtab %}
{% endtabs %}



