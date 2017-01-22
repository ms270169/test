# Agular 2 Tutorial

Original source: [http://learnangular2.com/](http://learnangular2.com/)

Chapter:

* Components: [http://learnangular2.com/components/](http://learnangular2.com/components/)
* [Inputs](#Inputs): [http://learnangular2.com/inputs/](http://learnangular2.com/inputs/)

--------------------------------------------------------------------

## Components

In Angular 2, “everything is a component.”

Components are the main way we build and specify elements and logic on the page,
through both custom elements and attributes that add functionality to our existing components.

### A simple component


Here’s a simple [Component](https://angular.io/docs/ts/latest/api/core/index/Component-decorator.html) 
that renders our name, and a button that triggers a method to print our name to the console:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'my-component',
  template: '<div>Hello my name is {{name}}. <button (click)="sayMyName()">Say my name</button></div>'
})

export class MyComponent {
  constructor() {
    this.name = 'Max'
  }
  sayMyName() {
    console.log('My name is', this.name)
  }
}
```
When we use the `<my-component></my-component>` tag in our HTML, 
this component will be created, our constructor called, and rendered.

--------------------------------------------------------------------

### Inputs (@Input)

Components are the core of an Angular 2 app but most developers need 
to know how to pass data into components to dynamically configure them.

To define an input for a component, we use the **@Input** decorator.

For example, our `<user-profile>` component needs a user argument 
to render information about that user:

```javascript
<user-profile [user]="currentUser"></user-profile>
```

So, we add an `@Input` binding to `user`:

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'user-profile',
  template: '<div>{{user.name}}</div>'
})
export class UserProfile {
  @Input() user;
  constructor() {}
}
```
