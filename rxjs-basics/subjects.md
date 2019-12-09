---
description: Summary from on-site sessions
---

# Subjects

Before to start is important to remember this:

> "RxJS Subjects different take on the GoF Observer Pattern Subjects"
>
> _Ben Lesh_

## 🤖 How does it work?

As we describe in previous sessions the subject is accountable to emit the values on the observable pattern. But in RxJs this is defined as:

> A Subject is like an Observable, but can multicast to many Observers. Subjects are like EventEmitters: they maintain a registry of many listeners.

In fact, in RxJS the Subject class inherits from Observable but it also implements Observer. **The subject class should not be used instead and observable.** 

Internally it works as an observable but with different usages.

{% hint style="info" %}
The Subject in RxJs is an observable and observer at the same time.
{% endhint %}

## RxJS Subjects vs Observables

On RxJS is easy to get confused about subjects. The main differences are:

| Observable | Subject |
| :--- | :--- |
| No state | Preserves state |
| Unicast | Multicast |
| Reusable | Not reusable |
| A method that setup observation | Implements Observer |

## 🤔 How to use it?

The code to use a Subject is pretty similar to the observable pattern flow. 

```javascript
import { Subject } from 'rxjs';

const subject = new Subject();

subject.next("Normal")
subject.subscribe(value=>console.log(`${value}-Rick`))
subject.next("Pickle")
subject.next("Tiny")
```

{% embed url="https://stackblitz.com/edit/rxjs-course-subjects-1?ctl=1&embed=1&file=index.js&hideExplorer=1&hideNavigation=1" caption="RxJs Subject example" %}

but **what happened with "Normal-Rick"?**

### 🍕 **Subjects flavors**

The Subjects in RxJs are powerful and we have different a kind of subject for all the cases.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Class name</th>
      <th style="text-align:left">Features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Subject</td>
      <td style="text-align:left">No replay or initial value</td>
    </tr>
    <tr>
      <td style="text-align:left">AsyncSubject</td>
      <td style="text-align:left">Emits the latest value upon completion</td>
    </tr>
    <tr>
      <td style="text-align:left">BehaviorSubject</td>
      <td style="text-align:left">
        <p>Requires an initial value</p>
        <p>Replay the latest value on the stream to the new subscriptions</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ReplaySubject</td>
      <td style="text-align:left">
        <p>Requires to specify how many items should reply</p>
        <p>Replay the latest N values from the stream</p>
      </td>
    </tr>
  </tbody>
</table>### Subject

The subject doesn't have an initial value or replay behavior. But what does it mean?

{% tabs %}
{% tab title="Question" %}
#### What will be the output?

```javascript
import { Subject } from 'rxjs';

const subject = new Subject();

console.log("Delicious Cusco dishes:")

subject.next("'Queso helado'")

subject.subscribe(console.log)

subject.next("'Chiriuchu'")

subject.subscribe(value => console.log(`I'm late: ${value}`))

subject.next("'Kapchi'")

subject.subscribe(value => console.log(`I'm too late: ${value}`))

```
{% endtab %}

{% tab title="Answer" %}
```text
Delicious Cusco dishes:
'Chiriuchu'
'Kapchi'
I'm late: 'Kapchi'
```
{% endtab %}

{% tab title="Why?" %}
#### No initial value

The new subscriptions won't execute the success callback with a default value.

#### No replay behavior

The new subscriptions won't execute the success callback with the previous values.

{% hint style="info" %}
So... the new subscriptions will only receive the values on the stream that are sent after their subscription.
{% endhint %}

So, the first item does not appear because the subject hasn't any active subscription when the subject emits that value.
{% endtab %}
{% endtabs %}

### AsyncSubject

The AsyncSubject doesn't have an initial value or replay behavior as the Subject class. But this will only emit the latest value upon completion. But what means "_upon completion"?_

{% tabs %}
{% tab title="Question" %}
#### What will be the output?

```javascript
import { AsyncSubject } from 'rxjs';

const subject = new AsyncSubject();

subject.next("'Queso helado'")

subject.next("'Chiriuchu'")

subject.subscribe(value => console.log(`I want to order: ${value}`))

subject.next("'Kapchi'")

subject.next("'Tarwi'")

