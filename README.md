# Brief-README-Angular

This is a description of an Angular tutorial completed at
- https://angular.io/start/
- https://angular.io/start/start-routing
- https://angular.io/start/start-data .

Applications are built in Angular by creating and using components.
A component consists of three parts :
- a component class ;
- an HTML template to help display the component ;
- a component-specific style file.

The superclass of all components is `<app-root>`, which is placed in the file `index.html` of the application :

`<html lang="en">`
 ` <head> ... </head>`
    `<body>`
     	`<app-root></app-root>`
   ` </body>`
`</html>`


Structural directives
----------------------------

A structural directive is an example of a class in Angular. There are many built-in directives, which have the prefix '*ng', for example :
`*ngIf`, `*ngForOf`, and `*ngSwitch`.

We include Angular structural directives by inserting the appropriate imports at the head of the component class for example :

`import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';`

To create a new structural directive `newStrDir`, the following command is executed on the command line :
`ng generate directive newStrDir`.

A structure directive can be embedded in an HTML template. In the following, a `<div>` is created for each product in products(.ts) using a `for loop` :

`<div *ngFor="let product of products">`
  `<h3>`
      `{{ product.name }}`
  `</h3>`
`</div>`


Interpolation syntax and property binding
-----------------------------------------------------------

Properties can be displayed in the HTML template using Angular's interpolation syntax, as in the example above for the product name (`product.name`).

A property value can be inserted, for example, into the title of the component using the `property-binding` syntax (`[]`). In the following `<a>` HTML element, the product name prefixes the text `details` in the `title` :

    <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>


Event binding
-------------------

An event can trigger an action by means of `event binding`. In the following HTML statement, the `click` of a given product `Buy` button adds the product to a shopping cart by means of the method `addToCart()`, which is defined in the `*.ts` file of the component :

  <button type="button" (click)="addToCart(product)">Buy</button>


Conditions
---------------

A condition can be expressed by an `*ngIf` directive. In the following, a `<p>` element is created and displayed only if a description of a given product exists : 

  `<p *ngIf="product.description">`
    `Description: {{ product.description }}`
  `</p>`


Associate a URL path with a component
--------------------------------------------------------

To define the URL path of a component, its path must be indicated in the file
`app.module.ts`.
For example, a component that describes a given product in our example can be found by adding the line

`{ path: 'products/:productId', component: ProductDetailsComponent },`

to `app.module.ts` and using the `[routerLink]` directive in the HTML file of the component

  `<h3>`
    `<a`
      `[title]="product.name + ' details'"`
      `[routerLink]="['/products', product.id]">`
      `{{ product.name }}`
    `</a>`
  `</h3>`
  
This particular path has a fixed part `/products` and a variable part `product.id`, which varies with the product ID.


Accessing component properties : the `Angular router`
----------------------------------------------------------------------------

A component can be accessed at a given path name using the directive of the Angular Router, by inserting the following import in the component `*.ts` file :

`import { ActivatedRoute } from '@angular/router';`

To extract a given property of a component, its so-called current `route parameters` are retrieved in the method `ngOnInit()` using `route.snapshot`, as in the example below :

`export class ProductDetailsComponent implements OnInit {`
  `product: Product | undefined;`
  `constructor(private route: ActivatedRoute) { }`
  `ngOnInit() {`
  	`const routeParams = this.route.snapshot.paramMap;`
  	`const productIdFromRoute = Number(routeParams.get('productId'));`
	`…..`
   `}`
`}`



Implementing a service -  Angular's `dependency injection system`
--------------------------------------------------------------------------------------------

An interface `Product` for instance can be imported into a component to provide static information about product data :

`import { Product } from './products';`

The product data can then stored in an array Product

`items: Product[] = [];`

To monitor the variable contents of a shopping cart however, a `service` needs to be created 
`ng generate service cart`
and then inserted within an appropriate component by means of the  Angular's dependency injection system. 

To add items to and get or clear items from a shopping cart, new methods are then added directly to the new cart-service component class : 

`export class CartService {`
  `items: Product[] = [];`  
  
  `constructor() { }`
  
  `addToCart(product: Product) {`
    `this.items.push(product);`
  `}`

  `getItems() {`
    `return this.items;`
  `}`

  `clearCart() {`
    `this.items = [];`
    `eturn this.items;`
  `}`
`}`

The component `CartService` can then be `injected` into the constructor of another component `ProductDetailsComponent` :

`export class ProductDetailsComponent implements OnInit {`

  `constructor(`
    `private route: ActivatedRoute,`
    `private cartService: CartService`
  `) { }`
`}`

A given product can then be added to a cart using the method in the same component class

  `addToCart(product: Product) {`
    `this.cartService.addToCart(product);`
    `window.alert('Your product has been added to the cart!');`
  `}`

which calls another method `addToCart()` of the new cart-service component on the `click` of a button `Buy` :
  `<button type="button" (click)="addToCart(product)">Buy</button>` .


A component and its service
---------------------------

To visualise the cart, another component `Cart` is created:
`ng generate component cart` .

The cart service can then be imported into this cart component class:

`import { Component } from '@angular/core';`
`import { CartService } from '../cart.service';`

`export class CartComponent {`
  `constructor(`
    `private cartService: CartService`
  `) { }`
`}`