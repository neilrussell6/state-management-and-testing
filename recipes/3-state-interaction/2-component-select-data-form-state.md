Component : using selected data
===

> Once the data we need is in state, we can select it.

### imports

```javascript
// ng-redux
import { select, NgRedux } from '@angular-redux/store';

// rxjs
import { Observable } from 'rxjs/Observable';
```

### root selection

```javascript
@select('memberProfile') readonly memberProfile$: Observable<IMemberProfile>; 
```

```javascript
@select('memberProfilePhoneNumbers') readonly memberProfile$: Observable<Array<<IMemberProfilePhoneNumber>>; 
```

### property selection

```javascript
@select([ 'memberProfile', 'name' ]) readonly memberProfileName$: Observable<string>;
```
