# First steps in angular:

### Modules:
* Importing modules
```ts
import {Component} from 'angular2/core';
```

### Components:
* A components contains application logic that controls a region of the UI that we call a view.
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

  ```html
  <!--Element event-->
  <button (click)="save()">Save</button>
  <!--Component event-->
  <hero-detail (changed)="heroChanged()"></hero-detail>
  ```
  * **$event**: contains a message about the event
  
  ```html
  <input [value]="hero.name" (input)="hero.name=$event.target.value">
  ```
* Two way binding: [(ngModel)] = "property/expression" (DOM <-- Component). Banana in a box. 

### Built-in Directives:
* ngClass:
  [ngClass]="{active:IsActive, color: myColor}"

  ```html
   <!-- Style binding.  Alternative to [class.class-name] -->
    <div [ngClass]="setClass()">{{hero.name}}</div>
  ```
* ngStyle: . Settining multiple styles.
  [ngStyle]="{color: colorPreference}"
  [style.color]="colorPreference"

 ```html
 <!-- Style binding. Alternative to [style.style-name] -->
 <div [ngStyle]="setStyles()">{{hero.name}}</div>
 ```
* *ngFor

  ```html
  <!-- "#"-declare local variable -->
  <div *ngFor="#hero of heroes, #i=index">
    {{i}}. {{hero.name}}
  </div>
  ```
* *ngIf: Conditionally removes elements from the DOM.
* *ngSwitch

### Pipes
* **Pipes**: allow us to transform data for display in a Template.(filters in angular1)
* **Async Pipe**: Subscribes to Promise or an Observalable, returning the latest value emmited
* **Custom pipes**:   

### Services

### Dependency Injection
* Use contsructor injection
* Add @Injectable() attribute
* Registering Sevices with the Injector:
  * Providers: registers services with Injector
  * Constructor Injection: inject 
  * **Register the service with injector at the parent that contains all components  that require the service**

### Component Lifecycle Hooks
* Allow us to tap into specific moments in the app. lifecycle to perform logic.
* Implement, for example, the lifecycle hook's interface **OnInit**(support ngOnInit() executed when Component initilizes)

### Http
* We use Http to get and save data with Prommises or Observables.
* Http step by step:
  * Add script refernce to http(http.dev.js).
  * Register the Http providers in the Component. 
  
    ```ts
    import { Component } from 'angular2/core';
    import { HTTP_PROVIDERS } from 'angular2/http'; // IMPORTING HTTP_PROVIDERS
    
    import { Vehicle, VehicleService } from './vehicle.service';
    import { VehicleListComponent } from './vehicle-list.component';
    
    @Component({
      selector: 'my-app',
      template: '<my-vehicle-list></my-vehicle-list>',
      directives: [VehicleListComponent],
      providers: [
        HTTP_PROVIDERS,                            // REGISTER
        VehicleService
      ]
    })
    export class AppComponent {}
    ```
  * Call Http.get in a Service and return the mapped result.
    ```ts
    @Injectable()
    export class VehicleService {
      constructor(private _http: Http) { }
    
      getVehicles() {
        return this._http.get('api/vehicles.json')
          .map((response: Response) => <Vehicle[]>response.json().data) // map the response
          .do(data => console.log(data))
          .catch(this.handleError);
      }
      
      // Handle any error
      private handleError(error: Response) {
        console.error(error);
        return Observable.throw(error.json().error || 'Server error');
      }
    }
    ```
  * Subscribe to the Service's function in the Component:
    ```ts
    getHeroes() {
    this._vehicleService.getVehicles()
      .subscribe(                                 // subscribe to the observable
        vehicles => this.vehicles = vehicles,     // success asnd failer cases
        error =>  this.errorMessage = <any>error
      );
    }
    ```
    
### RxJs
* Reactive Js implements the asynchronous observable pattern that used in angular2.[reactivex.io](http://reactivex.io/)
* In main.ts we should import the library: 
  
```ts
// main.ts

import 'rxjs/Rx';
``` 
.Be carefull, it's a big library, for production import the modules you require.

### Async Pipe
 


  
 
  
