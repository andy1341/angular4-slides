# Redux and Ngrx


### Simple Reducer
```typescript
import { INCREMENT_COUNTER, DECREMENT_COUNTER } from '../actions/counter-actions';
 
export default function counter(state = 0, action) {
  switch (action.type) {
    case INCREMENT_COUNTER:
      return state + 1;
    case DECREMENT_COUNTER:
      return state - 1;
    default:
      return state;
  }
}
```


### Synchronous Actions
```typescript
import { Injectable } from '@angular/core';
import { NgRedux } from 'ng2-redux';

export const INCREMENT_COUNTER = 'INCREMENT_COUNTER';
export const DECREMENT_COUNTER = 'DECREMENT_COUNTER';

@Injectable()
export class CounterActions {
  constructor(private redux: NgRedux<any>) {}
  
  increment() {
    this.redux.dispatch({ type: INCREMENT_COUNTER });
  }

  decrement() {
    this.redux.dispatch({ type: DECREMENT_COUNTER });
  }
}
```


<!-- .element: class="fs-90" -->
### Asynchronous Actions
```typescript
import { Injectable } from '@angular/core';
import { NgRedux } from 'ng2-redux';

export const INCREMENT_COUNTER = 'INCREMENT_COUNTER';
export const DECREMENT_COUNTER = 'DECREMENT_COUNTER';

@Injectable()
export class CounterActions {
  constructor(private redux: NgRedux<any>) {}
  // ...
  incrementIfOdd() {
    const { counter } = this.redux.getState();
    if (counter % 2 === 0) return;
    this.redux.dispatch({ type: INCREMENT_COUNTER });
  }
  
  incrementAsync(timeInMs = 1000) {
    setTimeout(() => this.redux.dispatch({ type: INCREMENT_COUNTER }), timeInMs)
  }
}
```


### Configuring your Application to use Redux
* Register Ng2-Redux with Angular 2
* Create our application reducer
* Create and configure a store


### Registering Ng2-Redux with Angular 2
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { NgReduxModule, NgRedux } from 'ng2-redux';
import { SimpleRedux } from './containers/app-container';

@NgModule({
  imports: [
    BrowserModule,
    NgReduxModule
  ],
  declarations: [
    SimpleRedux
  ],
  bootstrap: [ SimpleRedux ]
})
class AppModule {}
platformBrowserDynamic().bootstrapModule(AppModule);
```


### Create our Application Reducer
```typescript
// app/reducers/index.ts
import { combineReducers } from 'redux';
import counter from './counter-reducer';

export default combineReducers({
  counter
});
```


### Create and Configure a Store
```typescript
// app/containers/app-container.ts
import { Component } from '@angular/core';
import { NgRedux } from 'ng2-redux';
import logger from '../store/configure-logger';
import reducer from '../reducers';

@Component({
// ...
})
class SimpleRedux {
  constructor(ngRedux: NgRedux) {
    const initialState = {};
    const middleware = [ logger ];
    ngRedux.configureStore(reducer, initialState, middleware);
  }
}
```


<!-- .element: class="fs-70" -->
### Using Redux with Components
```typescript
// app/components/counter-component.ts
import { Component } from '@angular/core';
import { select } from 'ng2-redux';
import { Observable } from 'rxjs';
import { CounterActions } from '../actions/counter-actions';

@Component({
  selector: 'counter',
  providers: [ CounterActions ],
  template: `
    <p>
    Clicked: {{ counter$ | async }} times
    <button (click)="actions.increment()">+</button>
    <button (click)="actions.decrement()">-</button>
    <button (click)="actions.incrementIfOdd()">Increment if odd</button>
    <button (click)="actions.incrementAsync()">Increment async</button>
    </p>
  `
})
export class Counter {
  @select() counter$: Observable<number>;
  constructor(public actions: CounterActions) {}
}
```