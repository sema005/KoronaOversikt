# Korona app
Dette er en simpel app som bruker et <a href="https://coronavirus-tracker-api.herokuapp.com/v2/locations?timelines=1">korona api </a>
It uses the <a href="https://svelte-native.technology/docs">Svelte-native</a> framework to build a native application. Svelte-native is build on top of <a href="https://nativescript.org"></a>, so you need to go through the setup there in order to install the TNS CLI tools

## Set up
```html
npm install
ths run [ios|android]
```

## Project structure
This is a single page application. Everything is handled in app/App.svelte. This file...
- Fetches data from firestore in the svelte onMount function
- Uses a  <a href="https://svelte-native.technology/docs#scrollview">scrollViews</a> to display a list of items
- You are not able to do anything further whit the items - i.e delete, update and add - so example just serves as a demo of how you can include firestore data in a simple app