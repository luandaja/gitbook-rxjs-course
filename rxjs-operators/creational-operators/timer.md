---
description: Summary from on-site sessions
---

# timer

## 🔑 Signature

From official documentation:

```javascript
timer(dueTime: number | Date = 0, periodOrScheduler?: number | SchedulerLike, scheduler?: SchedulerLike): Observable<number>
```

Friendly signature:

```javascript
timer(dueTime, periodMillisecods?, scheduler?);
```

## 📖 Important notes

{% hint style="success" %}
This operator is designed to emit an incremental index every after due time.
{% endhint %}

{% hint style="info" %}
It also can keep emitting values as an interval if the user setup the period \(second parameter\).
{% endhint %}

{% hint style="warning" %}
If use the timer as a deferred interval, don't forget the unsubscription.
{% endhint %}

## 🤔 How to use it?

The implementation is similar than the _settimeout_ method.

```javascript
import { timer } from 'rxjs';
const source = timer(1000);

const subscribe = source.subscribe(val => console.log(val));
```

To setup this as a deferred interval: 

```javascript
import { timer } from 'rxjs';
const source = timer(5000, 1000);

const subscribe = source.subscribe(val => console.log(val));
```

{% hint style="info" %}
The first case will only emit  a 0 after 1 second. 
{% endhint %}

{% hint style="info" %}
The second case will emit 0 after 5 seconds. Then, it will emit the incremental index each second.
{% endhint %}

