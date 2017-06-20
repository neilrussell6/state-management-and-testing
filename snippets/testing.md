### test

```
import { expect } from 'chai';
import * as deepFreeze from 'deep-freeze';

// app.
import * as SUT from './$FILE$';

describe('$FILE$', () => {

    $END$
});
```

### describe

```
describe('$FUNCTION$', () => {

    $END$
});
```

### it

```
it('$SHOULD$', () => {

    $END$
});
```

### it (learning)

```
it('$SHOULD', () => {

    // given (create the parameters we will pass to teh test function) 
    const $PARAM$ = $VALUE$
    deepFreeze($PARAM$);
    
    // when (call the test function)
    const r = SUT.$FUNCTION$($PARAM$);
    
    // then (test our expectations regarding the result)
    expect(r).to.equal($WHAT$)  
});
```
