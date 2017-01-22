# Agular 2 Earwig Tutorial

* [Learning Angular 2](https://angular.io/docs/ts/latest/guide/learning-angular.html)
* [Angular 2 architecture](https://angular.io/docs/ts/latest/guide/architecture.html)
* [The Angular 2 root module](https://angular.io/docs/ts/latest/guide/appmodule.html)
* [Angular 2 components] (http://learnangular2.com/components/)
* [Displaying Data with Angular 2](https://angular.io/docs/ts/latest/guide/displaying-data.html)


Subchapter | Original source  
-----------|-----------------  
**[Components](#components)** | [http://learnangular2.com/components/](http://learnangular2.com/components/)  
**[Inputs](#inputs-input)** | [http://learnangular2.com/inputs/](http://learnangular2.com/inputs/)  
**[Outputs](#outputs-output)** | [http://learnangular2.com/outputs/](http://learnangular2.com/outputs/)  

--------------------------------------------------------------------

## Naming conventions

In Javascript / Typescript there is no mandatory naming concept for classes,
packages, modules, ...

The following rules are commonly accepted:

* **File names:**  
  Use only lower case characters, combinded with `.` and/or `-`  
  Example: `hero-app.component.ts`
* **Class names:**  
  Use [PascalCase](https://en.wikipedia.org/wiki/PascalCase) notation  
  Example: `HeroAppComponent`
* **Angular 2 architecture elements:**  
  Add a point and the element name for the filename.  
  Example *Component*: `app.component.ts`  
  Example *Service*: `app.service.ts`
* **PascalCase words:**  
  Use a `-` to separate the words in the filename.  
  Example for `class PersonListManganger`: `person-list-manager.component.ts`
* **Function and member names**  
  Use [CamelCase](https://en.wikipedia.org/wiki/Camel_case) notation  
  Example: `getFirstname()`

If every class is written in a separate file, a lot of resources must 
be loaded by the browser. Therefore such classes are often combined to a single file.  
In small projects, it is also a commonly used practice to locate small classes together in
a single file. 

A typical **folder structure** for Angular 2 project may be:  

```
  controllers/
  directives/
  services/
  shares/
  templates/
  util/
```

Need more information... [http://stackoverflow.com/questions/35346342](http://stackoverflow.com/questions/35346342)





--------------------------------------------------------------------

## New folders and spearate files for each class

In most Angular 2 Tutorials multiple classes are located inside the 
single file `app.component.ts`.

In this project we want a separate file for each class. 
We want also try out how to use multiple folders in the `src/angular2` directory,
in order to support a good folder structure in our project.

We create the folder [src/angular2/shared] and the class file 
[hero.ts](src/angular2/shared/hero.ts) inside this folder.

Each new folder needs some configuration lines in the file [systemjs.config.js](public/systemjs.config.js).  
For example for the new folder `shared`

```
  ...
  map: {
    ...
    shared: 'app/shared',
    ...
  },
  packages: {
    ...
    shared: {
      defaultExtension: 'js'
    },
    ...
  },
  ... 
```

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

```typescript
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

--------------------------------------------------------------------

### Outputs (@Output)

If you want to bind to particular event, you can use the new 
[Event syntax](http://learnangular2.com/events) 
in Angular 2, but what if you need your own custom event?

To create a custom event, we can use the new `@Output` decorator. 
Take the following component:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'user-profile',
  template: '<div>Hi, my name is {{user.name}}</div>'
})
export class UserProfile {
  constructor() {}
}
```

Let’s import `Output` and `EventEmitter` and create our new event

```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'user-profile',
  template: '<div>Hi, my name is {{user.name}}</div>'
})
export class UserProfile {
  @Output() userUpdated = new EventEmitter();

  constructor() {
    // Update user
    // ...
    this.userUpdated.emit(this.user);
  }
}
```
Now when we used this component elsewhere in our app, 
we can bind the event that `user-profile` emits

```typescript
<user-profile (userUpdated)="handleUserUpdated($event)"></user-profile>
```

```typescript
export class SettingsPage {
  constructor(){}

  handleUserUpdated(user) {
    // Handle the event
  }
}
```
