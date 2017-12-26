# Observables


<!-- .element: class="fs-70" -->
### Observables
```html
<b>Angular 2 Component Using Observables!</b>
<h6>VALUES:</h6>
<div *ngFor="let value of values">- {{ value }}</div>
<h6>ERRORs: {{anyErrors}}</h6>
<h6>FINISHED: {{ finished }}</h6>
```
<!-- .element: class="right" -->

```typescript
import {Component, OnInit} from '@angular/core';
import {Observable} from 'rxjs/Observable';

@Component({
  selector: 'app',
  template: 'observable.component.html'
})
export class MyApp implements OnInit{
  private data: Observable<Array<number>>;
  private values: Array<number> = [];
  private anyErrors: boolean;
  private finished: boolean;
  
  ngOnInit() {
    this.data = new Observable(observer => {
      setTimeout(() => observer.next(42), 1000);
      setTimeout(() => observer.next(43), 2000);
      setTimeout(() => observer.complete(), 3000);
    });
    
    let subscription = this.data.subscribe(
      value => this.values.push(value),
      error => this.anyErrors = true,
      () => this.finished = true
    );
  }
}
```
<!-- .element: class="left" -->


<!-- .element: class="fs-70" -->
### Disposing Subscriptions and Releasing Resources
```typescript
export class MyApp implements ngOnInit {
  private data: Observable<Array<string>>;
  private value: string;
  private subscribed: boolean;
  private status: string;

  ngOnInit() {
    this.data = new Observable(observer => {
      let timeoutId = setTimeout(() => { 
          observer.next('You will never see this message')
          }, 2000);
      this.status = 'Started';
      
      return onUnsubscribe = () => {
        this.subscribed = false;
        this.status = 'Finished';
        
        clearTimeout(timeoutId);
      }
    });
    
    let subscription = this.data.subscribe(
      value => this.value = value,
      error => console.log(error),
      () => this.status = 'Finished'
    );

    this.subscribed = true;
    
    setTimeout(() => subscription.unsubscribe(), 1000);
  }
}
```


<!-- .element: class="fs-80" -->
### Using Observables to get data from an API service
```typescript
@Injectable()
export class ApiService {
  private _postsURL = "https://some.posts";

  constructor(private http: Http) {}

  getPosts(): Observable<Post[]> {
    return this.http.get(this._postsURL)
      .map((response: Response) => {
          <Post[]>response.json()
      }).catch(this.handleError);
  }

  private handleError(error: Response) {
    return Observable.throw(error.statusText);
  }
}
```
<!-- .element: class="left" -->

```typescript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  providers: [ApiService]
})
export class AppComponent implements OnInit {
  _postsArray: Post[];

  constructor(private apiSerivce: ApiService) {}

  getPosts(): void {
    this.apiSerivce.getPosts().subscribe(
      resultArray => this._postsArray = resultArray,
      error => console.log("Error :: " + error)
    )
  }

  ngOnInit(): void {
    this.getPosts();
  }
}
```
<!-- .element: class="right" -->


### Observables vs Promises
* As seen in the example above, `Observables` can define both the setup and teardown aspects of asynchronous behavior.
* `Observables` are cancellable.
* Moreover, `Observables` can be retried using one of the retry operators provided by the API, such as
`retry` and `retryWhen`. On the other hand, Promises require the caller to have access to the original 
function that returned the promise in order to have a retry capability. 