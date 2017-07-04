### 1. API config

define resource key & endpoint
``app/state/api/api.config.ts``

### 2. defaults

define defaults and update default state tree
``app/state/store.defaults.ts``

eg. ``app/state/store.defaults.ts``
```
DEFAULT_RESTAURANTS = null

export const DEFAULT_APP_STATE: IAppState = {
  restaurants: DEFAULT_RESTAURANTS,
  ...
```

eg. ``app/state/store.interfaces.ts``
```
export interface IAppState {
  restaurants: Array<IDiningRestaurantCardEntity>;
```

### 3. Actions

create actions

live templates group: ``ng-ts-state-management``
quick reference live templates: ``actions, action, apiactions``

### 4. epics (with tests soon) *OPTIONAL

create & register epic
eg. ``app/state/member-profile/member-profile.epics.ts``

register in ``app/state/store.module.ts``

### 5. reducer with tests

create, test & register reducer
eg .``app/state/restaurant/restaurant.reducers.ts``

register in ``app/state/store.module.ts``

live templates group: ``ng-ts-testing``
quick reference live templates: ``test, testredf, desc, it, r``

### 6. resolver *OPTIONAL

dispatch index or view in resolver
eg. ``app/resolvers/countryList.resolve.ts``

### 7. select / dispatch in component

live templates group: ``ng-ts-component``
quick reference live templates: ``sel, dis, stateimports``
