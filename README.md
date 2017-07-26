# Angular Routing inside NativeScript app



In an Angular application navigation is done using the *Angular Component Router*.


#### Configuration

The router configuration usually consists of the following steps:

* Create a RouterConfig object which maps paths to components and parameters:

  `export const routes = [
       { path: "login", component: LoginComponent },
       { path: "groceries", component: GroceryListComponent },
       { path: "grocery/:id", component: GroceryComponent }
   ];`