---
description: Summary from on-site sessions
---

# Creation operators

## 🤖 How does it work?

The creational operators are designed to receive an input and output an observable. You have operators for common uses and also specific cases. 

{% hint style="info" %}
The creational operator will convert anything into a data stream.
{% endhint %}

## 🤔 How to use it?

To use the creational operators we need to import them from the main library path. Like that:

```text
import { ... } from 'rxjs';
```

These are the only operators that need to be imported in this way. 

## ✅ How to choose the correct one?

To choose the correct operator we can use the decision tree from RxJS. Check it and find the one that matches your case.

### I want to create an observable...

using custom logic... Observable

using a state machine similar to a for loop... generate

that throws an error... throwError

that just completes, without emitting values... EMPTY

that never emits anything... NEVER

from an existing source of events coming from the DOM or Node.js or similar... fromEvent

from an existing source of events that uses an API to add and remove event handlers... fromEventPattern

from a Promise or an event source... from

that iterates over the values in an array... from

that iterates over values in a numeric range... range

that iterates over predefined values given as arguments... of

that emits values on a timer regularly... interval

that emits values on a timer with an optional initial delay... timer



