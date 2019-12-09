---
description: Summary from on-site sessions
---

# interval

## 🔑 Signature

From official documentation:

```javascript
interval(period: 0 = 0, scheduler: SchedulerLike = async): Observable<number>
```

Friendly signature:

```javascript
interval(periodMiliseconds, scheduler?);
```

## 📖 Important notes

{% hint style="success" %}
This operator is designed to emit an incremental index every X period.
{% endhint %}

{% hint style="info" %}
The start index is 0
{% endhint %}

{% hint style="warning" %}
The observable will keep emitting  unless the user setup a top.
{% endhint %}

## 🤔 How to use it?

The implementation is similar than the javascript interval.

```javascript
import { interval } from 'rxjs';
const source = interval(1000);

const subscribe = source.subscribe(val => console.log(val));
```

