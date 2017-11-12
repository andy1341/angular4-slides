# Components


Components are the fundamental building block of Angular 2 applications.
The “application” itself is just the top-level Component. Then we break our application into smaller
child Components. <!-- .element: class="fs-80" -->

![sad](img/components/components-flow.png)
<!-- .element: style="max-height: 400px" -->


### Creating Components
```ts
import {Component} from '@angular/core';

@Component({
  selector: 'hello',
  template: '<p>Hello, {{name}}</p>'
})

export class Hello {
  name: string;
  
  constructor() {
    this.name = 'World';
  }
}
```


### Application Structure with Components
```
<TodoApp>
  <TodoList>
    <TodoItem></TodoItem>
    <TodoItem></TodoItem>
    <TodoItem></TodoItem>
  </TodoList>
  <TodoForm></TodoForm>
</TodoApp>
```


### Passing Data into a Component
```
import {Component, Input} from '@angular/core';

@Component({
  selector: 'hello',
  template: '<p>Hello, {{name}}</p>'
})

export class Hello {
  @Input() name: string;
}
```

```
<!-- To bind to a raw string -->
<hello name="World"></hello>

<!-- To bind to a variable in the parent scope -->
<hello [name]="name"></hello>
```


<!-- .element: class="fs-90" -->
### Responding to Component Events
```
import {Component, EventEmitter, Input, Output} from '@angular/core';

@Component({
  selector: 'counter',
  template: 'counter.html'
})

export class Counter {
  @Input() count: number = 0;
  @Output() result: EventEmitter = new EventEmitter();
  
  increment() {
    this.count++;
    this.result.emit(this.count);
  }
}
```

```html
<!-- counter.html -->
<div>
  <p>Count: {{ count }}</p>
  <button (click)="increment()">Increment</button>
</div>
```


<!-- .element: class="fs-80" -->
### Responding to Component Events
```
import {Component} from '@angular/core';

@Component({
  selector: 'app',
  template: `app.html`
})

export class App {
  num: number;
  parentCount: number;

  constructor() {
    this.num = 0;
    this.parentcount = 0;
  }
  
  onChange(val: any) {
    this.parentCount = val;
  }
}
```

```html
<!-- app.html -->
<div>
  Parent Num: {{ num }}<br />
  Parent Count: {{ parentCount }}
  <counter [count]="num" (result)="onChange($event)"></counter>
</div>
```


### Two-Way Data Binding

Two-way data binding combines the input and output binding into a single notation using the ngModel directive.
```
<input [(ngModel)]="name" >
```
What this is doing behind the scenes is equivalent to:
```
<input [ngModel]="name" (ngModelChange)="name=$event">
```


### Two-Way Data Binding In Your Component
```
import {Component, Input, Output, EventEmitter} from '@angular/core';

@Component({
  selector: 'counter',
  template: 'counter.html'
})

export class Counter {
  @Input() count: number = 0;
  @Output() countChange: EventEmitter<number> = new EventEmitter<number>();
  
  increment() {
    this.count++;
    this.countChange.emit(this.count);
  }
}
```


### Projection
```
import {Component, Input} from '@angular/core';

@Component({
  selector: 'child',
  template: `
    <h4>Child Component</h4>
    <ng-content></ng-content>
  `
})
class ChildComponent {}
```

```
<child>
  <p>My projected content.</p>
</child>
```


### Projection
```
import {Component, Input} from '@angular/core';

@Component({
  selector: 'child',
  template: `
    <h4>Child Component</h4>
    <ng-content select="header"></ng-content>
    <ng-content></ng-content>
    <ng-content select="footer"></ng-content>
  `
})

class ChildComponent {}
```

```
<div>
  <child>
    <header>Header Content</header>
    Main Content
    <footer>Footer Content</footer>
  </child>
</div>
```


### Component Lifecycle
* `ngOnChanges` - called when an input binding value changes
* `ngOnInit` - after the first `ngOnChanges`
* `ngDoCheck` - after every run of change detection
* `ngAfterContentInit` - after component content initialized
* `ngAfterContentChecked` - after every check of component content
* `ngAfterViewInit` - after component's view(s) are initialized
* `ngAfterViewChecked` - af ter every check of a component's view(s)
* `ngOnDestroy` - just before the component is destroyed


### Accessing Child Component Classes
```
import {Component, QueryList, ViewChild, ViewChildren} from '@angular/core';
import {Alert} from './alert.component';

@Component({
  selector: 'app',
  template: `
    <my-alert #first ok="Next" (close)="showAlert(2)">
      Step 1: Learn angular
    </my-alert>
    <my-alert ok="Next" (close)="showAlert(3)">Step 2: Love angular</my-alert>
    <my-alert ok="Close">Step 3: Build app</my-alert>
    <button (click)="showAlert(1)">Show steps</button>`
})
export class App {
  @ViewChild('first') alerts: Alert;
  @ViewChildren(Alert) alerts: QueryList<Alert>;
  alertsArr = [];
  
  ngAfterViewInit() {
    this.alertsArr = this.alerts.toArray();
  }
  
  showAlert(step) {
    this.first.show();
  }
}
```