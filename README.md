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
    
  
