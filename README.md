# user_repos

### Description
This web-application takes a github user's name and lists their repositories, which are sorted by their star count. 
Written in Vue.js 3.

## Features 
- Username value is validated using predefined regular expression taken from [this package](https://github.com/shinnn/github-username-regex)
- If the username is not found an error message is displayed
- List of repositories is sorted by their star count from highest to lowest
- Maximum number of repositories listed is: 30
- When repository heading (name or star count) is hovered the stars turn yellow

It consists of two main components: App.vue (main functionality) and ListElement.vue (single repository in the list).

## Project setup
```
npm install
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

