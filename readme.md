# Reactive Programming RxJs

This file contains [RxJS] + [Angular] Concepts fro scratch

I am dammn sure you have used RxJs in your projects in order to build the reactive and async functionality. Purpose of this page is to get to know the different use case where we can implement the reactive programming. Nothing better then a purpose, Right. Learn with purpose make work fun, Isn't ? :)
## Table of Contents
* [Old Days](#old-days)
* [Observables RxJS Questions](#observables-rxjs-questions)
* [Performance Questions](#performance-questions)
* [TypeScript Questions](#typescript-questions)
* [JavaScript Questions](#javascript-questions)
* [Coding Questions](#coding-questions)
* [Fun Questions](#fun-questions)
## Observable
An observable represents a stream, or source of data that can arrive over time.

## Subscription
Subscriptions are what set everything in motion. You can think of this like a faucet, you have a stream of water ready to be tapped (observable), someone just needs to turn the handle.
```ts
// import the fromEvent operator
import { fromEvent } from 'rxjs';
// grab button reference
const button = document.getElementById('myButton');
// create an observable of button clicks
const myObservable = fromEvent(button, 'click');
// for now, let's just log the event on each click
const subscription = myObservable.subscribe(event => console.log(event));
```
## [Operators](https://www.learnrxjs.io/learn-rxjs/operators)
Operators offer a way to manipulate values from a source, returning an observable of the transformed values
- crateation operators: of, from, and fromEvent.
- combination operators: combineLatest, concat, merge, startWith, and withLatestFrom.
- filtering operators: debounceTime, distinctUntilChanged, filter, take, and takeUntil.
- multicast operators: shareReplay.
- transformation operators: concatMap, map, mergeMap, scan, and switchMap.
```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';
/*
 *  'of' allows you to deliver values in a sequence
 *  In this case, it will emit 1,2,3,4,5 in order.
 */
const dataSource = of(1, 2, 3, 4, 5);
// subscribe to our source observable
const subscription = dataSource
  .pipe(
    // add 1 to each emitted value
    map(value => value + 1)
  )
  // log: 2, 3, 4, 5, 6
  .subscribe(value => console.log(value));
```

## Pipe
The pipe function is the assembly line from your observable data source through your operators. Just like raw material in a factory goes through a series of stops before it becomes a finished product, source data can pass through a pipe-line of operators where you can manipulate, filter, and transform the data to fit your use case.

```ts
// observable of values from a text box, pipe chains operators together
inputValue
  .pipe(
    // wait for a 200ms pause
    debounceTime(200),
    // if the value is the same, ignore
    distinctUntilChanged(),
    // if an updated value comes through while request is still active cancel previous request and 'switch' to new observable
    switchMap(searchTerm => typeaheadApi.search(searchTerm))
  )
  // create a subscription
  .subscribe(results => {
    // update the dom
  });

```
## Main differences between the Promise and the Observable are :
- the Promise is eager, whereas the Observable is lazy,
- the Promise is always asynchronous, while the Observable can be either asynchronous or synchronous,
- the Promise can provide a single value, whereas the Observable is a stream of values (from 0 to multiple values),
- you can apply RxJS operators to the Observable to get a new tailored stream.

## Observable vs Promise vs Function vs Iterator
- I am sure the term came in you life in the order
  Function -> Iterator -> Promise -> Observable
  In nut shell all are function :) its just the fancy name for the design pattern they have been implemented on


- Observable: Observables are lazy Push collections of multiple values.

----  | -------  SINGLE ------|  MULTIPLE
Pull  | 	Function      |  Iterator
Push  | 	Promise	      |  Observable

## Hot vs Cold Observable
- Cold observables start running upon subscription, i.e., the observable sequence only starts pushing values to the observers when Subscribe is called. (…) 

- This is different from hot observables such as mouse move events or stock tickers which are already producing values even before a subscription is active.

- Making a “cold” Observable “hot” is easy by using the RxJS operator SHARE.

```ts
import { share } from 'rxjs/operators';

var observable1 = Observable.create((observer: any) => {
    observer.next('Observable One is alive: ' + Date.now());
}).pipe(share());

var subscription1 = observable1.subscribe(
    (x:any) => logItem(x, 1) // its get the first value from .next and complete 
);
var subscription2 = observable1.subscribe(
    (x:any) => logItem(x, 2) // it will get nothing as subs1 and 2 both are sharing the same state
);
```

## Unicasting vs Multicasting
- By default, a subscription creates a one on one, one-sided conversation between the    
  observable and observer. This is like your boss (the observable) yelling (emitting) at you (the observer) for merging a bad PR. This is also known as UNICASTING.

- If you prefer a conference talk scenario - one observable, many observers - you will take a
  different approach which includes MULTICASTING with Subjects (either explicitly or behind the scenes).
## Subject vs ReplaySubject vs BehaviourSubject vs AsyncSubject
- The Subject class inherits both Observable and Observer, in the sense that it is both an observer and an observable. You can use a subject to subscribe all the observers, and then subscribe the subject to a backend data source. In this way, the subject can act as a proxy for a group of subscribers and a source.

- ReplaySubject stores all the values that it has published. Therefore, when you subscribe to it, you automatically receive an entire history of values that it has published, even though your subscription might have come in after certain values have been pushed out. 

- BehaviourSubject is similar to ReplaySubject, except that it only stores the last value it published. BehaviourSubject also requires a default value upon initialization. This value is sent to observers when no other value has been received by the subject yet. This means that all subscribers will receive a value instantly on subscribe, unless the Subject has already completed.

- AsyncSubject is similar to the Replay and Behavior subjects, however it will only store the last value, and only publish it when the sequence is completed. You can use the AsyncSubject type for situations when the source observable is hot and might complete before any observer can subscribe to it. In this case, AsyncSubject can still provide the last value and publish it to any future subscribers.

## Schedulers
A scheduler controls when a [subscription starts]() and when [notifications are published]()
It consists of three components.
- 1) A DATA STRUCTURE.
When you schedule for tasks to be completed, they are put into the scheduler for queueing based on priority or other criteria.
- 2) EXECUTION CONTEXT: which denotes where the task is executed (e.g., in the immediately, current thread, or in another callback mechanism such as setTimeout or process.nextTick).
- 3) Lastly, it has a CLOCK which provides a notion of time for itself (by accessing the now method of a scheduler). Tasks being scheduled on a particular scheduler will adhere to the time denoted by that clock only.

## Schedule Types
When to Use Which Scheduler
To make things a little easier when you are creating your own operators, or using the standard built-in ones, which scheduler you should use. The following table lays out each scenario with the suggested scheduler.

```
Scenario	                Scheduler

Constant Time Operations	Rx.Scheduler.immediate
Tail Recursive Operations	Rx.Scheduler.immediate
Iteration Operations	        Rx.Scheduler.currentThread
Time-based Operations	        Rx.Scheduler.default
Asynchronous Conversions	Rx.Scheduler.default
Historical Data Operations	Rx.HistoricalScheduler
Unit Testing	                Rx.TestScheduler
```

## Use case for map and <ul>flatMap(@deprecated)</ul> or mergeMap ?


```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

// when we transform the value into non-Observable use Map
of(1,2,3)
 .pipe(
         map((x) => '$ ' + x)
 )
 .subscribe((v) => console.log(`value: ${v}`));

// when we transform the value into Observable and wants to get the normal value when subscripe use flatMap or mergeMap
 of(1,2,3)
 .pipe(
         flatMap((x) => inerval(1000)
                                .pipe(map( i => (`${i} = ${x}`))); // inner observable
                                // try replace flatMap with map and mergeMap and observe the output
         )
 )
 .subscribe((v) => console.log(`value: ${v}`));
```

```html
<div *ngIf='rich$ | async as richData; else getRich'>{{richData}}</div>
<ng-template #getRich>
  keep learning...
</ng-template>
```



### [Throttle vs Debounce] (https://redd.one/blog/debounce-vs-throttle)
- A throttled function is called once per N amount of time. Any additional function calls within the specified time interval are ignored.

Throttle is a spring that throws balls: after a ball flies out it needs some time to shrink back, so it cannot throw any more balls unless it's ready.

- A debounced function is called after N amount of time passes since its last call. It reacts to a seemingly resolved state and implies a delay between the event and the handler function call.

Debounce is an overloaded waiter: if you keep asking him your requests will be ignored until you stop and give him some time to think about your latest inquiry.


- [Differences](https://stackoverflow.com/questions/25991367/difference-between-throttling-and-debouncing-a-function)

+--------------+-------------------+-------------------+
|              |  Throttle 1 sec   |  Debounce 1 sec   |
+--------------+-------------------+-------------------+
| Delay        | no delay          | 1 sec delay       |
|              |                   |                   |
| Emits new if | last was emitted  | there is no input |
|              | before 1 sec      |  in last 1 sec    |
+--------------+-------------------+-------------------+
Explanation by use case:

Search bar- Don't want to search every time user presses key? Want to search when user stopped typing for 1 sec. Use debounce 1 sec on key press.

Shooting game- Pistol take 1 sec time between each shot but user click mouse multiple times. Use throttle on mouse click.

Reversing their roles:

Throttling 1 sec on search bar- If users types abcdefghij with every character in 0.6 sec. Then throttle will trigger at first a press. It will will ignore every press for next 1 sec i.e. bat .6 sec will be ignored. Then c at 1.2 sec will again trigger, which resets the time again. So d will be ignored and e will get triggered.

Debouncing pistol for 1 sec- When user sees an enemy, he clicks mouse, but it will not shoot. He will click again several times in that sec but it will not shoot. He will see if it still has bullets, at that time (1 sec after last click) pistol will fire automatically.


# map vs mergeMap vs concatMap vs switchMap
- map: Apply projection with each value from source

- memregMap: This operator is best used when you wish to flatten an inner observable but want to manually control the number of inner subscriptions.

Be aware that because mergeMap maintains multiple active inner subscriptions at once it's possible to create a memory leak through long-lived inner subscriptions.

No order guaranted 

- concatmap: Map values to inner observable, subscribe and emit in order. Maintain order
the difference between concatMap and mergeMap. Because concatMap does not subscribe to the next observable until the previous completes, the value from the source delayed by 2000ms will be emitted first. Contrast this with mergeMap which subscribes immediately to inner observables, the observable with the lesser delay (1000ms) will emit, followed by the observable which takes 2000ms to complete.
```ts
// RxJS v6+
import { of } from 'rxjs';
import { concatMap, delay, mergeMap } from 'rxjs/operators';
//emit delay value
const source = of(2000, 1000);
// map value from source into inner observable, when complete emit result and move to next
const example = source.pipe(
  concatMap(val => of(`Delayed by: ${val}ms`).pipe(delay(val)))
);
//output: With concatMap: Delayed by: 2000ms, With concatMap: Delayed by: 1000ms
const subscribe = example.subscribe(val =>
  console.log(`With concatMap: ${val}`)
);

// showing the difference between concatMap and mergeMap
const mergeMapExample = source
  .pipe(
    // just so we can log this after the first example has run
    delay(5000),
    mergeMap(val => of(`Delayed by: ${val}ms`).pipe(delay(val)))
  )
  //output: With mergeMap: Delayed by: 1000ms, With mergeMap: Delayed by: 2000ms
  .subscribe(val => console.log(`With mergeMap: ${val}`));
```

- switchMap: The main difference between switchMap and other flattening operators is the cancelling effect. On each emission the previous inner observable (the result of the function you supplied) is cancelled and the new observable is subscribed. You can remember this by the phrase switch to a new observable.

# mergeMap
```ts 
of("hound", "mastiff", "retriever")        //outer observable
  .pipe(
    mergeMap(breed => {
      const url = 'https://dog.ceo/api/breed/' + breed + '/list';
      return this.http.get<any>(url)       //inner observable   
    })
  )
  .subscribe(data => {
    console.log(data)
  })
```

# mergeMap with forkJoin
```ts
  of("hound", "mastiff", "retriever")
    .pipe(
      mergeMap(breed => {
        const url1 = 'https://dog.ceo/api/breed/' + breed + '/list';
        const url2 = 'https://dog.ceo/api/breed/' + breed + '/images/random';
 
        let obs1= this.http.get<any>(url1)
        let obs2= this.http.get<any>(url2)
 
        return forkJoin(obs1,obs2)
 
      })
    )
    .subscribe(data => {
      console.log(data)
    })
```

# switchMap
```ts
ngOnInit() {
    this.activatedRoute.paramMap
      .pipe(
        switchMap((params: Params) => {
          return this.service.getProduct(params.get('id'))
        }
        ))
      .subscribe((product: Product) => this.product = product);
}
```

```ts
 
this.mainForm.get("productCode").valueChanges
.pipe(
  debounceTime(700),
  switchMap(val => {
    return this.queryDepositData();
  })
)
.subscribe(data => {
  this.product=data;
})
```
## The ExhaustMap
- differs from the MergeMap, SwitchMap & ConcatMap the way it handles the inner observable. ExhaustMap always waits for the inner observable to finish. It ignores any value it receives from the source during this period. Any value it receives after the inner observable is finished is accepted and it creates a new inner observable.
```ts
 
let srcObservable= of(1,2,3,4)
let innerObservable= of('A','B','C','D')
 
srcObservable.pipe(
  exhaustMap( val => {
    console.log('Source value '+val)
    console.log('starting new observable')
    return innerObservable
  })
)
.subscribe(ret=> {
  console.log('Recd ' + ret);
})

```

- The ExhaustMap receives its values from the srcObservable. For each value, it creates a new observable i.e. innerObservable. It also automatically subscribes to the innerObservable. The innerObservable emits the values (A, B, C, D), and pushes it to the subscribers.

- The exhaustMap is useful in scenarios, where you want to prevent submission of duplicate values
For Example, the user clicking on the Submit button twice will trigger two HTTP calls to the back end. The ExhaustMap will prevent the creation of new HTTP call until the previous one finishes



## map vs pluck vs mapTO
- map: The map operator in RxJS transforms values emitted from the source observable based on a provided projection function. 

- pluck: any time we just want to grab a single property from an emitted value, instead of using map we could use pluck. The pluck operator accepts a list of values which describe the property you wish to grab from the emitted item. 

- mapTo: mapping to constant value,

```ts
import { fromEvent } from 'rxjs';
import { mapTo } from 'rxjs/operators';
const click$ = fromEvent(document, 'click');
// method-1

click$
  .pipe(map(() => 'You clicked!'))
  // 'You clicked!', 'You clicked!'
  .subscribe(console.log);

// method-2
click$
  .pipe(mapTo('You clicked!'))
  // 'You clicked!', 'You clicked!'
  .subscribe(console.log);

```

## switchMap vs switchMapTo
- switchMap takes a function which gets invoked when a new value comes from up stream. The result of that function is subscribed to as a new stream.
- signature: switchMap(project: function: Observable, resultSelector: function(outerValue, innerValue, outerIndex, innerIndex): any): Observable

- switchMapTo takes an Observable which is subscribed to for every value that comes from up stream. 
- signature: switchMapTo(innerObservable: Observable, resultSelector: function(outerValue, innerValue, outerIndex, innerIndex): any): Observable
```ts
import { interval, fromEvent } from 'rxjs';
import { switchMapTo, switchMap } from 'rxjs/operators';
fromEvent(document, 'click')
.pipe(
  // restart counter on every click
  switchMapTo(interval(1000)) // direct observable
  // or
  // switchMap(() => interval(1000)) // function returning observable
)
.subscribe(console.log);
```

# Take, TakeUntil, TakeWhile & TakeLast in Angular Observable

- The take(n) emits the first n values, while takeLast(n) emits the last n values.
- The takeUntil(notifier) keeps emitting the values until it is notified to stop.
- takeWhile(predicate) emits the value while values satisfy the predicate.
- All of the stops emitting once done.

```ts 
obs = of(1, 2, 3, 4, 5).pipe(take(2)); // takes 2 values

export class AppComponent {
  notifier = new Subject();
  obs = interval(1000).pipe(takeUntil(this.notifier)); // takes until we call the stopobs()
  ngOnInit() {
    this.obs.subscribe(val => console.log(val));
  }
  stopObs() {
    this.notifier.next();
    // this.notifier.complete();
  }
}


export class AppComponent {
  obs = of(1, 2, 7, 1, 2, 3, 1, 2, 3)
    .pipe(
      takeWhile(val => val < 3) // only first 2 values after that value 7 will break the logic 
    );
 
  ngOnInit() {
    this.obs.subscribe(val => console.log(val));
  }
}
*** Console *** 1 2

```
## first last and single
- The first emits the first matching value,
- the Last emits the last matching value &
- the Single emits only if a single value matches the predicate.
## First Vs Take(1)
- The first() (without condition & default value) and take(1) returns the first value they receive from the source and closes the observable.

- But they have a difference in behavior when the source does not emit anything.
The first() send an error notification, while take(1) will not emit anything, but closes the observable.

## Single
The Single operator emits a single value that meets the condition.

The Single operator waits for the source to complete before emitting the value
Emits a single value that meets the condition
If more than one value satisfies the condition, then it raises an error notification
If no value satisfies the condition, then it emits the value undefined
Raises the error notification, if the source does not emit any value

## scan vs reduce
- The Scan & Reduce Operators in Angular applies an accumulator function on the values of the source observable.
- The Scan Operator returns all intermediate results of the accumulation,
- while Reduce only emits the last result. Both also use an optional seed value as the initial value.

## Angular Global CSS styles

There are several ways to add Global (Application wide styles) styles to the Angular application. The styles can be added inline, imported in index.html or added via angular-cli.json or angular.json.
```ts
"styles": [
  "src/styles.css",
  "src/assets/css/morestyles.css"
   "node_modules/bootstrap/dist/css/bootstrap.min.css"
],
```
## error-handling-in-angular-applications/
 ```ts

 providers: [
    { provide: ErrorHandler, useClass: GlobalErrorHandlerService },
  ],
 
import { ErrorHandler, Injectable} from '@angular/core';
 
@Injectable()
export class GlobalErrorHandlerService implements ErrorHandler {
 
    constructor() { 
    }
  
    handleError(error) {
       console.error('An error occurred:', error.message);
       console.error(error);
       alert(error);
   }
 
}
```
## REF
- https://www.tektutorialshub.com/angular-tutorial/
