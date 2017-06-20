Component : transforming observables
===

Define the Observable in any of the following locations (in this order)

 1) at declaration
 2) in constructor
 3) in ngOnInit

### 1) at declaration

The best place to define an observable's behaviour is at declration time:

```javascript
myObs$ = Observable.from(memberProfile$)
    .withLatestFrom(countries$, (memberProfile, countries) => {
        // do something with those 2
    });
```

### 2) in constructor

But if we need to wait for some angular service to be available (eg. form builder), then we have to define the observable in the constructor.

```javascript
Observable.from(this.phoneNumberForm.statusChanges)
  .statusChanges
  .debounceTime(1000)
  .filter((status) => status === 'VALID')
  ...
```

### 3) in ngOnInit

And if we need to wait for some DOM element to load then we have to define the observable in ngOnInit.

```javascript
Observable
  .fromEvent(this.elSomeButton.nativeElement, 'click')
  .subscribe((value) {
    ...
```
