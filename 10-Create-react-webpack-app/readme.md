React + Webpack 4 + Babel 7 Setup Tutorial
===

Project configuration
```sh
mkdir minimal-react-boilerplate
cd minimal-react-boilerplate
```

Initilize `package.json`
```sh
npm init -y
```

Configure attributes
```sh
npm config list

npm set init.author.name "<Your Name>"
npm set init.author.email "you@example.com"
npm set init.author.url "example.com"
npm set init.license "MIT"
```

Create a distribution folder
```sh
mkdir dist
cd dist
touch index.html
```

The created file should have the following content:
```html
<!DOCTYPE html>
<html>
  <head>
    <title>The Minimal React Webpack Babel Setup</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="./bundle.js"></script>
  </body>
</html>
```


Reference
===
[1. minimal-react-webpack-babel-setup](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/)