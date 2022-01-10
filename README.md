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

It consists of two main components: App.vue (main functionality) and ListElement.vue (single repository in the list). Splitting App.vue into more components would be a possible improvement.

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

## Notes for recruiters:
On Sunday noon I had the whole project done and ready (even despite being sick with high fever for 3 days), I was getting 30 repositories in the list, but then I thought to myself "what if this is the tricky part? What if they want ALL of the repositories" and that changed everything. 
I worked really hard on trying to figure out how pagination works and api headers, some stuff started to work, but there was still the problem of asynchronous communication. My lack of full understanding of meant, that my loop was only accessing the first page (because of "hasNext" changing value during fetch, so the value was coming to me too late).
Perhaps if I was using pure javascript I could have made it work, but since I'm not a pro at vue3 yet, I couldn't do it. Maybe with a feature called Suspense, but it's still being tested.