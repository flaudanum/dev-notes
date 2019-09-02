# Angular CLI notes

Angular is named after the *angular brackets* of HTML. The command **ng** very likely stands from *aNGular*.

- [Angular CLI notes](#Angular-CLI-notes)
  - [Installation of the Angular CLI](#Installation-of-the-Angular-CLI)
  - [Create a new application project](#Create-a-new-application-project)
  - [Run the application on local server in *dev mode*](#Run-the-application-on-local-server-in-dev-mode)
  - [Run the application on local server in production](#Run-the-application-on-local-server-in-production)
  - [Generate a new component in the application](#Generate-a-new-component-in-the-application)
  - [Generate a new service in the application](#Generate-a-new-service-in-the-application)


## Installation of the Angular CLI

First install the *Node Package Manager* [**npm**](https://www.npmjs.com/) then install the latest version of the **Angular CLI**:

```
> npm install -g @angular/cli@latest
```

## Create a new application project

For creating a new *Angular project* called `my-new-project`:

```bash
> ng new my-new-project
```

For creating  a new project with SCSS style sheets and without unit test templates:

```bash
> ng new mon-projet-angular --style=scss --skip-tests=true
```

## Run the application on local server in *dev mode*

Change to the directory of the project and open the application in the default web browser with the command:

```bash
> ng serve --open
```

The default port where the application is served is the port 4200. Another port can be specified with the option `--port`. This is useful when running several application at the same time. The option `--open` automatically opens a new tab at the URL of the local port where the application is served.

*Remark:* depending on the version of the *Angular CLI* or *Node.js* option ` --poll=1000` may be required for auto-reload.

## Run the application on local server in production

First compile the application for production from the application top directory:

```bash
> ng build --prod
```

Install the package *http-server*:

```bash
> npm install http-server -g
```

Change to the *distribution directory* and run HTTP server:

```bash
> cd dist/
> http-server
```

## Generate a new component in the application

From the app's main path, a component called `my-new-comp` is created as follows:

```bash
> ng generate component my-new-comp
> ng g c my-new-comp
```

This command makes two modifications in directory `src/`. First a new folder with the new component's name is created in `src/app/`.
Second, file `src/app/app.module.ts` is updated with:

* an importation of the component:
  ```typescript
  import { AuthComponent } from './my-new-comp/my-new-comp.component';
  ```
* the attribute `import` of the argument of the decorator `@NgModule` is updated with the class identifier `MyNewComponent`

## Generate a new service in the application

Use command `ng generate service` or more shortly `ng g s`. Type the following command for creating the service *my-serv-name* in the folder *app/services/* which is created if necessary:

```bash
> ng generate service services/my-serv-name
```

Files *services/my-serv-name.service.ts* and *services/my-serv-name.service.spec.ts* are created, the second one being a test file. File *my-serv-name.service.ts* provides the declaration of the service class **MyServNameService** with the decorator [`@Injectable`](https://angular.io/api/core/Injectable). This class must be manually referenced in file *app.module.ts* in order to be injectable in the whole module **app.module**, that is the whole application (field [providers](https://angular.io/api/core/NgModule#providers) of the decorator `@NgModule`):

```typescript
import { MyServNameService } from './services/my-serv-name.service';

@(Coding and setting notes)NgModule({
  ...
  providers: [
    MyServNameService,
  ],
  ...
})
```