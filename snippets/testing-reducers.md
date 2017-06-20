### test (reducer)

```javascript
import { expect } from 'chai';
import * as deepFreeze from 'deep-freeze';

// app.state
import * as $FILECAMEL$Actions from './$SUT$.actions';

// SUT
import * as SUT from './$SUT$.reducers';

describe('$SUT$.reducers', () => {

  describe('default', () => {

    it('should return default by default', () => {
      const r = SUT.$STATENAME$(DEFAULT_$DEFAULT$, {});
      expect(r).to.equal(DEFAULT_$DEFAULT$);
    });
  });
  
  $END$  
});
```

### describe (reducer)

```
describe('$ACTION$', () => {

    $END$
});
```

### it (reducer)

```
it('$SHOULD$', () => {

  const state: any = {}; // state before test for this state endpoint
  const payload: any = {}; // optional payload for action
  const action: any = $SUT$Actions.$ACTION$(payload); // action we are testing
  
  // deep freeze all objects and arrays to ensure no mutations occur
  deepFreeze(state);
  deepFreeze(payload);
  deepFreeze(action);
  
  // test
  const r = SUT.$STATENAME$(state, action);
  
  expect(r).to.equal($END$);
});
```
