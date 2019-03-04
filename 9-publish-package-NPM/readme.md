Creating Node JS module
===

Create a `package.json` file
---
`npm init`

provide `name` and `version` of package:
- **name:** Name of module
- **version:** The initial module version
- **main:** The name of the file that will be loaded when module is required by another application. The default name is `index.js`

Semantic Versioning
---
helps others to understand changes in a given version

## Incrementing semantic versions in published packages
| Code status	| Code status	| Rule               | Example Version |
| ------------- |:-------------:| :-----------------:| :--------------: |
| First release | New product   | Start with 1.0.0   | 1.0.0
| Backward compatible bug fixes | Patch release | Increment the third digit | 1.0.1
| Backward compatible new features | Minor release | Increment the middle digit and reset last digit to zero | 1.1.0
| Changes that break backward compatibility | Major release | Increment the first digit and reset middle and last digits to zero | 2.0.0

Example: acceptable version ranges up to 1.0.4

- Patch releases: 1.0 or 1.0.x or ~1.0.4
- Minor releases: 1 or 1.x or ^1.0.4
- Major releases: * or x


Create a file that will be loaded when your module is required by another application.
---
The default name for this file is index.js
```javascript
exports.printMsg = function() {
  console.log("This is a message from the demo package");
}
```

Test a module
---
Create dir and install package in it. <br/>
```npm install <your-module-name>``` <br/>

then run: <br/>
```node test.js```



References:
====
[1. Creating-node-js-modules](https://docs.npmjs.com/creating-node-js-modules) <br/>
[2. Semantic Versioning 2.0.0](https://semver.org/) <br/>
[3. Publish a package](https://gist.github.com/arjun-kava/f3899d8bd28e5791e4ef7aca9c7dc35f) <br/>
[4. Example Package](https://github.com/zujoio-archieved/yolonode-js) <br/>
[5. Guidelines](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)
