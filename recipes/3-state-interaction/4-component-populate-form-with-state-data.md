Component : populate form with state data
===

> We can populate a form by subscribing to our selected state data, and updating the form with patchValue

```javascript
@select('memberProfile') readonly memberProfile$: Observable<IMemberProfile>; 

constructor(
  private formBuilder: FormBuilder,
  private store: NgRedux<IAppState>
) {

  // create form
  this.memberProfileForm = this.formBuilder.group({
    'first_name': [ '', Validators.required ],
    'last_name': [ '', Validators.required ]
  });

  Observable.from(this.memberProfile$)
    .subscribe((value) => {
      this.memberProfileForm.patchValue({
        first_name: value.first_name,
        last_name: value.last_name
      });
    });
}
```
