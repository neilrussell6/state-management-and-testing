Component : using selected data
===

> Once selected we can use the state data in our JavaScript or HTML.

> The selected state data behaves just like any Observable, 
> so you can subscribe to it or use it in your HTML with the async pipe.

```javascript
this.memberProfile$.subscribe((memberProfile) => {
    console.log(memberProfile);
    this.memberProfile = memberProfile; // <-- try to avoid this sort of thing
});
```

```html
{{ memberProfile$ | async | json }}
```

```html
{{ (memberProfile$ | async)?.first_name }}
```

```html
<li *ngFor="let phoneNumber of memberProfilePhoneNumbers$ | async">
  is preferred: {{ phoneNumber.preferred }}
</li>
```
