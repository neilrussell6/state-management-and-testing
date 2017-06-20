Component : dispatch data to state
===

### imports

```javascript
// ng-redux
import { NgRedux } from '@angular-redux/store';

// rxjs
import { Observable } from 'rxjs/Observable';

// app.state
import { IAppState } from 'app/state/store.interfaces';
import * as memberProfilePhoneNumberActions from 'app/state/member-profile-phone-number/member-profile-phone-number.actions';
```

### constructor

```javascript
constructor(
  private store: NgRedux<IAppState>
) {}
```

### dispatching

> This can be done from within the constructor or ngOnInit, depending on whether we need any view elements to be loaded.

```javascript
Observable.fromEvent(this.elNotPreferredButton.nativeElement, 'click')
  // create our action
  .map((event) => memberProfilePhoneNumberActions.update({ data: { preferred: false }}))
  .subscribe((action) => {
    this.store.dispatch(action);
  });
```

The subscribe part can more concisely be written as:

```javascript  
  .subscribe(this.store.dispatch);
```
