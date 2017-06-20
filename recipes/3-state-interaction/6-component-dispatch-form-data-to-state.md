Component : dispatch form data to state
===

```javascript
Observable
  .fromEvent(this.elPhoneNumberFormSubmitButton.nativeElement, 'click')
  .debounceTime(1000)
  .withLatestFrom(this.phoneNumberForm.statusChanges, (event, valid) => valid)
  .filter((status) => status === 'VALID')
  .withLatestFrom(this.phoneNumberForm.valueChanges, (valid, value) => value)
  .distinctUntilChanged((a, b) => JSON.stringify(a) === JSON.stringify(b))
  .withLatestFrom(this.memberProfilePhoneNumber$, (formData, stateData) => ({ formData, stateData }))
  .map(({ formData, stateData }) => memberProfilePhoneNumberActions.update({
    id: stateData.id,
    data: {
      number: formData.number,
      phone_prefix: formData.country.callingCodes[0],
      preferred: formData.preferred,
      type: formData.type
    }
  }))
  .subscribe(this.store.dispatch);
```

next is the same as above but with comments:

```javascript
// start with the submit button click
Observable
  .fromEvent(this.elPhoneNumberFormSubmitButton.nativeElement, 'click')
  // debounce form clicks
  .debounceTime(1000)
  // next get the latest emission from the form's status change observable
  .withLatestFrom(this.phoneNumberForm.statusChanges, (event, valid) => valid)
  // don't continue if form is invalid
  .filter((status) => status === 'VALID')
  // next get the latest emission from the form's value change observable
  .withLatestFrom(this.phoneNumberForm.valueChanges, (valid, value) => value)
  // make sure the form's value has actually changed
  .distinctUntilChanged((a, b) => JSON.stringify(a) === JSON.stringify(b))
  // next get the state data (purely to get some unique identifier to pass along to the update action), id in this case 
  .withLatestFrom(this.memberProfilePhoneNumber$, (formData, stateData) => ({ formData, stateData }))
  // now we have the state data & the form data and we can call our update action
  // with the payload it asks for
  .map(({ formData, stateData }) => memberProfilePhoneNumberActions.update({
    id: stateData.id,
    data: {
      number: formData.number,
      phone_prefix: formData.country.callingCodes[0],
      preferred: formData.preferred,
      type: formData.type
    }
  }))
  // next dispatch the final subscribe value 
  .subscribe(this.store.dispatch);
```

If we need to call either update or create depending on some data then we can update the last map function as follows:

```javascript
.map(({ formData, stateData }) => {

  const data = {
    number: formData.number,
    phone_prefix: formData.country.callingCodes[0],
    preferred: formData.preferred,
    type: formData.type
  };

  if (stateData === null) {
    this.router.navigate(['/my-profile/details']);
    return memberProfilePhoneNumberActions.create({ data });
  }

  return memberProfilePhoneNumberActions.update({ id: stateData.id, data });
})
```

And lastly, if this were a create form, instead of an update form, then we could skip grabbing the state data eg.
 
```javascript
// dispatch form changes to state
Observable
  .fromEvent(this.elPhoneNumberFormSubmitButton.nativeElement, 'click')
  .debounceTime(1000)
  .withLatestFrom(this.phoneNumberForm.statusChanges, (event, valid) => valid)
  .filter((status) => status === 'VALID')
  .withLatestFrom(this.phoneNumberForm.valueChanges, (valid, value) => value)
  .distinctUntilChanged((a, b) => JSON.stringify(a) === JSON.stringify(b))
  .map((formData) => memberProfilePhoneNumberActions.create({
    data: {
      number: formData.number,
      phone_prefix: formData.country.callingCodes[0],
      preferred: formData.preferred,
      type: formData.type
    }
  }))
  .subscribe(this.store.dispatch);
```
