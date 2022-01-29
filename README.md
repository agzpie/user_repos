# user_repos

### Description
This web-application takes a github user's name and lists their repositories, which are sorted by their star count. 
Written in Vue.js 3.

## Features 
- Username value is validated using predefined regular expression taken from [this package](https://github.com/shinnn/github-username-regex)
- If the username is not found an error message is displayed
- List of repositories is sorted by their star count from highest to lowest
- Lists all of the repositories thanks to an asynchronous function
- When repository heading (name or star count) is hovered the stars turn yellow

It consists of two main components: App.vue (main functionality) and ListElement.vue (single repository in the list). Splitting App.vue into more components would be a possible improvement.

## Project setup
```
npm install
```

### Newer versions of Node.js (17+) may require additional enabling of legacy OpenSSL support
See: https://github.com/webpack/webpack/issues/14532
```
export NODE_OPTIONS=--openssl-legacy-provider
```

### Debugger support
See: https://v3.vuejs.org/cookbook/debugging-in-vscode.html

See: https://stackoverflow.com/a/67641777/3917133

launch.json:
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "vuejs: chrome",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}/src",
            "breakOnLoad": true,
            "sourceMapPathOverrides": {
                "webpack:///./src/*": "${webRoot}/*",
                "webpack:///src/*": "${webRoot}/*"
            }
        },
        {
            "type": "firefox",
            "request": "launch",
            "name": "vuejs: firefox",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}/src",
            "pathMappings": [
                {
                    "url": "webpack:///./src/",
                    "path": "${webRoot}/"
                }
            ]
        }
    ]
}
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```