# Pipes


### Pipes in Angular 2
Angular 2 provides a new way of filtering data: `pipes`. Pipes are a replacement for Angular1.x's filters.


### Using Pipes
```
import {Component} from '@angular/core';

@Component({
  selector: 'product-price',
  template: `<p>Total price of product is {{ price | currency }}</p>`
})

export class ProductPrice {
  price: number = 100.1234;
}
```


### Passing Parameters
```
import {Component} from '@angular/core';

@Component({
	selector: 'product-price',
	templateUrl: 'product-price.component.html'
})
export class ProductPrice {
  price: number = 100.123456;
}
```

```html
<p>Total price of product is {{ price | currency: "CAD": true: "1.2-4" }}</p>
```


<!-- .element: class="fs-80" -->
### Custom pipes
```
import {Pipe, PipeTransform} from '@angular/core';

const FILE_SIZE_UNITS = ['B', 'KB', 'MB', 'GB', 'TB'];
const FILE_SIZE_UNITS_LONG = ['Bytes', 'Kilobytes', 'Megabytes', 'Gigabytes'];

@Pipe({
  name: 'formatFileSize'
})
export class FormatFileSizePipe implements PipeTransform {
  transform(sizeInBytes: number, longForm: boolean): string {
    const units = longForm ? FILE_SIZE_UNITS_LONG : FILE_SIZE_UNITS;
    let power = Math.round(Math.log(sizeInBytes)/Math.log(1024));
    power = Math.min(power, units.length - 1);
    const size = sizeInBytes / Math.pow(1024, power); // size in new units
    const formattedSize = Math.round(size * 100) / 100; // keep up to 2 decimals
    const unit = units[power];
    return `${formattedSize} ${unit}`;
  }
}
```


<!-- .element: class="fs-80" -->
### Async Pipe
```
import {Component} from '@angular/core';
import {Observable} from 'rxjs/Observable';
@Component({
  selector: 'product-price',
  template: `product-price.component.html`
})

export class ProductPrice {
  count: number = 0;
  
  fetchPrice: Promise<number> = new Promise((resolve, reject) => {
     setTimeout(() => resolve(10), 500);
  });
  
  seconds: Observable<number> = new Observable(observer => {
    setInterval(() => { observer.next(this.count++); }, 1000);
  });
}
```

```html
<!--product-price.component.html-->
<p>Total price of product is {{fetchPrice | async | currency: "CAD": true: "1.2-2" }}</p>
<p>Seconds: {{seconds | async}}</p>
```