Observables vs non-observables / event handlers
===

Event handler approach

```javascript
something1: string = 'AAA';

onClick(value) {
    this.something1 = 'BBB';
}
```

```html
<button (click)="onClick(value)">click me</button> 
```

Observable approach

```javascript
@ViewChild('elSomeButton') elSomeButton: ElementRef;

something1$: Observable<string>; 

ngOnInit() {
    this.something1$ = Observable
        .fromEvent(this.elSomeButton.nativeElement, 'click')
        .mapTo('BBB');
}
```

```html
<button #elSomeButton>click me</button> 
```

### issues with non-observable properties

 * they can be modified anywhere in the component, even in the HTML, whereas observables can't be modified after being defined.    
 * you cannot at a glance determine all the potential values of a non-observable property.
 * their changes can't contribute to other observables

### benefits of observables over properties:

 * for an observable property, all it's potential values and the paths that lead to that outcome are in one place, at it's definition. As opposed to regular properties whose "definition" could be scattered throughout a component's multiple methods. 

### benefits of Observable.fromEvent over event handlers

 * this approach allows us to have far more control over how we use DOM events
   for instance we can use the event to contribute to multiple changes in different ways
   we can map, filter, debounce etc. the event as we need 
 * we can read the javascript and know exactly how the component functions, whereas even a well named event handler doesn't unfailingly describe what is happening.

### a case for non-observable properties

If using a non-observable property makes more sense than using an observable (eg. for better performance in lists etc), then we should still use Observable.subscribe to set that property:

```javascript
@ViewChild('elSomeButton') elSomeButton: ElementRef;

something1: string = 'AAA';

ngOnInit() {
    Observable
        .fromEvent(this.elSomeButton.nativeElement, 'click')
        .mapTo('BBB')
        .subscribe((value) => {        
            this.something1 = value;
        });
}
```

```html
<button #elSomeButton>click me</button> 
```

This way other observables can contribute to this.something1 and we can still use RxJs for our interaction with the event. 

### conclusion

This approach also allows us to make use of the advice in this series:
[Save time avoiding common mistakes using RxJS](https://egghead.io/courses/save-time-avoiding-common-mistakes-using-rxjs)
