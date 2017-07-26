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

One thing you might have noticed in the code above is the **nsRouterLink** directive. It is similar to routerLink, but works with NativeScript navigation.
To use it, you need to import **NativeScriptRouterModule** in your NgModule.

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

            <router-outlet></router-outlet>
        </StackLayout>
    `
```

The result is that with each navigation the content of the **router-outlet** is replaced with the new component.

### Page Router Outlet

```
@Component({
    selector: "page-navigation-test",
    template: `<page-router-outlet></page-router-outlet>`
})
export class PageNavigationApp {
}

@NgModule({
    declarations: [PageNavigationApp, FirstComponent, SecondComponent],
    bootstrap: [PageNavigationApp],
    imports: [
        NativeScriptModule,
        NativeScriptRouterModule,
        NativeScriptRouterModule.forRoot(routes)
    ]
})
export class PageNavigationAppModule { }

platformNativeScriptDynamic().bootstrapModule(PageNavigationAppModule);
```

The main difference here is that when navigating - the new component will be loaded as a root view in a **new Page**.
This means that any content outside the **page-router-outlet** will not be included in the new page. This is the reason why the **page-router-outlet** is usually the single root element in the application component.

Note that we can now use the **Back** button and the **NavigationBar** to navigate.

It is possible to nest **<router-outlet>** component inside **<page-router-outlet>** or another **<router-outlet>**.

### Navigation Options

You can define the trigger in your application declaratively - using the **nsRouterLink** directive in your markup. Or you can do it through code - by injecting the **RouterExtensions** class and using its methods:

```
@Component({
    // ...
})
export class MainComponent {
    constructor(private routerExtensions: RouterExtensions) {
        // ...
    }
}
```

#### Navigating Back

You can navigate back using **back()** method of the **RouterExtensions**

```
public goBack() {
    this.routerExtensions.back();
}
```

You can also navigate back to the previous page with **backToPreviousPage**():

```
public goBackPage() {
    this.routerExtensions.backToPreviousPage();
}
```

The difference between the two methods is visible when there are nested(child) router-outlet(s) inside the current page:

* **back()** - goes back to the previous router location even if the navigation occurred inside the child router outlet on the current page.
* **backToPreviousPage()** - goes back to the previous page. The method skips all child router-outlet navigations inside the current page and goes directly to the previous one.



