<h1>Integration of CSS frameworks with Angular</h1>

## Angular Material

Install Angular material with the `ng add` command.

**Beware**: there is no command for undoing this operation.

```
> ng add @angular/material
Installing packages for tooling via npm.
Installed packages for tooling via npm.
? Choose a prebuilt theme name, or "custom" for a custom theme: Deep Purple/Amber  [ Preview: https://material.angular.io?theme=deeppurple-amber ]
? Set up global Angular Material typography styles? No
? Set up browser animations for Angular Material? Yes
UPDATE package.json (1271 bytes)
√ Packages installed successfully.
UPDATE src/app/app.module.ts (629 bytes)
UPDATE angular.json (3781 bytes)
UPDATE src/index.html (559 bytes)
UPDATE src/styles.css (181 bytes)
```

## Materialize-CSS

With `npm` install both `materialize-css` and JQuery which is required by the JavaScript code embedded in Materialize:

```
npm install materialize-css jquery --save
```

Edit Angular config file `angular.json` and add in:

- _projects> [project's name]> architect> build> options> styles_,
- and _projects> [project's name]> architect> build> options> scripts_

```JSON
{
    ...
    "styles": [
        "node_modules/materialize-css/dist/css/materialize.css"
    ],
    "scripts": [
        "node_modules/jquery/dist/jquery.js",
        "node_modules/materialize-css/dist/js/materialize.min.js"
    ]
    ...
}
```
