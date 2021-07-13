<style>
    h1, h2, h3 {
        color: rgb(0,0,150);
        background-color: rgb(230,230,230);
    }
</style>

# Webpack

- [Webpack](#webpack)
  - [Basic setup](#basic-setup)
  - [Webpack CLI](#webpack-cli)
    - [Initialize a new configuration script](#initialize-a-new-configuration-script)
    - [Build with stats data](#build-with-stats-data)
  - [Other webpack tools](#other-webpack-tools)
    - [Analyze a bundle](#analyze-a-bundle)

## Basic setup

Excerpt from [Bhargav Bachina](https://medium.com/@bhargavbachina?source=follow_footer--------------------------follow_footer-)'s
[post in Medium](https://medium.com/bb-tutorials-and-thoughts/how-to-write-production-ready-node-js-rest-api-javascript-version-db64d3941106).

Once development is complete, we need to bundle our app and deploy into other
environments like stage,uat and prod etc. Let’s add webpack to our project to
bundle the entire app files into a single file.

Webpack's setup requires to run some `npm install` commands:

Global install
```
$ npm install -g webpack
```

Project installs as dev dependencies
```
$ npm install webpack webpack-cli --save-dev
```

Add this line in the *scripts* section in file `package.json`:
```json
"build": "webpack"
```

Add the `webpack.config.js` where you mention starting file and output
directory and the target is important because our runtime environment is node.
```javascript
const path = require('path');

module.exports = {
  entry: './index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'api.bundle.js'
  },
  target: 'node'
};
```

When running the command below, webpack will build the entire project into
single bundle file and place that in the target folder mentioned in the
`webpack.config.js`:

```
npm run build
```

After completion of the build, let's add the following `npm` script to the
*scripts* section in the `package.json`:

```json
"prod": "node dist/api.bundle.js"
```

and run the command:

```npm
$ npm run prod
```

## Webpack CLI

### Initialize a new configuration script

<b style="color:red">BUG:</b> This functionality is currently bugged.
See [issue tracker on GitHub](https://github.com/webpack/webpack-cli/issues/1127)

Use `webpack-cli init` and answer the questions. If webpack is installed as a project dependency:
```
$ npx webpack-cli init
```

### Build with stats data
```
$ npx webpack-cli --profile --json > compilation-stats.json
```

## Other webpack tools

### Analyze a bundle

```
$ npm install webpack-bundle-analyzer
```
Use the script for analyzing the dependencies packed in the bundle.
```
$ npx webpack-bundle-analyzer compilation-stats.json ./dist
```
A web page with an awesome dependency graph opens.
