Using state for dummy data (simplest approach)
===

> This is the simple approach, and requires no tests or epics, but move onto the [better approach](2-using-state-for-dummy-data-better-apporach.md) as soon as you have your head around this approach.

### endpoint 

We will use the term ``endpoint`` to describe a root property in the state tree
eg. given this state tree:

```javascript
{
  user: { url: ... },
  memberProfile: { first_name: ... },
  memberProfilePhoneNumbers: [ ... ]
}
```
user, memberProfile & memberProfilePhoneNumbers are all state endpoints. 

Steps
---

1) create an interface for your endpoint
2) create default for endpoint
3) create the action creators for the endpoint 
4) create a reducer for the endpoint (hard-coding the dummy data)
5) register the reducer
6) dispatch an action to add the dummy data to state and select it

Details
---

> For this tutorial we will be creating an endpoint for memberProfile

### 1) create an interface for your endpoint

create ``src/app/interfaces/member-profile.interface.ts``

and populate:

```javascript
export interface IMemberProfile {
  id?: number;
  birth_date?: string;
  ...
}
```

### 2) create default for endpoint

Create a default value for memberProfile and update the default AppState:

open ``src/app/state/store.defaults.ts``

and update:

```javascript
export const DEFAULT_MEMBER_PROFILE: IMemberProfile = null; // <-- add this

export const DEFAULT_APP_STATE: IAppState = {
  ...
  memberProfile: DEFAULT_MEMBER_PROFILE, // <-- add this
  ...
};
```

### 3) create the action creators for the endpoint

create ``src/app/state/member-profile/member-profile.actions.ts``

and populate:

```javascript
import { IAction } from '../store.interfaces';

// --------------------------------------
// action constants
// --------------------------------------

// view
export const MEMBER_PROFILE_VIEW = 'MEMBER_PROFILE_VIEW';
export const MEMBER_PROFILE_VIEW_SUCCESS = 'MEMBER_PROFILE_VIEW_SUCCESS';

// --------------------------------------
// actions creators
// --------------------------------------

export const view        = (): IAction => ({ type: MEMBER_PROFILE_VIEW });
export const viewSuccess = (): IAction => ({ type: MEMBER_PROFILE_VIEW_SUCCESS });
```

### 4) create a reducer for the endpoint (hard-coding the dummy data)

create ``src/app/state/member-profile/member-profile.reducers.ts``

and populate:

```javascript
// app.state
import { DEFAULT_MEMBER_PROFILE } from '../store.defaults';
import * as memberProfileActions from './member-profile.actions';

export const memberProfile = (state, action) => {
  switch (action.type) {

    case memberProfileActions.MEMBER_PROFILE_VIEW_SUCCESS:
      return {
        // ... dummy data (replace this with whatever you need, this can also be an array or integer or anything)
      };

    default:
      return state;
  }
};
```

### 5) register the reducer

open ``src/app/state/state/store.module.ts``

and update as follows:

```javascript
// app.state.reducers
import { memberProfile } from './member-profile/member-profile.reducers';  // <-- add this

const rootReducer = composeReducers(
  defaultFormReducer(),
  combineReducers({
    ...
    memberProfile, // <-- add this
    ...
  })
);
```

### 6) dispatch an action to add the dummy data to state and select it

open your component

and update as follows:

```javascript

// redux
import { select, NgRedux } from '@angular-redux/store'; // <-- add this

// rxjs
import { Observable } from 'rxjs/Observable'; // <-- add this

// app.interfaces
import { IMemberProfile } from 'app/interfaces/member-profile.interface'; // <-- add this

// app.state
import { IAppState } from 'app/state/store.interfaces'; // <-- add this
import * as memberProfileActions from 'app/state/member-profile/member-profile.actions'; // <-- add this

...
export class YourComonent {
  
  // selectors
  @select('memberProfile') readonly memberProfile$: Observable<IMemberProfile>; // <-- add this

  constructor(
    ...
    private store: NgRedux<IAppState> // <-- add this
  ) {
    this.store.dispatch(memberProfileActions.view()); // <-- add this
  }
```

> For more info on selecting data from state and using it see [/recipes/3-state-interaction](https://github.com/neilrussell6/state-management-and-testing/blob/master/recipes/3-state-interaction)
