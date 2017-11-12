# Directives


### Directives 
Directives are entities that change the behavior of components or elements and are one of
the core building blocks Angular 2 uses to build applications. In fact, Angular 2 components
are in large part directives with templates


### Directives types
* `Attribute directives` - directives that change the behavior of a component or element but
don't affect the template
* `Structural directives` - directives that change the behavior of a component or element by
affecting how the template is rendered


### Attribute directives
Attribute directives are a way of changing the appearance or behavior of a component.
Ideally, a directive should work in a way that is component agnostic and not bound to implementation details.

For example, Angular 2 has built-in attribute directives such as `ngClass` and `ngStyle` that work on any component or element.


### NgStyle Directive
```
@Component({
  selector: 'app',
  templateUrl: 'index.component.html'
})
export class App {
  borderStyle: string = '1px solid black';
}
```

```html
<!-- index.component.html-->
<p [ngStyle]="{borderBottom: borderStyle}">Amazing Title</p>
```


### NgClass Directive
There are a few different ways of using this directive:

```html
<p ngClass="warning">Binding a string</p>
<p [ngClass]="['warning', 'bold']">Binding an array</p>
<p [ngClass]="{warning: true, bold: isBold()}">Binding an object</p>

```


### Structural Directives
Structural Directives are a way of handling how a component or element renders through the use of the template tag. 
This allows us to run some code that decides what the final rendered output will be. 

Angular 2 has a few built-in structural directives such as `ngIf`, `ngFor`, and `ngSwitch`.


### NgIf Directive
```
@Component({
  selector: 'app',
  templateUrl: 'app.component.html'
})
export class AppComponent {
  exists: boolean = true;
  
  toggleExists() {
    this.exists = !this.exists;
  }
}
```

```html
<!--app.component.html-->
<button type="button" (click)="toggleExists()">Toggle Component</button>
<div *ngIf="exists">
  Hello
</div>
```


### NgFor Directive
```
@Component({
  selector: 'app',
  templateUrl: 'app.component.html'
})
export class AppComponent {
  episodes: any[] = [
    { title: 'Winter Is Coming', director: 'Tim Van Patten' },
    { title: 'The Kingsroad', director: 'Tim Van Patten' },
    { title: 'Lord Snow', director: 'Brian Kirk' },
    { title: 'Cripples, Bastards, and Broken Things', director: 'Brian Kirk' },
    { title: 'The Wolf and the Lion', director: 'Brian Kirk' },
    { title: 'A Golden Crown', director: 'Daniel Minahan' },
    { title: 'You Win or You Die', director: 'Daniel Minahan' },
    { title: 'The Pointy End', director: 'Daniel Minahan' }
  ];
}
```

```html
<!--app.component.html-->
<for-example *ngFor="let episode of episodes" [episode]="episode">
  {{episode.title}}
</for-example>

```


<!-- .element: class="fs-80" -->
### NgSwitch Directives
```
@Component({
  selector: 'app',
  templateUrl: 'app.component.html'
})
export class AppComponent {
  tab: number = 0;

  setTab(num: number) {
    this.tab = num;
  }
  
  isSelected(num: number) {
    return this.tab === num;
  }
}
```

```html
<div class="tabs-selection">
  <tab [active]="isSelected(1)" (click)="setTab(1)">Tab 1</tab>
  <tab [active]="isSelected(2)" (click)="setTab(2)">Tab 2</tab>
</div>
<div [ngSwitch]="tab">
  <tab-content *ngSwitchCase="1">Tab content 1</tab-content>
  <tab-content *ngSwitchCase="2">Tab content 2</tab-content>
  <tab-content *ngSwitchDefault>Select a tab</tab-content>
</div>
```


### Custom Directives
```
import { Directive, ElementRef, Renderer } from '@angular/core';

@Directive({ selector: '[myHidden]' })
export class HiddenDirective {
    constructor(el: ElementRef, renderer: Renderer) {
       renderer.setElementStyle(el.nativeElement, 'display', 'none');
    }
}
```