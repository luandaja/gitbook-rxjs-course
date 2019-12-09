---
description: Summary from on-site sessions
---

# Changing the paradigm - Part 1

Before start with ReactiveX we need to be aware about the mindset change that we need.

## 🧠 Pull vs Push architectures

### Pull architecture

![Common &quot;pull&quot; architecture](../.gitbook/assets/observable-pattern-1.png)

The pull architecture refers to when the client "push" a request to the server. This is the most famous example: Client makes a data request to the server and the server response with the data. The main characteristic of this approach is **the client manually make the request.**

### Push architecture

Contrary to pull architectures, the push approach requires the client to wait for the notification from a source.

![Simple &quot;push&quot; architecture schema](../.gitbook/assets/observable-pattern-2.png)

### Cheatsheet

|  | Source | Client |
| :--- | :--- | :--- |
| Pull | Send data when requested | Decide when the data is requested |
| Push | Send the data by it's own pace | Reacts to the received data |

## 📒 Additional Resources

* [Jeff Poole medium article](https://medium.com/@_JeffPoole/thoughts-on-push-vs-pull-architectures-666f1eab20c2)
* [RxJs course by Brian Troncome \(paid\)](https://ultimatecourses.com/learn/rxjs-basics)
* [Thomas Burleson medium article](https://medium.com/@thomasburlesonIA/push-based-architectures-with-rxjs-81b327d7c32d)
* [Pull vs Push](https://rxjs-dev.firebaseapp.com/guide/observable)



