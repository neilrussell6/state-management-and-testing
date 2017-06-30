State Management and testing guidelines
===

part 1 (basic coding guidelines and selecting from state)
---

### components (general guidelines)

 - favour RxJs where-ever possible, so:
   - avoid all properties that are not either selectors, observables or element refs (they can usually be replaced with some RxJs approach)
   - avoid methods (there is usually a better RxJs approach)
 - avoid Subjects, instead:
   - use ViewChild element refs and Observable.fromEvent for event handling
 - know if your component is smart or dumb and name it accordingly
 - use formBuilder to create forms and specify validators at creation point
   - we can then update the view using a combination of:
     - myForm.controls.myControl.value
     - myForm.controls.myControl.valid
     - myForm.controls.myControl.touched
     - myForm.controls.myControl.errors

### smart components

 - get all your state using @select
 - don't mutate any state
 - name these xxx.container instead of xxx.component (but still use XxxComponent for the clas name)
 - don't interact directly with services (epics will do this if neccessary)

### dumb components

 - these can have their own state
 - you can do whatever you want in these (but still follow the component guidelines above), except:
   - interact with store in any way
   - mutate any state outside the dumb component

### services

 - don't create these unless you absolutely have to (eg. ApiService & LocalStorageService)
 - this functionality is better solved with a combination of libraries, epics and reducers (which we will go over soonish).
