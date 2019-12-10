---
description: Summary from on-site sessions
---

# Filtering operators

## 🤖 How does it work?

The filtering operators are designed to filter the emitted values. 

{% hint style="info" %}
The data stream will be evaluated by the operators. It will filter them by a predicate or condition.
{% endhint %}

## 🤔 How to use it?

The filtering operators should be imported from

```text
import { ... } from 'rxjs/operators';
```


## ✅ How to choose the correct one?

Follow the operators description to choose the correct one:

### audit

#### 🔑 Signature

From official documentation:

```javascript
audit<T>(durationSelector: (value: T) => SubscribableOrPromise<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
audit(durationSelector)
//durationSelector is a method that returns a observable or promise
audit(value => { return anObservableBasedOnTimeEmisions })
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to prevent the emition for X time. Then emit the latest value.
{% endhint %}

{% hint style="info" %}
This operator will filter emission to only emit the latest on when an observable completes.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than auditTime.

```javascript
import { fromEvent, interval} from 'rxjs';
import { audit } from 'rxjs/operators'

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(audit(ev => interval(1000)));
result.subscribe(value => console.log(`(${value.x},${value.y})`));
```


### auditTime

#### 🔑 Signature

From official documentation:

```javascript
auditTime<T>(duration: number, scheduler: SchedulerLike = async): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
auditTime(duration, scheduler?)
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to prevent the emition for X time. Then emit the latest value.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than the interval operator

```javascript
import { fromEvent, interval} from 'rxjs';
import { audit ,auditTime} from 'rxjs/operators'

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(auditTime(1000));
result.subscribe(value => console.log(`(${value.x},${value.y})`));
```


### debounce

#### 🔑 Signature

From official documentation:

