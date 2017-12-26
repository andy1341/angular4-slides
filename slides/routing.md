# Routing


### Routing allows you to:
* Maintain the state of the application
* Implement modular applications
* Implement the application based on the roles (certain roles have access to certain URLs)


### Configuring Routes
The Base URL tag must be set within the `<head>` tag of index.html:
```
<base href="/">
```


<!-- .element: class="fs-80" -->
### Route Definition Object
Each route can have different attributes; some of the common attributes are:
* `path` - URL to be shown in the browser when application is on the specific route
* `component` - component to be rendered when the application is on the specific route
* `redirectTo` - redirect route if needed; each route can have either component or redirect attribute defined in the route
* `pathMatch` - optional property that defaults to 'prefix'; determines whether to match full URLs or just the 
beginning. When defining a route with empty path string set pathMatch to 'full', otherwise it will match all paths.
* `children` - array of route definitions objects representing the child routes of this route.

```
const routes: Routes = [
  { path: 'component-one', component: ComponentOne },
  { path: 'component-two', component: ComponentTwo }
];
```


### RouterModule
```
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  { path: 'component-one', component: ComponentOne },
  { path: 'component-two', component: ComponentTwo }
];

export const routing = RouterModule.forRoot(routes);

@NgModule({
  imports: [ BrowserModule, routing],
  declarations: [AppComponent, ComponentOne, ComponentTwo],
  bootstrap: [ AppComponent ]
})
export class AppModule {}

platformBrowserDynamic().bootstrapModule(AppModule);
```


### Navigation
Add links to routes using the `RouterLink` directive.
```
<a [routerLink]="['/component-one']">Component One</a>
```

Alternatively, you can navigate to a route by calling the navigate function on the router:
```
this.router.navigate(['/component-one']);
```   


### Declaring Route Parameters
The route for the component that displays the details for a specific product would need a
route parameter for the ID of that product. We could implement this using the following Routes:
```
export const routes: Routes = [
  { path: 'products', component: ProductList },
  { path: 'products/:id', component: ProductDetails }
];
```


### Linking to Routes with Parameters
```
<a *ngFor="let product of products" [routerLink]="['/products', product.id]">
  {{ product.name }}
</a>
```

Alternatively, from function: 
```
goToProductDetails(id) {
  this.router.navigate(['/products', id]);
}
```


<!-- .element: class="fs-70" -->
### Reading Route Parameters
```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'product-details',
  template: `<div> Showing product details for product: {{id}} </div>`,
})
export class LoanDetailsPage implements OnInit, OnDestroy {
  id: number;
  private sub: any;
  
  constructor(private route: ActivatedRoute) {}
  
  ngOnInit() {
    this.sub = this.route.params.subscribe(params => {
      this.id = +params['id'];
    });
  }
  
  ngOnDestroy() {
    this.sub.unsubscribe();
  }
}
```


### Controlling Access to or from a Route
```typescript
import { Routes, RouterModule } from '@angular/router';
import { AccountPage } from './account-page';
import { LoginRouteGuard } from './login-route-guard';
import { SaveFormsGuard } from './save-forms-guard';

const routes: Routes = [
  { path: 'home', component: HomePage },
  {
    path: 'accounts',
    component: AccountPage,
    canActivate: [LoginRouteGuard],
    canDeactivate: [SaveFormsGuard]
  }
];

export const appRoutingProviders: any[] = [];
export const routing = RouterModule.forRoot(routes);
```


### Implementing CanActivate
```typescript
import { CanActivate } from '@angular/router';
import { Injectable } from '@angular/core';
import { LoginService } from './login-service';

@Injectable()
export class LoginRouteGuard implements CanActivate {
  constructor(private loginService: LoginService) {}
  
  canActivate() {
    return this.loginService.isLoggedIn();
  }
}
```


### Implementing CanDeactivate
```typescript
import { CanDeactivate } from '@angular/router';
import { Injectable } from '@angular/core';
import { AccountPage } from './account-page';

@Injectable()
export class SaveFormsGuard implements CanDeactivate<AccountPage> {
  canDeactivate(component: AccountPage) {
    return component.areFormsSaved();
  }
}
```


### Passing Query Parameters
```html
<a [routerLink]="['product-list']" [queryParams]="{ page: 99 }">Go to Page 99</a>
```

Alternatively, we can navigate programmatically using the Router service:
```typescript
goToPage(pageNum) {
   this.router.navigate(['/product-list'], { queryParams: { page: pageNum } });
}
```


<!-- .element: class="fs-70" -->
### Reading Query Parameters
```typescript
import { Component } from '@angular/core';
import { ActivatedRoute, Router } from '@angular/router';

@Component({
  selector: 'product-list',
  template: `<!-- Show product list -->`
})
export default class ProductList {
  constructor(private route: ActivatedRoute, private router: Router) {}
  
  ngOnInit() {
    this.sub = this.route.queryParams.subscribe(params => {
      this.page = +params['page'] || 0;
    });
  }
  
  ngOnDestroy() {
    this.sub.unsubscribe();
  }
  
  nextPage() {
    this.router.navigate(['product-list'], { queryParams: { page: this.page + 1 } });
  }
}
```


<!-- .element: class="fs-90" -->
### Auxiliary Routes
```html
<nav>
  <a [routerLink]="['/component-one']">Component One</a>
  <a [routerLink]="['/component-two']">Component Two</a>
  <a [routerLink]="[{ outlets: { 'sidebar': ['component-aux'] } }]">Component Aux</a>
</nav>

<div style="color: green; margin-top: 1rem;">Outlet:</div>
<div style="border: 2px solid green; padding: 1rem;">
  <router-outlet></router-outlet>
</div>

<div style="color: green; margin-top: 1rem;">Sidebar Outlet:</div>
<div style="border: 2px solid blue; padding: 1rem;">
  <router-outlet name="sidebar"></router-outlet>
</div>
```