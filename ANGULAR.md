# ANGULAR

[home page](https://angular.io/)
[material Angular](https://material.angular.io/)

## WHAT IS IT

## ANGULAR CLI

[home page](https://cli.angular.io/)

### what is it
Angular CLI is a very powerful CLI which nicely complements (or even sustain) the Angular landscape. it is the key tool which you will be living by as soon as you start creating app, component, directive,.... anything in Angular.
better to get familiarized with it.

https://medium.com/codingthesmartway-com-blog/creating-angular-projects-with-angular-cli-e32b2cb486da

## SETUP

## HELLO WORLD

## BASICS OF COMPONENTS AND DIRECTIVES

## ONE LEVEL DEEPER

### ANGULAR Forms
https://blog.ng-book.com/the-ultimate-guide-to-forms-in-angular-2/ 


#### Reactive Forms

#### how to deactivate forms based on entry of another field
see [here](https://www.technouz.com/4725/disable-angular-reactiveform-input-based-selection/)
example of situation is profile editor which we want to be editable on demand
https://www.technouz.com/4725/disable-angular-reactiveform-input-based-selection/

## MORE DETAILS


### ANGULAR MATERIAL
[home page](https://material.angular.io/)
#### THEMING YOUR APP
example [here](https://www.positronx.io/create-angular-material-8-custom-theme/)

### NGX-Permissions
Ngx permission is an angular package which allows to enable RBAC on the front end. Stay aware though it is not changing the strategy in place on your back-end.

### PROGRESSIV WEBAPP

### REALTIME app
https://www.djamware.com/post/5e50b735525fc968b04a707f/mean-stack-angular-9-build-realtime-crud-web-app-quickly#express-router 

### SERVICE WORKERS / WEBWORKERS

### Authentication with Firebase
[here](https://www.youtube.com/watch?v=O_jxEC0hWcA&t=225s)

### UI LIBRARY


- [PrimeNG](https://primefaces.org/primeng/showcase/#/setup): offers more than 70 UI component ready to use in angular webapp
- [GoldenLayout](https://github.com/EmbeddedEnterprises/ng6-golden-layout): has a docking windows mechanism
  
### Theming your app

dynamic themes
- change the stylesheet: http://www.carbonrider.com/2019/03/27/angular-change-theme-at-runtime/
- with observable and class on component: https://blog.wb.gy/build-dynamic-themes-for-angular-material/
- https://nerdhut.de/2020/01/13/dynamic-angular-material-themes/
  
Theme can be applied either on the entirety of the app or to a subpart of it.
They can be changed dynamically.
Let's see how the inner panel only of our app can be themed with a dynamic change.

Note: many references over the internet indicates to use either ``--style=sass`` or ``--style=scss`` option for the creation of the app with ``angular CLI``, it seems that ``scss`` is the only really working.

#### creation of the theme
There are plenty of tutorial for the creation of the theme itself. Let's not dig into this here.
Good practice is to avoid modifying the ``styles.scss`` which is found into ``src`` folder. This one is not meant to host theme and this would create confusion in the code structure.
Instead, one should consider the ``theme`` as an asset of the application and describe into the ``assets\themes`` folder which one will create.
In this folder, have a ``xxx-theme.scss`` file created and simply import it into your ``src\styles.scss``.
````scss
@import "./assets/styles/xxx-theme";
````

In your ``xxx-theme.Scss`` file have the proper styles created or referenced
````scss
@import "~@angular/material/theming";

@include mat-core();

$primary: mat-palette($mat-indigo);
$accent: mat-palette($mat-pink, A200, A100, A400);
$warn: mat-palette($mat-red);
$theme: mat-light-theme($primary, $accent, $warn);

@include angular-material-theme($theme);

// Our dark theme
.dark-theme {
  color: $light-primary-text;
  $dark-primary: mat-palette($mat-yellow);
  $dark-accent: mat-palette($mat-amber, A400, A100, A700);
  $dark-warn: mat-palette($mat-red);
  $dark-theme: mat-dark-theme($dark-primary, $dark-accent, $dark-warn);

  @include angular-material-theme($dark-theme);
}
````

Now that the files are created, let's consider how to use them into the application.

#### applying a theme
the angular Material document refers to an ``OverlayKit`` or something. this is to be furter explored especially when using custom component it seems.

the most efficient approach for dynamically changing the style of a component is to use ``class binding``.
So in the component which is to be theme, add the following piece of code:
````html
    <div [ngClass]="{'dark-theme': isThemeDark | async}">
      <div class="mat-app-background">
        
      </div>
    </div>
````
the class binding allows then to dynamically change the class (note the usage of observable here with the ``async`` pipe).

In the Typescript component code, one shall add the reference to the theme itself,
````ts
... //add the observable for class binding
isThemeDark: Observable<boolean>;
...
````
This observable is initialized during OnInit to connect to the a service in which the theme is modified.
This service is ``ThemeService``. It will be described later on.
in your component,
- inject the ThemeService
- initialize isThemeDark to the said reference in ThemeService
  
````ts
constructor(private breakpointObserver: BreakpointObserver,
            ...
            private theme: ThemeService
            ) {}

ngOnInit(){
  this.auth.logout();
  this.isThemeDark = this.theme.isThemeDark;
}
````
Last but not least, one must add in the UI a way to select the theme itself.
Let's put it, for now, as a simple toggle button which triggers a function in the component.
````html
    <mat-slide-toggle
      [checked]="isThemeDark | async"
      (change)="onChangeTheme($event.checked)"
      >Dark theme</mat-slide-toggle
    >
````
add this function in the component code
````ts
  onChangeTheme(checked: boolean){
    this.theme.setTheme(checked);
  }
````

#### capturing the change event and propagating
last but not least, let's create our service for Theme management.
So far, not firm recommendation as per where to put this service, so I put it into shared (as it is likely to be shared...).

````ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ThemeService {
private themeDark: Subject<boolean> = new Subject<boolean>();
isThemeDark = this.themeDark.asObservable();
  constructor() { }

  setTheme(theme: boolean){
    this.themeDark.next(theme);
  }
}
````
Code is pretty simple it offers the possibility to change the theme with the ``setTheme`` which only swithes the subject ``boolean``.
Then a public Observable is shared on the Subject... this is the observable that we will connect to.