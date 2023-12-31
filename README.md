# Angular Course

Angular is a JavaScript Framework which allows you to create creative Single-Page-Applications.

To start a new project `ng new [appname] --no-strict --standalone false --routing false`

> Course Structure -> Basics -> Components&Databinding -> Directives -> Services &Dependency Injection -> Routing -> Observables -> Forms -> Pipes -> Http -> Authentication -> Obtimizations & NgModules -> Deployment

> TypeScript: More features than vanilla JS like Types, Classes, Interfaces. It compiled to JavaScript handled by the CLI

- How Angular Apps gets Loaded and Started: main.ts started, then we boostrap an Angular application, and we pass this module as an argument and analyze it.

## Components

Its own business logic, it allows you to split up your complex application, into reusable parts.

> Creating a new component: `ng generate component [name]` or `ng g c [name]`

## Decorators

Is a TypeScript feature, which allow you to enhance your classes.
`@Component({
  selector: '[app selector unique name]',
  templateUrl: './path into html',
})`

or the @NgModule decorator
`@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent],
})`

Properties:

1. bootstrap: Which components you should be aware of when app starts.
2. declerations: Register new components in this property.
3. imports: Allows us to use another modules

## Databinding

Databinding = Communication.
Output data from TS code into HTML. `String Interpolation ( {{ data }})` or `Property Binding([property]='data')`

Click event using Event Binding ((event)="expression")

The combination of Outputdata and React to Events are -> _Two-Way-Binding_ ([ngModel]="data")

1. String Interpolation: A great tool for outputting data from my ts file into the html template.
2. Property Binding: [disabled]="booleanVal"
3. Event Binding: (click)="function()"

- Property Binding vs. String Interpolation:

## Directives

Directives are Instructions in the DOM.
It tells Angular to listen to anything you enter and store it in the given name property.

1. \*ngIf: Super important director.
2. \*ngStyle: Dynamically assign a style.
3. \*ngClass: Dynamically assign or remove CSS classes.
4. \*ngFor: `let logItem of log; let i = index;` for getting the index.
5. \*ngSwitch: Like switch case

Attribute Directive: You never destory, you only change properties of that element, for example the backgroundColor.
Structural Directives: Affect the whole DOM or the view container.(directive with a \*, Angular willl transform them to something else, )

Creating a Basic Attribute Directive:
Filename: better-highlight.directive.ts

```
@Directive({
  selector: '[appBasicHighlight]'
})
export class BasicHighlightDirective implements OnInit{
  constructor(private elementRef: ElementRef){
  }

  ngOnOnit(){
    this.elementRef.nativeElement.style.backgroundColor = 'green';
  }
}
```

> Directives Deep Dive (HostListener, HostBinding, ngFor, ngIf, ngClass, ngStyle, Attribute Directive)

- Using HostBinding to Bind to Host Properties:

```
@HostBinding('style.backgroundColor') backgroundColor: string = 'transparent'; //On the element this directive sits, please access the style property.
```

- Using HostListener to Listen to Host Events:

```
@HostListener('mouseenter') mouseover(eventData: Event){
  this.backgroundColor = 'blue';
}

@HostListener('mouseleave') mouseleave(eventData: Event){
    this.backgroundColor = 'transparent';
}
```

## Databinding

Properties are only accessible in their components.

- Binding to Custom Properties: using `@Input('properties you want to use outside this component')`
- `@Output()`

- Binding to Custom Events: `EventEmitter<>` this.serverCreated.emit();

> Understanding view encapsulation: Only your component receives the styles you define for it, and it can be overwritten.

- ngOnInit(): A lifecycle hook, called once the component is initialized.
- ngOnChange(): Called after a bound input property changes.
- ngDoCheck(): Called during every chagne detection run.
- ngAfterContentInit: Called after content(ng-content) has been projected into view.
- ngAfterContentChecked:Called every time the projected content has been checked.
- ngAfterViewInit: Called after the component's view(and child views) has been initialized.
- ngAfterViewCheck: Called every time the view(and child views)has been checked.
- ngOnDestroy: Called once the component is about to be destroyed.

## Servers & Dependency Injection

Injecting is to add to the @Component a providers:[name of the service].

## Observables

Observables are that stream of data, and whenever a new data has emitted, our subscribtion will know about it.

> Its added by a package name rxjs, you find it in package.json which lists all our dependencies, you find rxjs there.

Variuos data sources, user inputs events, http requests, triggered in code ...

> Observer: This is actually your code, its the `subscribe` function.
> 3 Data packages we can receive, Handle Data, Handle Error, Handle Completion.

- params is an observable to which we subscribe, to be informed about changes in data.
- interval is an timer event emitter `interval(period: 1000).subscribe(count => { console.log(count)});` it increments value (count) by 1 every second. (be aware of memory leak, starting new observable after each click for example).
- To prevent memory leak.

```
private firstObservableSubscribtion: Subscription;
this.firstObservableSubscription = interval(1000).subscribe(count => {console.log(count)});
```

And we can also implement

```
ngOnDestroy(): void{
  this.firstObservableSubscription.unsubscribe();
}
```

That means that whenever we leave that component(navigate away), we clear the previous subscription.

- We can also build our own custom observable:
  observer.next():

```
const customIntervalObservable = Observable.create(observer => {
  setInterval(()=> {
     observer.next(count);
     count++;
      }, 1000)
})

this.firstObservableSubscribtion.subscribe(data=> console.log(data));
```

observer.error():

```
const customIntervalObservable = Observable.create(observer => {
  setInterval(()=> {
     observer.next(count);
     if(count > 3) {
      observer.error(new Error("Count is greater that 3 !"));
     }
     count++;
      }, 1000)
})

this.firstObservableSubscribtion.subscribe(data=> console.log(data));
```

observer.complete():

```
const customIntervalObservable = Observable.create(observer => {
  setInterval(()=> {
     observer.next(count);
     if(count == 2){
      observer.complete();
     }
     if(count > 3) {
      observer.error(new Error("Count is greater that 3 !"));
     }
     count++;
      }, 1000)
})

this.firstObservableSubscribtion.subscribe(data=> {
  console.log(data);
}, error => console.log(error);
  alert(error.message) );
```

## Operators

The magic feature of RxJS library

## Forms

### How Angular works with Forms

Angular retrives the data used at the form, and check things like is the form valid...
Angular offers 2 approaching for handling forms:

1. Template-Driven: Angular infers the Form Object form the DOM.
2. Reactive: Form is created programmatically and synchronized with the DOM.

- Template-Driven:

1. Registering the Controls: using `ngModel` as a directive may be available on Form module.

- Reactive:

## Pipes

The main purpose is transforming values.

```
username = 'Max'

<p> {{ username }} </p>
```

I want to change it while represeiting it to the view, so we use | pipes.

```
username = 'Max'

<p> {{ username | uppercase }} </p>
```

## HTTP

- How to I connect Angular to Database?
  We don't connect Angular to a database directly, this would be highly in-secure, because everyone can read you Angular code. So instead we use HTTP request/respond from a server, a server is an API.

- The anatomy of Http request: GET, POST, PATCH, DELETE.

## Authentication

> How Authentication Works?

We have a Client, and a Server, when the user enters his credentials, so the e-mail and password, then this authentication data is sent to a server where that data is validated.
If all the data is valid, the server will send the client a TOKEN, a JSON Web Token typically. That is an encoded string which contain a lot of metadata and that string is encoded not encrypted, which means this string could be unpacked and read by the client. Only the server knows and only the server can validate incoming tokens for their validity.
