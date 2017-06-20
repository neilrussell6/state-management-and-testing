Component : request data into state
===

> This can be done in a resolver or in a component constructor, wherever makes sense.
> Basically we will ask the store to populate state with the data we want.

### imports

```javascript
// ng-redux
import { select, NgRedux } from '@angular-redux/store';

// rxjs
import { Observable } from 'rxjs/Observable';

// app.state
import { IAppState } from 'app/state/store.interfaces';
import * as memberProfileActions from 'app/state/member-profile/member-profile.actions';
```

### constructor

```javascript
constructor(
  private store: NgRedux<IAppState>
) {
    this.store.dispatch(memberProfileActions.view())
}
```
