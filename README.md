# First steps in angular:

### Modules:
* Importing modules
```ts
import {Component} from 'angular2/core';
```

### Components:
* A components contains application logic tha controls a region of the UI that we call a view.
* Lets look a declaration:

  ```ts
  // Imports
  import {Component} from 'angular2/core';
  
  // Metadata (describe the component)
  @Component({  
    selector: 'my-app',
    templateUrl: './app.component.html' // what is rendered
  })
  
  // Class (define the component)
  export class AppComponent { 
    cats: Cat[];
    name = 'Hassan'
  }
  ```
* As we see, **Component** identifies its **Template**. **Component** has logic
* We should declare AppComponent(something like ShellWindow in wpf) that contains others nested components.
* **main.ts**(by convention) is a entry point of the app, this is where we start (bootstraping a component)
  
    ```ts
      import { bootstrap }    from '@angular/platform-browser-dynamic';
      import { AppComponent } from './app.component';
      
      bootstrap(AppComponent);
    ```
    
### Templates:
* Template may contain : HTML, Directives(as needed. e.g. *ngFor, *ngIf), Template binding syntax
* Components have templates, which may contain other components.
* To use nested components add to **directives** array nested components:

  ```ts
   @Component({  
      selector: 'my-app',
      templateUrl: './app.component.html',
      directives: [ComponentA, ComponentZ]
    })
  ```

### Metadata:
* Metadata links the Template to the Component
  
  ```ts
    @Component({  
      selector: 'my-app',
      templateUrl: './app.component.html',
      styleUrls:  ['./app.component.css'],
      directives: [ComponentA, ComponentZ],
      providers:  [HTTP_PROVIDERS, SomeService]
    })
  ```
  - **selector**: the name of the element in the Template
  - **templateUrl** and styleUrls: point to styles and template
  - **directives**: declare custom derictives it uses
  - **providers**: declare Services

### Component Communication:
* Components allow input properties to flow in (from parent to child), while output events allow a child Component to communicate with a parent Component. [See example](http://a2-first-look.azurewebsites.net/examples/component-input-output/plnkr.demo.html?bust=1462360273196)
* **@Output**: Component can communicate to anyone hosting it.
* **@Input**: Pass values into the Component
* **@ViewChild**: When parent component needs to access a member of its child Component.[See in example vehicle-list.component.ts](http://a2-first-look.azurewebsites.net/examples/storyline-tracker/plnkr.demo.html?bust=1462360273196) 

### Data Binding
* Interpolation: {{expression}} (DOM <-- Component). It's one way to the template

  ```html
  <h2>{{hero.name}} details!</h2>
  <img src="{{hero.imageUrl}}">
  ```
* One way binding: [property] = "expression" (DOM <-- Component). We set properties and events of DOM elements, not attributes.

   ```html
   <!--Element property-->
    <img [src]="hero.imageUrl">
    
    <!--Component property-->
    <hero-detail [hero]="currentHero"></hero-detail>
    
    <!--Directive property-->
    <div [ngClass] = "{selected: isSelected}">X-Wing</div>
  ```
  As we see, we can bind to element, Component and Directive property.
  Another example:
  ```html
  <!--Attribute binding. For attributes use attr-->
  <button [attr.aria-label]="ok">ok</button>
  
  <!--Class property binding. Use dots for nested properies-->
  <div [class.isStopped]="isStopped">stopped</div>
  
  <!--Style property binding-->
  <button [style.color]="isStopped ? 'red' : 'blue'"></button>
  ```
  
* Event Binding: (event) = "statement" (DOM --> Component)
* Two way binding: [(ngModel)] = "property" (DOM <-- Component)
  
 
  