subject.complete()

subject.next("'Rocoto relleno'")

subject.subscribe(value => console.log(`On my next trip, I'll find: ${value}`))
```
{% endtab %}

{% tab title="Answer" %}
```text
I want to order: 'Tarwi'
On my next trip, I'll find: 'Tarwi'
```
{% endtab %}

{% tab title="Why?" %}
**It emits only the latest values before .complete\(\) the subject**

that's is why the callback only print _'Tarwi'_ and ignore _'Rocoto relleno'_
{% endtab %}
{% endtabs %}

### BehaviorSubject

It requires an initial value and it has a _reply latest value_ behavior.

{% tabs %}
{% tab title="Question" %}
**What is the initial value for both cases?**

```javascript
import { BehaviorSubject } from 'rxjs';

console.group("Sharknado case");

const caseOneSubject = new BehaviorSubject();
const firstSubscription = caseOneSubject.subscribe(value=> console.log(`We use as initial value: ${value}`));
firstSubscription.unsubscribe();

caseOneSubject.next("Vis a Vis");
caseOneSubject.next("Sharknado");
caseOneSubject.subscribe(value => console.log(`A cult movie is: ${value}`));

for(var i=2; i < 7; i++){
  caseOneSubject.next(`Sharknado ${i}`);
}

caseOneSubject.subscribe(value => console.log(`Do you like ${value}?`));
caseOneSubject.next(`Sharknado ${7}`);
console.groupEnd()
```

```javascript

import { BehaviorSubject } from 'rxjs';

console.group("Youtubers case");
const caseTwoSubject = new BehaviorSubject("Te lo resumo así no más");
caseTwoSubject.subscribe(value => console.log(`This is an awesome youtube channel: ${value}`))
caseTwoSubject.next("'PostScript'")
console.groupEnd()
```
{% endtab %}

{% tab title="Answer" %}
```text
We use as initial value: undefined
This is an awesome youtube channel: Te lo resumo así nomás
```
{% endtab %}

{% tab title="Why?" %}
**Sharknado case**

We didn't pass any default value on the _BehaviorSubject_ constructor. So, the initial value was _undefined_. It will automatically emit the initial \(or latest value on the stream\) to the new subscriptions.

**Youtubers case**

We set _Te lo resumo así no más_ as the initial value. The next subscriptions will get this value until a new value be emitted.
{% endtab %}
{% endtabs %}

### ReplaySubject

It requires a repetition value and it has a _reply latest value_ behavior.

{% tabs %}
{% tab title="Question" %}
#### What will be the output?

```javascript
import { ReplaySubject } from 'rxjs';

const subject = new ReplaySubject(3);

subject.next("The Sims")
subject.next("The legend of Zelda")
subject.next("Sayonara wild hearts")
subject.next("Number 8")

const subscriber2 = subject.subscribe(game=> console.log(`Awesome game: ${game}`))

subject.next("Monument Valley")

const subscriber3 = subject.subscribe(game=> console.log(`I wan't this games: ${game}`))

```
{% endtab %}

{% tab title="Answer" %}
```text
Awesome game: The legend of Zelda
Awesome game: Sayonara wild hearts
Awesome game: Number 8
Awesome game: Monument Valley
I wan't this games: Sayonara wild hearts
I wan't this games: Number 8
I wan't this games: Monument Valley
```
{% endtab %}

{% tab title="Why" %}
The repetition number is 3. So, it will only replay the last three items in the stream when a new subscription comes. After that, it will behave similar than the Subject.
{% endtab %}
{% endtabs %}

## 📒 Additional Resources

* [Official documentation](https://rxjs-dev.firebaseapp.com/guide/subject)
* [Learn rxjs](https://www.learnrxjs.io/subjects/)
* [Understanding Subject](https://medium.com/@luukgruijs/understanding-rxjs-subjects-339428a1815b)
* [Rxjs crash course](https://coursetro.com/posts/code/149/RxJS-Subjects-Tutorial---Subjects,-BehaviorSubject,-ReplaySubject-&-AsyncSubject)
* [Ultimate courses \(paid\)](https://ultimatecourses.com/learn/rxjs-basics)
* [Mastering subject](https://www.youtube.com/watch?v=_q-HL9YX_pk)

