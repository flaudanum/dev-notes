<h1>Getting started with an Angular project</h1>

- [Project's setup](#projects-setup)
- [Routing](#routing)
- [Adding new elements with Anfular CLI](#adding-new-elements-with-anfular-cli)
  - [New Component](#new-component)

## Project's setup

In an admin shell, update `npm` and install/update Angular CLI:

```
npm install -g npm@latest
npm install -g @angular/cli@latest
```

Now change to the project's parent directory and initialize a new _Angular workspace_:

```
> ng new angular-sandbox
? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace?
  This setting helps improve maintainability and catch bugs ahead of time.
  For more information, see https://angular.io/strict No
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
CREATE angular-sandbox/angular.json (3607 bytes)
...
CREATE angular-sandbox/e2e/src/app.po.ts (274 bytes)
√ Packages installed successfully.
    Successfully initialized git.
```

This operation creates the Angular workspace in a directory `angular-sandbox` which also contains a new NPM project. A _Hello World_ Angular app is now available and can be served for development purposes:

```
ng serve --open
```

## Routing

Angular's route management system requires the package `@angular/router`.

Create a `routes.ts` file which specifies and exports a `Routes` object:

```typescript
import { Routes } from '@angular/router';

export const appRoutes: Routes = [
// Define routes here
];
```

In file `app.module.ts` add `RouterModule.forRoot(appRoutes)` to imports:

```typescript
import { RouterModule } from '@angular/router';
import { appRoutes } from './app.routes';

@NgModule({
  ...
  imports: [
    BrowserModule,
    ...
    RouterModule.forRoot(appRoutes)
  ],
  ...
})
export class AppModule { }
```

Alternatively, import a module `AppRoutingModule` containing routes.


## Adding new elements with Anfular CLI

New components can be added with the command `ng generate <element type>`. Type `ng generate -h` for further details.

**Beware**: there are NO COMMANDS for undoing these operations

### New Component

For instance, this command adds a new _component_ `dummy`:

```
> ng generate component dummy
CREATE src/app/dummy/dummy.component.html (20 bytes)
CREATE src/app/dummy/dummy.component.spec.ts (619 bytes)
CREATE src/app/dummy/dummy.component.ts (271 bytes)
CREATE src/app/dummy/dummy.component.css (0 bytes)
UPDATE src/app/app.module.ts (632 bytes)
```

This creates the directory `src/app/dummy` with the following content:

```
dummy.component.css
dummy.component.html
dummy.component.spec.ts
dummy.component.ts
```

and imports the component in the main module `app.module.ts`:

```typescript
import { DummyComponent } from './dummy/dummy.component';

@NgModule({
  declarations: [AppComponent, DummyComponent],
  ...
})
export class AppModule {}
```