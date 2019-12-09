---
description: Summary from on-site sessions
---

# range

## 🔑 Signature

From official documentation:

```javascript
range(start: number = 0, count?: number, scheduler?: SchedulerLike): Observable<number>
```

Friendly signature:

```javascript
//with start on 0
range(count, scheduler?)
//with custom start
range(start, count, scheduler?)
```

## 📖 Important notes

{% hint style="success" %}
This operator is designed to "_generate_" a stream of numbers.
{% endhint %}

{% hint style="info" %}
The observable will complete itself after emit the latest range value.
{% endhint %}

{% hint style="warning" %}
The count parameter is the number of items to generate. Not the upper top of the range.

This range\(2,4\) will emit 2,3,4,5.
{% endhint %}

## 🤔 How to use it?

The implementation is similar than the _of_ operator:

```javascript
import { range } from 'rxjs'; 

const source = range(2, 4);

source.subscribe(x => console.log(x));

//output: 2, 3, 4, 5
```

## 🕵 Common questions

### Just a friendly reminder

{% tabs %}
{% tab title="Question" %}
#### What will be the output? Does it throw an error?

```javascript
import { range } from 'rxjs'; 

const source = range(10, 6);

source.subscribe(x => console.log(x));
```
{% endtab %}

{% tab title="Answer" %}
```javascript
10
11
12
13
14
15
```
{% endtab %}

{% tab title="Why?" %}
Remember: the count is the number of items to generate.
{% endtab %}
{% endtabs %}



