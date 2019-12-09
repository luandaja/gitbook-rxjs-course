---
description: Summary from in-site class
---

# What is ReactiveX?

## The ultimate description

From the official site:

> **ReactiveX is a combination of the best ideas from Observer pattern, the Iterator pattern, and functional programming**

## **Main concepts**

1. This is an API for asynchronous programming with observable streams.
2. It uses the iterator pattern to create streams and the observable pattern to _react_ to that streams.
3. We need to start thinking **reactively**.
4. The API let us manipulate data as nedded. 
5. It has an implementation for multiple languages. On javascript it is called **RxJs**

## How does it implements the observable and iterator pattern?

ReactiveX has a strong definition about how to convert data into streams and how to consume them. \(Code from official site\)

```text
// defines, but does not invoke, the Subscriber's onNext handler
// (in this example, the observer is very simple and has only an onNext handler)
def myOnNext = { it -> do something useful with it };
// defines, but does not invoke, the Observable
def myObservable = someObservable(itsParameters);
// subscribes the Subscriber to the Observable, and invokes the Observable
myObservable.subscribe(myOnNext);
// go on about my business

def myOnNext     = { item -> /* do something useful with item */ };
def myError      = { throwable -> /* react sensibly to a failed call */ };
def myComplete   = { /* clean up after the final response */ };
def myObservable = someMethod(itsParameters);
myObservable.subscribe(myOnNext, myError, myComplete);
// go on about my business
```

ReactiveX provides _contracts_ to use the Subject and Observer. To handle the streams subscription they provide the Observable interface. So, we can create data streams from any data source by observables \(it will handle the subject\).   
  
In this case, the subscription method has three callbacks: onSuccess, onError, onComplete. We will discuss how it works later.

## Additional resources

1. [ReactiveX official page](http://reactivex.io/)

