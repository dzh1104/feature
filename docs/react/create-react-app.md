## Quick Overview

~~~
npm install -g create-react-app

create-react-app my-app
cd my-app/
npm start
~~~

## npm start
>Starts the development server.

## npm run build
>Bundles the app into static files for production.

## npm test
>Starts the test runner.

## npm run eject
>Removes this tool and copies build dependencies, configuration files and scripts into the app directory. If you do this, you can’t go back!

## npm run build

>You may serve it with a static server

npm install -g serve

serve -s build

## Debugging in the Editor

>You would need to have the latest version of `VS Code` and VS Code `Chrome Debugger Extension` installed.

>Then add the block below to your `launch.json` file and put it inside the `.vscode` folder in your app’s root directory.

~~~json
{
  "version": "0.2.0",
  "configurations": [{
    "name": "Chrome",
    "type": "chrome",
    "request": "launch",
    "url": "http://localhost:3000",
    "webRoot": "${workspaceRoot}/src",
    "sourceMapPathOverrides": {
      "webpack:///src/*": "${webRoot}/*"
    }
  }]
}
~~~

>Note: the URL may be different if you've made adjustments via the `HOST` or `PORT` environment variables.

**use :**Start your app by running npm start, and start debugging in VS Code by pressing F5 or by clicking the green debug icon. You can now write code, set breakpoints, make changes to the code, and debug your newly modified code—all from your editor.

## Syntax Highlighting in the Editor

You would need to install an `ESLint` plugin for your editor first. Then, add a file called `.eslintrc` to the project root

~~~
{
  "extends": "react-app"
}
~~~

## Formatting Code Automatically

>[`Prettier`](https://prettier.io/docs/en/editors.html) is an opinionated code formatter with support for JavaScript, CSS and JSON.

>[Editor Integration](https://prettier.io/docs/en/editors.html)

With Prettier you can format the code you write automatically to ensure a code style within your project. See the [Prettier's GitHub page](https://github.com/prettier/prettier) for more information, and look at this [page to see it in action](https://prettier.io/).

>To format our code whenever we make a commit in git, we need to install the following dependencies

~~~
npm install husky lint-staged prettier -D
~~~

>Now we can make sure every file is formatted correctly by adding a few lines to the `package.json` in the project root.

~~~
  "scripts": {
+   "precommit": "lint-staged",
    "start": "react-scripts start",
    "build": "react-scripts build",
~~~

>Next we add a 'lint-staged' field to the package.json, for example

~~~
  "dependencies": {
    // ...
  },
+ "lint-staged": {
+   "src/**/*.{js,jsx,json,css}": [
+     "prettier --single-quote --write",
+     "git add"
+   ]
+ },
  "scripts": {
~~~

## Proxying API Requests in Development

To tell the development server to proxy any unknown requests to your API server in development, add a proxy `field` to your `package.json`

The proxy option supports HTTP, HTTPS and WebSocket connections.If the proxy option is not flexible enough for you, We can [Configure the proxy yourself](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#configuring-the-proxy-manually)

~~~json
"proxy": {
  "/dev": {
    "target": "http://localhost:3000",
    "ws": true
  }
}
~~~

