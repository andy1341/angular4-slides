# Forms


### Froms Types
The Angular framework provides us a couple of alternative ways for handling forms:

* Template Driven Forms
* Model Driven or Reactive Forms


<!-- .element: class="fs-80" -->
### Enabling Forms
Unlike the case of AngularJs, ngModel and other form-related directives are not available by default,
we need to explicitly import them in our application module:
* `FormsModule` - for Template Driven Forms
* `ReactiveFormsModule` - for Reactive Froms

```
// app.module.ts
// ...
import {FormsModule, ReactiveFormsModule} from "@angular/forms";

@NgModule({
    declarations: [App],
    imports: [BrowserModule, FormsModule, ReactiveFormsModule],
    bootstrap: [App]
})
export class AppModule {}
```


### Template Driven Form
```
<section class="sample-app-content">
    <h1>Template-driven Form Example:</h1>
    <form #f="ngForm" (ngSubmit)="onSubmitTemplateBased()">
        <p>
            <label>First Name:</label>
            <input type="text"  
                [(ngModel)]="user.firstName" required>
        </p>
        <p>
            <label>Password:</label>
            <input type="password"  
                [(ngModel)]="user.password" required>
        </p>
        <p>
            <button type="submit" [disabled]="!f.valid">Submit</button>
        </p>
    </form>
</section>
```


### Form Validation
```
<input id="name" name="name" class="form-control"
       required minlength="4" forbiddenName="bob"
       [(ngModel)]="hero.name" #name="ngModel" >

<div *ngIf="name.invalid && (name.dirty || name.touched)"
     class="alert alert-danger">

  <div *ngIf="name.errors.required">
    Name is required.
  </div>
  <div *ngIf="name.errors.minlength">
    Name must be at least 4 characters long.
  </div>
  <div *ngIf="name.errors.forbiddenName">
    Name cannot be Bob.
  </div>

</div>
```


### Advantages and disadvantages of Template Driven Forms
+ Easy to use
+ Suitable for simple scenarios
+ Two way data binding(using [(NgModel)] syntax)
+ Minimal component code
- Difficult for complex scenarios
- Unit testing is another challenge


### Model Driven Or Reactive Forms
```
<form [formGroup]="loginForm" (ngSubmit)="login()">
  <label for="username">username</label>
  <input type="text" name="username" id="username" [formControl]="username">
  <br>

  <label for="password">password</label>
  <input type="password" name="password" id="password" [formControl]="password">
  <br>

  <button type="submit">log in</button>
</form>
```


<!-- .element: class="fs-80" -->
### Reactive From Component
```
import { Component } from '@angular/core';
import { FormGroup, FormControl, FormBuilder } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: 'app/app.component.html'
})
export class AppComponent {
  username = new FormControl('')
  password = new FormControl('')

  loginForm: FormGroup = this.builder.group({
    username: this.username,
    password: this.password
  });

  constructor(private builder: FormBuilder) { }

  login() {
    console.log(this.loginForm.value);
    // Attempt Logging in...
  }
}
```


<!-- .element: class="fs-70" -->
### Reactive Froms Validations
```
import { Component } from '@angular/core';
import { Validators, FormBuilder, FormControl } from '@angular/forms';

@Component({
  // ...
})
export class AppComponent {
  username = new FormControl('', [
    Validators.required,
    Validators.minLength(5)
  ]);

  password = new FormControl('', [Validators.required]);

  loginForm: FormGroup = this.builder.group({
    username: this.username,
    password: this.password
  });

  constructor(private builder: FormBuilder) { }

  login () {
    console.log(this.loginForm.value);
    // Attempt Logging in...
  }
}
```


<!-- .element: class="fs-70" -->
### Reactive Froms Validations
```
<form [formGroup]="loginForm" (ngSubmit)="login()">
  <div>
    <label for="username">username</label>
    <input type="text" name="username" id="username" [formControl]="username">

    <div [hidden]="username.valid || username.untouched">
      <div> The following problems have been found with the username: </div>
      <div [hidden]="!username.hasError('minlength')"> Username can not be shorter than 5 characters. </div>
      <div [hidden]="!username.hasError('required')"> Username is required. </div>
    </div>
  </div>
  
  <div >
    <label for="password">password</label>
    <input type="password" name="password" id="password" [formControl]="password">

    <div [hidden]="password.valid || password.untouched">
      <div> The following problems have been found with the password: </div>
      <div [hidden]="!password.hasError('required')"> The password is required. </div>
    </div>
  </div>

  <button type="submit" [disabled]="!loginForm.valid">Log In</button>
</form>
```


<!-- .element: class="fs-80" -->
### Advantages and disadvantages of Reactive Forms
+ More flexible, but needs a lot of practice.
+ Handles any complex scenarios.
+ No data binding is done(Immutable data)
+ More component code and less HTML markup
+ Reactive transformations can be made possible such as
+ Handling a event based on a debounce time
+ Hanlding events when the components are distinct until changed
+ Adding elements dynamically
+ Easier Unit testing