```javascript
debounce<T>(durationSelector: (value: T) => SubscribableOrPromise<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
debounce(durationSelector);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to emit and value after X time.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than the audit operator.

```javascript
import { fromEvent, interval} from 'rxjs';
import { debounce} from 'rxjs/operators'

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(debounce(val => interval(1000)));
result.subscribe(value => console.log(`(${value.x},${value.y})`));
```


### debounceTime ⭐️

#### 🔑 Signature

From official documentation:

```javascript
debounceTime<T>(dueTime: number, scheduler: SchedulerLike = async): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
debounceTime(periodMiliseconds, scheduler?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to emit the stream value after X time.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than the debounce operator.

```javascript
import { fromEvent, interval} from 'rxjs';
import { debounceTime } from 'rxjs/operators'

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(debounceTime(1000));
result.subscribe(value => console.log(`(${value.x},${value.y})`));
```


### distinct

#### 🔑 Signature

From official documentation:

```javascript
distinct<T, K>(keySelector?: (value: T) => K, flushes?: Observable<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
distinct(keySelector?, flushesObservable?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to prevent the emission of duplicated values.
{% endhint %}

{% hint style="info" %}
The operator prevent the value emission if it is already emitted on any moment.
{% endhint %}

{% hint style="info" %}
For objects you can use the the keySelector param.
{% endhint %}

{% hint style="warning" %}
It evaluates the value of the property. It does not implement flattering to interpretate arrays or complex types
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { of } from 'rxjs';
import { distinct } from 'rxjs/operators';

of(1, 1, 2, 2, 2, 1, 2, 3, 4, 3, 2, 1).pipe(
    distinct(),
  )
  .subscribe(x => console.log(x));

  // 1, 2, 3, 4
```


### distinctUntilChanged ⭐️

#### 🔑 Signature

From official documentation:

```javascript
distinctUntilChanged<T, K>(compare?: (x: K, y: K) => boolean, keySelector?: (x: T) => K): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
distinctUntilChanged(comparerMethod?, keySelector?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to prevent the emission of sequently duplicated values.
{% endhint %}

{% hint style="info" %}
The comparer method could be used to specify a custom logic to compare the emitted values.
{% endhint %}

{% hint style="warning" %}
The observable will keep emitting  unless the user setup a top.
{% endhint %}

{% hint style="info" %}
The key selector is used to manipulate the evaluated item (actual and previous). Commonly not used.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than distinct.

```javascript
import { of } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

 console.clear();

interface Artist {
   likes: number,
   name: string
}
 
of<Artist>(
  {likes: 1223, name: '2 minutos'},
  {likes: 55234, name: 'Fito Paez'},
  {likes: 4565354, name: '2 minutos'},
  {likes: 65653, name: '2 minutos'},
  {likes: 99874, name: 'Fito Paez'},
  ).pipe(
    distinctUntilChanged((p: Artist, q: Artist) => p.name === q.name),
  )
  .subscribe(x => console.log(x));
```

using the key selector. In this case this :

```javascript
import { of } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

 console.clear();

interface Artist {
   likes: number,
   name: string
}
 
of<Artist>(
  {likes: 1223, name: '2 minutos'},
  {likes: 55234, name: 'Fito Paez'},
  {likes: 4565354, name: '2 minutos'},
  {likes: 65653, name: '2 minutos'},
  {likes: 99874, name: 'Fito Paez'},
  ).pipe(
    distinctUntilChanged(
      (actual: Artist, previous: Artist) => {
        console.log(actual.name, previous.name)
        return actual.name === previous.name;
        },
      (item: Artist) => ({likes: item.likes, name: "I'm the same for all" })
    ),
  )
  .subscribe(x => console.log(x));
```



### distinctUntilKeyChanged

#### 🔑 Signature

From official documentation:

```javascript
distinctUntilKeyChanged<T, K extends keyof T>(key: K, compare?: (x: T[K], y: T[K]) => boolean): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
distinctUntilKeyChanged(key, comparer?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to evaluate object emissions and prevenet sequently duplicated emissions.
{% endhint %}

{% hint style="info" %}
The key parameter define the object property to evaluate.
{% endhint %}

{% hint style="info" %}
The comparer property is used to provide a custom evaluation method.
{% endhint %}

#### 🤔 How to use it?

The implementation is similar than distinct... but for objects.

```javascript
import { of } from 'rxjs';
import { distinctUntilKeyChanged } from 'rxjs/operators';

 console.clear();

interface Artist {
   likes: number,
   name: string
}
 
of<Artist>(
  {likes: 1223, name: '2 minutos'},
  {likes: 55234, name: 'Fito Paez'},
  {likes: 4565354, name: '2 minutos'},
  {likes: 65653, name: '2 minutos'},
  {likes: 99874, name: 'Fito Paez'},
  ).pipe(
    distinctUntilChanged("name"),
  )
  .subscribe(x => console.log(x));
```



### filter

#### 🔑 Signature

From official documentation:

```javascript
filter<T>(predicate: (value: T, index: number) => boolean, thisArg?: any): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
filter(predicate, thisArgument?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed filter the emitted values.
{% endhint %}

#### 🤔 How to use it?

The implementation looks similar than the first.

```javascript
import { from } from 'rxjs';
import { filter } from 'rxjs/operators';

const source = from([1, 2, 3, 4, 5]);
const example = source.pipe(filter(num => num % 2 === 0));
const subscribe = example.subscribe(val => console.log(`Even number: ${val}`));
```


### first

#### 🔑 Signature

From official documentation:

```javascript
first<T, D>(predicate?: ((value: T, index: number, source: Observable<T>) => boolean) | null, defaultValue?: D): OperatorFunction<T, T | D>
```

Friendly signature:

```javascript
first(predicate?, defaultValue?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed emit the first value that match the predicate.
{% endhint %}

{% hint style="warning" %}
If the observable completes before emit a value it will throw an error.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { from } from 'rxjs';
import { first } from 'rxjs/operators';

const source = from([1, 2, 3, 4, 5]);
const example = source.pipe(first(num => num % 2 === 0));
const subscribe = example.subscribe(val => console.log(`Even number: ${val}`));
```


### ignoreElements

#### 🔑 Signature

From official documentation:

```javascript
ignoreElements(): OperatorFunction<any, never>
```

Friendly signature:

```javascript
ignoreElements();
```

#### 📖 Important notes

{% hint style="success" %}
Ignore the emmitted stream items.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { of } from 'rxjs';
import { ignoreElements } from 'rxjs/operators';
const source = of("J.B","B.B","D.Y","B.B",).pipe(ignoreElements());
const observer = {
    next: val => console.log('next', val),
    error: err => console.log('error', err),
    complete: () => console.log('complete!')
};
const subscribe = source.subscribe(val => console.log(val));
```


### last

#### 🔑 Signature

From official documentation:

```javascript
last<T, D>(predicate?: ((value: T, index: number, source: Observable<T>) => boolean) | null, defaultValue?: D): OperatorFunction<T, T | D>
```

Friendly signature:

```javascript
last(predicate?, defaultValue?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to emit the last item on the stream.
{% endhint %}

{% hint style="info" %}
If you provides a predicate, it will emit the last item on the stream that match the condition.
{% endhint %}

{% hint style="warning" %}
If the predicate is not satisfied it will throw an error. Unless, the user provides a defaultValue. 
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { from } from 'rxjs';
import { last} from 'rxjs/operators';

const source = from([1, 2, 3, 4, 5]);
const example = source.pipe(last(num => num === 4 ));
const subscribe = example.subscribe(val =>
  console.log(`Last: ${val}`)
);
```


### sample

#### 🔑 Signature

From official documentation:

```javascript
sample<T>(notifier: Observable<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
sample(notifier);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed emit the last emitted value when the emiter dispatch a value.
{% endhint %}

{% hint style="info" %}
The emitter should be another observable.
{% endhint %}


#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { fromEvent, interval } from 'rxjs';
import { sample } from 'rxjs/operators';

const seconds = interval(1000);
const clicks = fromEvent(document, 'click');
const result = seconds.pipe(sample(clicks));
result.subscribe(x => console.log(x));
```


### single

#### 🔑 Signature

From official documentation:

```javascript
single<T>(predicate?: (value: T, index: number, source: Observable<T>) => boolean): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
single(predicate);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to emit value if it is unique (or the only one that match the predicate)
{% endhint %}

{% hint style="info" %}
If it's invoked without predicate, it will expect an observable with only one item.
{% endhint %}

{% hint style="warning" %}
If the operator is not satisfied (not unique items) it will throw an error.
{% endhint %}

{% hint style="warning" %}
If the operator is not satisfied (any value match the predicate) it will emit undefined.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { range, of } from 'rxjs';
import { single } from 'rxjs/operators';

const numbers = of(1,1).pipe(single(x=> x===1));
numbers.subscribe(x => console.log('next',x), e => console.log('error'));
// result
// 'error'
```
but if the result is unique:

```javascript
import { range, of } from 'rxjs';
import { single } from 'rxjs/operators';

const numbers = of(1,2,3).pipe(single(x=> x===1));
numbers.subscribe(x => console.log('next',x), e => console.log('error'));
// result
// next 1
```

```javascript
import { range, of } from 'rxjs';
import { single } from 'rxjs/operators';

const numbers = of(1).pipe(single());
numbers.subscribe(x => console.log('next',x), e => console.log('error'));
// result
// next 1
```

if any item match the predicate

```javascript
import { range, of } from 'rxjs';
import { single } from 'rxjs/operators';

const numbers = of(1,2,3,4,5).pipe(single(x=>x === 100));
numbers.subscribe(x => console.log('next',x), e => console.log('error'));
// result
// next undefined
```


### skip

#### 🔑 Signature

From official documentation:

```javascript
skip<T>(count: number): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
skip(countToSkip);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to skip the first X items on the stream.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { interval } from 'rxjs';
import { skip } from 'rxjs/operators';

const source = interval(1000);
const result = source.pipe(skip(3));
result.subscribe(x => console.log(x));
```


### skipUntil

#### 🔑 Signature

From official documentation:

```javascript
skipUntil<T>(notifier: Observable<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
skipUntil(notifier);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to emit their values after the notifier observable emit a value.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { interval , timer} from 'rxjs';
import { skipUntil } from 'rxjs/operators';

const source = interval(1000);
const notifier = timer(5000)

const result = source.pipe(skipUntil(notifier));
result.subscribe(x => console.log(x));
```


### skipWhile

#### 🔑 Signature

From official documentation:

```javascript
skipWhile<T>(predicate: (value: T, index: number) => boolean): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
interval(predicate?);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to skip the stream items until the predicate will be satisfied.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { interval} from 'rxjs';
import { skipWhile } from 'rxjs/operators';

const source = interval(1000);

const result = source.pipe(skipWhile(x=>x < 5));
result.subscribe(x => console.log(x));
```


### take ⭐️

#### 🔑 Signature

From official documentation:

```javascript
take<T>(count: number): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
take(count);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to only take X amout of items from the stream. Then, it completes it.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { interval} from 'rxjs';
import { take} from 'rxjs/operators';

const observer = {
    next: val => console.log('next', val),
    error: err => console.log('error', err),
    complete: () => console.log('complete!')
};
const source = interval(1000);

const result = source.pipe(take(5) );
result.subscribe(observer);
```


### takeUntil ⭐️

#### 🔑 Signature

From official documentation:

```javascript
takeUntil<T>(notifier: Observable<any>): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
takeUntil(notifier);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to take the stream emissions until a notifier observable emits a value.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { fromEvent, interval } from 'rxjs';
import { takeUntil } from 'rxjs/operators';

const source = interval(1000);
const clicks = fromEvent(document, 'click');
const result = source.pipe(takeUntil(clicks));
result.subscribe(x => console.log(x));
```


### takeWhile

#### 🔑 Signature

From official documentation:

```javascript
takeWhile<T>(predicate: (value: T, index: number) => boolean, inclusive: false = false): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
takeWhile(predicate, inclusive = false);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to take items from the stream as long as the value satisfies the predicate.
{% endhint %}

{% hint style="info" %}
If _inclusive_ param is true, the item that caused the predicate returns false  will be emitted as last item.
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { fromEvent, interval } from 'rxjs';
import { takeWhile } from 'rxjs/operators';

const source = interval(1000);
const result = source.pipe(takeWhile(x=> x < 10 ));
result.subscribe(x => console.log(x));
```


### throttle

#### 🔑 Signature

From official documentation:

```javascript
throttle<T>(durationSelector: (value: T) => SubscribableOrPromise<any>, config: ThrottleConfig = defaultThrottleConfig): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
throttle(durationSelector:config = defaultConfig)
```

#### 📖 Important notes

{% hint style="success" %}
The operator will ignore the subsequent emissions from the stream by another observable duration.
{% endhint %}

{% hint style="info" %}
The default config is _{ leading: true, trailing: false }_
{% endhint %}

{% hint style="info" %}
the emissions are grouped by segments of time.
leading: emit the first item of the segment
trailing: emit the last item of the segment
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
import { fromEvent ,interval } from 'rxjs';
import { throttle  } from 'rxjs/operators';

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(throttle(ev => interval(3000), { leading: true, trailing: false }));
result.subscribe(x => console.log(x));
```


### throttleTime

#### 🔑 Signature

From official documentation:

```javascript
throttleTime<T>(duration: number, scheduler: SchedulerLike = async, config: ThrottleConfig = defaultThrottleConfig): MonoTypeOperatorFunction<T>
```

Friendly signature:

```javascript
throttleTime(duration, scheduler = async, config = defaultConfig);
```

#### 📖 Important notes

{% hint style="success" %}
This operator is designed to group the emissions by a segment of time (defined by another observable) and skip the middle emissions.
{% endhint %}

{% hint style="info" %}
The configuration change the behavior og the fitered emissions
{% endhint %}

{% hint style="info" %}
the emissions are grouped by segments of time.
leading: emit the first item of the segment
trailing: emit the last item of the segment
{% endhint %}

#### 🤔 How to use it?

The next code is an example of this operator.

```javascript
  import { fromEvent ,interval } from 'rxjs';
  import { throttleTime  } from 'rxjs/operators';

  const clicks = fromEvent(document, 'click');
  const result = clicks.pipe(throttleTime(3000));
  result.subscribe(x => console.log(x));
```





