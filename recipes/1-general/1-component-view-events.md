Component : view events
===

### native elements

```html
<button #elSomeButton>click me</button>
```

```js
@ViewChild('elSomeButton') elSomeButton: ElementRef;

ngOnInit () {
  Observable.fromEvent(this.elSomeButton.nativeElement, 'click')
    .subscribe((value) => {
      console.log(value);
    });
}
```

### custom components

**some.component.ts**

```js
@Output('someEvent') someEvent: EventEmitter;
```

**parent.component.ts**

```html
<some-component #elSomeComonent></some-component>
```

```js
@ViewChild('elSomeComponent') elSomeComponent: SomeComponent;

ngOnInit () {
  this.elSomeComponent.someEvent.subscribe((value) => {
    console.log(value);
  });
}
```
