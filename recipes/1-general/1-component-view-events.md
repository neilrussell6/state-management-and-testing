Component : view events
===

### native elements

```html
<button #elSomeButton>click me</button>
```

```javascript
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

```javascript
@Output('someEvent') someEvent: EventEmitter;
```

**parent.component.ts**

```html
<some-component #elSomeComonent></some-component>
```

```javascript
@ViewChild('elSomeComponent') elSomeComponent: SomeComponent;

ngOnInit () {
  this.elSomeComponent.someEvent.subscribe((value) => {
    console.log(value);
  });
}
```
