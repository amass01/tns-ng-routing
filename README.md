# Angular Routing inside NativeScript app



In an Angular application navigation is done using the **Angular Component Router**.


### Configuration

The router configuration usually consists of the following steps:

* Create a **RouterConfig** object which maps paths to components and parameters:

```
export const routes = [
   { path: "login", component: LoginComponent },
   { path: "groceries", component: GroceryListComponent },
   { path: "grocery/:id", component: GroceryComponent }
];
```

* Use the **NativeScriptRouterModule** API to import your routes:

```
import { NativeScriptRouterModule } from "nativescript-angular/router";

@NgModule({
    bootstrap: [GroceriesApp],
    imports: [
        NativeScriptRouterModule,
        NativeScriptRouterModule.forRoot(routes)
    ]
})
export class GroceriesAppModule { }
```

*  Pass your module to the **bootstrapModule** function to start your app

### Pages


NativeScript apps consist of pages which represent the separate application screens.
Pages are instances of the Page class.
Page navigation integrates with the native navigation elements on the current platform (ex. the Back button in Android or the NavigationBar in iOS).


In NativeScript you have a choice between two router outlets:

* router-outlet - replaces the content of the outlet with different component. It is the default outlet that comes from Angular.
* page-router-outlet - uses pages to navigate. The new components are shown in a new page.

### Router Links

```
@Component({
    selector: "second",
    template: `
        <StackLayout>
            <Label text="Second component" class="title"></Label>
            <Button text="GO TO FIRST" [nsRouterLink]="['/first']" class="link"></Button>
        </StackLayout>
    `
})
```

One thing you might have noticed in the code above is the **nsRouterLink** directive. It is similar to routerLink, but works with NativeScript navigation. To use it, you need to import NativeScriptRouterModule in your NgModule.

### Router Outlet

```
template: `
        <StackLayout>
            <StackLayout class="nav">
                <Button text="First"
                    [nsRouterLink]="['/first']"></Button>
                <Button text="Second"
                    [nsRouterLink]="['/second']"></Button>
            </StackLayout>

            **<router-outlet></router-outlet>**
        </StackLayout>
    `
```

The result is that with each navigation the content of the **router-outlet** is replaced with the new component



