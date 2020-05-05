## 安装 gitbash 下载安装 
## 安装 nodejs  https://nodejs.org/en/ 下载安装
## 安装 VS https://code.visualstudio.com/ 下载安装
## 安装 yarn  https://classic.yarnpkg.com/zh-Hans/docs/install#windows-stable

安装了nodejs后，就会有 npm npx命令

```
[LGF@DESKTOP-FF2MAUH /c/web_react]$ npx create-react-app my-app --template typescript
npx: 98 安装成功，用时 76.195 秒

Creating a new React app in C:\web_react\my-app.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template-typescript...

yarn add v1.22.4
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.12: The platform "win32" is incompatible with this module.
info "fsevents@1.2.12" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.1.2: The platform "win32" is incompatible with this module.
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 13 new dependencies.
info Direct dependencies
├─ cra-template-typescript@1.0.3
├─ react-dom@16.13.1
├─ react-scripts@3.4.1
└─ react@16.13.1
info All dependencies
├─ @babel/plugin-transform-flow-strip-types@7.9.0
├─ @babel/plugin-transform-runtime@7.9.0
├─ @babel/plugin-transform-typescript@7.9.6
├─ @babel/preset-typescript@7.9.0
├─ babel-preset-react-app@9.1.2
├─ cra-template-typescript@1.0.3
├─ eslint-config-react-app@5.2.1
├─ react-dev-utils@10.2.1
├─ react-dom@16.13.1
├─ react-error-overlay@6.0.7
├─ react-scripts@3.4.1
├─ react@16.13.1
└─ scheduler@0.19.1
Done in 139.91s.

Initialized a git repository.

Installing template dependencies using yarnpkg...
yarn add v1.22.4
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@2.1.2: The platform "win32" is incompatible with this module.
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@1.2.12: The platform "win32" is incompatible with this module.
info "fsevents@1.2.12" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
warning " > @testing-library/user-event@7.2.1" has unmet peer dependency "@testing-library/dom@>=5".
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 23 new dependencies.
info Direct dependencies
├─ @testing-library/jest-dom@4.2.4
├─ @testing-library/react@9.5.0
├─ @testing-library/user-event@7.2.1
├─ @types/jest@24.9.1
├─ @types/node@12.12.37
├─ @types/react-dom@16.9.7
├─ @types/react@16.9.34
├─ react-dom@16.13.1
├─ react@16.13.1
└─ typescript@3.7.5
info All dependencies
├─ @babel/runtime-corejs3@7.9.6
├─ @sheerun/mutationobserver-shim@0.3.3
├─ @testing-library/dom@6.16.0
├─ @testing-library/jest-dom@4.2.4
├─ @testing-library/react@9.5.0
├─ @testing-library/user-event@7.2.1
├─ @types/jest@24.9.1
├─ @types/node@12.12.37
├─ @types/prop-types@15.7.3
├─ @types/react-dom@16.9.7
├─ @types/react@16.9.34
├─ @types/testing-library__dom@7.0.2
├─ @types/testing-library__react@9.1.3
├─ css.escape@1.5.1
├─ csstype@2.6.10
├─ dom-accessibility-api@0.3.0
├─ min-indent@1.0.0
├─ react-dom@16.13.1
├─ react@16.13.1
├─ redent@3.0.0
├─ strip-indent@3.0.0
├─ typescript@3.7.5
└─ wait-for-expect@3.0.2
Done in 60.52s.

We detected TypeScript in your project (src\App.test.tsx) and created a tsconfig.json file for you.

Your tsconfig.json has been populated with default values.

Removing template package using yarnpkg...

yarn remove v1.22.4
[1/2] Removing module cra-template-typescript...
[2/2] Regenerating lockfile and installing missing dependencies...
info fsevents@2.1.2: The platform "win32" is incompatible with this module.
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@1.2.12: The platform "win32" is incompatible with this module.
info "fsevents@1.2.12" is an optional dependency and failed compatibility check. Excluding it from installation.
warning " > @testing-library/user-event@7.2.1" has unmet peer dependency "@testing-library/dom@>=5".
success Uninstalled packages.
Done in 5.10s.
Git commit not created Error: Command failed: git commit -m "Initialize project using Create React App"
    at checkExecSyncError (child_process.js:611:11)
    at execSync (child_process.js:647:15)
    at tryGitCommit (C:\web_react\my-app\node_modules\react-scripts\scripts\init.js:62:5)
    at module.exports (C:\web_react\my-app\node_modules\react-scripts\scripts\init.js:334:25)
    at [eval]:3:14
    at Script.runInThisContext (vm.js:131:20)
    at Object.runInThisContext (vm.js:297:38)
    at Object.<anonymous> ([eval]-wrapper:10:26)
    at Module._compile (internal/modules/cjs/loader.js:1176:30)
    at evalScript (internal/process/execution.js:94:25) {
  status: 128,
  signal: null,
  output: [ null, null, null ],
  pid: 1092,
  stdout: null,
  stderr: null
}
Removing .git directory...

Success! Created my-app at C:\web_react\my-app
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd my-app
  yarn start

Happy hacking!
[LGF@DESKTOP-FF2MAUH /c/web_react]$
```



