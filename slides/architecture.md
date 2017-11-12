# Angular Architecture


## Basic building blocks of Angular 2:
* Modules
* Components
* Templates
* Metadata
* Data binding
* Directives
* Services
* Dependency Injection


## Architecture Overview
![asd](img/architecture/architecture.png)


## Modules
Angular apps are modular and Angular has its own modularity system called NgModules.
NgModule is a decorator function that takes a single metadata object whose properties describe the module.
```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent }  from './app.component';

@NgModule({
  imports: [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```


### Modules properties
- ***declarations*** - the view classes that belong to this module.
- ***exports*** - the subset of declarations that should be visible and usable in the component templates of other modules.
- ***imports*** - other modules whose exported classes are needed by component templates declared in this module.
- ***providers*** - creators of services that this module contributes to the global collection of services.
- ***bootstrap*** - the main application view, called the root component, that hosts all other app views. Only the root module should set this bootstrap property.

<!-- .element: class="fs-80" -->


## Components
The core concept of any Angular application is the component. In effect, the whole application can be modeled as a tree of these components.

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'world';
}

```


## Templates
You define a component's view with its companion template. A template is a form of HTML that tells Angular how to render the component.

A template looks like regular HTML, except for a few differences. Here is a template for our AppComponent:
```
<h2>Hello {{title}}</h2>
```


## Metadata
Metadata tells Angular how to process a class. In TypeScript, you attach metadata by
using a decorator. Here's some metadata for *AppComponent*:
```
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent { ... }
```


## Data binding
Data binding is a mechanism for coordinating parts of a template with parts of a component.
Add binding markup to the template HTML to tell Angular how to connect both sides.
![asd](img/architecture/databinding.png)


## Directives
A directive is a custom HTML element that is used to extend the power of HTML.
Two kinds of directives exist: structural and attribute directives.
```
<li *ngFor="let item of items"></li>
<input [(ngModel)]="name">
```


## Services
Service is a broad category encompassing any value, function, or feature that your application needs.

Almost anything can be a service. A service is typically a class with a narrow, well-defined purpose. It should do something specific and do it well.

```
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```


## Dependency Injection
Dependency injection is a way to supply a new instance of a class with the fully-formed dependencies it requires. Most dependencies are services. Angular uses dependency injection to provide new components with the services they need.

```
constructor(private service: HeroService) { }
```
