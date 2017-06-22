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

Some guidelines
---

* keep the state tree flat eg.

memberProfile
memberProfilePhoneNumbers
memberProfilePaymentCards
user

* create a directory for each state endpoint, and name it and it files in singular eg.

member-profile-phone-number
 - member-profile-phone-number.actions.ts
 - member-profile-phone-number.reducers.ts

> So even though our endpoint is ``memberProfilePhoneNumbers`` (plural) we name the directory & files in singular ``member-profile-phone-number.actions.ts``.

Before you go through the Details section below
---

In order for this tutorial to actually achieve something if you follow it, you need to do some renaming.
 
Copy everything under Details section below to a new text file and run the follow search and replaces:

``YOUR_ENDPOINT_NAME`` will refer to whatever your endpoint's name is (eg. memberProfilePhoneNumbers) 

1. replace ``FILE_NAME`` with your ``YOUR_ENDPOINT_NAME`` in singular, formatted like this: ``member-profile-phone-number``

2. replace ``INTERFACE_NAME`` with your ``YOUR_ENDPOINT_NAME`` in singular, formatted like this: ``MemberProfilePhoneNumber``

3. replace ``CONST_ACTION_NAME`` with your ``YOUR_ENDPOINT_NAME`` in singular, formatted like this: ``MEMBER_PROFILE_PHONE_NUMBER``

For the following replacements (4, 5 & 6), you need to replace with either singular or plural depending on what your endpoint is: 
  so if your data needs to be an array then it's plural,
  but if your data needs to be an object then it's singular

4. replace ``CONST_DEFAULT_NAME`` with your ``YOUR_ENDPOINT_NAME`` as follows: 
  if plural, then format like this: ``MEMBER_PROFILE_PHONE_NUMBERS``
  if singular, then format like this: ``MEMBER_PROFILE_PHONE_NUMBER``

5. replace ``ENDPOINT_NAME`` with your ``YOUR_ENDPOINT_NAME`` as follows: 
  if plural, then format like this: ``memberProfilePhoneNumbers``
  if singular, then format like this: ``memberProfilePhoneNumber``

6. replace ``TYPE_SIGNATURE`` as follows:
  if plural, then replace ``TYPE_SIGNATURE`` with your ``YOUR_ENDPOINT_NAME`` interface wrapped in an array, formatted like this: ``Array<IMemberProfilePhoneNumber>``
  if singular, then replace ``TYPE_SIGNATURE`` with your ``YOUR_ENDPOINT_NAME`` interface, formatted like this: ``IMemberProfilePhoneNumber``

Details
---

### 1) create an interface for your endpoint

create ``src/app/interfaces/FILE_NAME.interface.ts``

and populate:

```javascript
export interface IINTERFACE_NAME {
  id?: number;
  birth_date?: string;
  ...
}
```

### 2) create default for endpoint

Create a default value for ENDPOINT_NAME and update the default AppState:

open ``src/app/state/store.defaults.ts``

and update:

```javascript
export const DEFAULT_CONST_DEFAULT_NAME: TYPE_SIGNATURE = null; // <-- add this

export const DEFAULT_APP_STATE: IAppState = {
  ...
  ENDPOINT_NAME: DEFAULT_CONST_DEFAULT_NAME, // <-- add this
  ...
};
```

### 3) create the action creators for the endpoint

create ``src/app/state/FILE_NAME/FILE_NAME.actions.ts``

and populate:

```javascript
import { IAction } from '../store.interfaces';

// --------------------------------------
// action constants
// --------------------------------------

// view
export const CONST_ACTION_NAME_VIEW = 'CONST_ACTION_NAME_VIEW';
export const CONST_ACTION_NAME_VIEW_SUCCESS = 'CONST_ACTION_NAME_VIEW_SUCCESS';

// these constant names don't have to match their values
// and can be shortened, as they'll be referred to like this phoneNumberActions.CONST_ACTION_NAME_VIEW
// from within our epics, reducers & tests, components won't use these constants (they will use the functions below)
// but make sure the values are globally unique within the app
// add whatever other actions you need here

// --------------------------------------
// actions creators
// --------------------------------------

export const view        = (): IAction => ({ type: CONST_ACTION_NAME_VIEW });
export const viewSuccess = (): IAction => ({ type: CONST_ACTION_NAME_VIEW_SUCCESS });

// these are the functions that our components will call to dispatch actions
// they will be called lke this:
// store.dispatch(phoneNumberActions.view())
// add whatever other actions creators you need here
// have a look at ``state/member-profile/member-profile.actions.ts`` and ``state/member-profile-phone-number/member-profile-phone-number.actions.ts`` for reference.
```

### 4) create a reducer for the endpoint (hard-coding the dummy data)

create ``src/app/state/FILE_NAME/FILE_NAME.reducers.ts``

and populate:

```javascript
// app.interfaces
import { IMemberProfilePhoneNumber } from 'app/interfaces/member-profile-phone-number.interface';

// app.state
import { DEFAULT_MEMBER_PROFILE_PHONE_NUMBERS } from '../store.defaults';
import { IAction } from '../store.interfaces';
import * as ENDPOINT_NAMEActions from './FILE_NAME.actions';

export const ENDPOINT_NAME = (state: TYPE_SIGNATURE = DEFAULT_CONST_DEFAULT_NAME, action: IAction) => {
  switch (action.type) {

    case ENDPOINT_NAMEActions.CONST_ACTION_NAME_VIEW_SUCCESS:
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
import { ENDPOINT_NAME } from './FILE_NAME/FILE_NAME.reducers';  // <-- add this

const rootReducer = composeReducers(
  defaultFormReducer(),
  combineReducers({
    ...
    ENDPOINT_NAME, // <-- add this
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
import { IINTERFACE_NAME } from 'app/interfaces/FILE_NAME.interface'; // <-- add this

// app.state
import { IAppState } from 'app/state/store.interfaces'; // <-- add this
import * as ENDPOINT_NAMEActions from 'app/state/FILE_NAME/FILE_NAME.actions'; // <-- add this

...
export class YourComonent {
  
  // selectors
  @select('ENDPOINT_NAME') readonly ENDPOINT_NAME$: Observable<TYPE_SIGNATURE>; // <-- add this

  constructor(
    ...
    private store: NgRedux<IAppState> // <-- add this
  ) {
    this.store.dispatch(ENDPOINT_NAMEActions.viewSuccess()); // <-- add this
    
    this.ENDPOINT_NAME$ // <-- add this
      .subscribe((value) => { // <-- add this
        console.log(value); // <-- add this
      }); // <-- add this
  }
```

If you reload your component in the browser you should see two things:
  1) The value of your dummy data logged in the console
  2) A log of the ``CONST_ACTION_NAME_VIEW_SUCCESS`` action in ReduxDev tools

> For more info on selecting data from state and using it see [/recipes/3-state-interaction](https://github.com/neilrussell6/state-management-and-testing/blob/master/recipes/3-state-interaction)
