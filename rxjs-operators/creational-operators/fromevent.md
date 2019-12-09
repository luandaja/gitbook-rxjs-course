---
description: Summary from on-site sessions
---

# fromEvent

## 🔑 Signature

From official documentation:

```javascript
fromEvent<T>(target: FromEventTarget<T>, eventName: string, options?: EventListenerOptions | ((...args: any[]) => T), resultSelector?: ((...args: any[]) => T)): Observable<T>
```

Friendly signature:

```javascript
fromEvent(taget, eventName, options?, selector?)
```

## 📖 Important notes

{% hint style="success" %}
This operator is designed to "_listen_" events from DOM.
{% endhint %}

{% hint style="info" %}
Internally for each subscription it creates an event listener.
{% endhint %}

{% hint style="warning" %}
It needs to call unsubscribe to dispose the event listener.
{% endhint %}

## 🤔 How to use it?

The implementation for the _fromEvent_ is very similar than the other creational operators.

```javascript
import { fromEvent } from 'rxjs';

const observer = {
  next: val => console.log('next', val),
  error: err => console.log('error', err),
  complete: () => console.log('Complete!')
};

const source$ = fromEvent(document, 'keyup');
const subcription = source$.subscribe(observer);

setTimeout(() => {
  subcription.unsubscribe();
  console.log("unsubscribed")
}, 3000);


```

What about the options? what kind of options? Well, they are the addEventListenerOptions. This is outside the scope of the course, but you can review it [here](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener). Here is an example of it. 

```javascript
import { fromEvent } from 'rxjs';

const observer = {
  next: val => console.log('next', val),
  error: err => console.log('error', err),
  complete: () => console.log('Complete!')
};

const source$ = fromEvent(document, 'keyup', { once: true });
const subcription = source$.subscribe(observer);

setTimeout(() => {
  subcription.unsubscribe();
  console.log("unsubscribed")
}, 3000);

```

On the second example, the event listener has the _once_ option. That means the event listener will only one time. 

## 🕵 Common questions

### When using a custom target element

{% tabs %}
{% tab title="Question" %}
#### Does it work? What is going to be the behavior?

```javascript
import { fromEvent } from 'rxjs';

const target = document.getElementById("customElement")

const source$ = fromEvent(document, 'click');
const subcription = source$.subscribe(console.log);

setTimeout(() => {
  subcription.unsubscribe();
  console.log("unsubscribed")
}, 3000);

```
{% endtab %}

{% tab title="Answer" %}
Yes, it will work. The observable will emit values when the custom element dispatch a click event.
{% endtab %}
{% endtabs %}

