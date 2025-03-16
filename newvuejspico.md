# How to create a new vuejs project with pico css
```
npm create vue@latest
npm install

cd $new-project-directory/src

rm assets
rm components

npm install @picocss/pico

```

- remove import with reference to the assets directory
- add import in main.js : import '@picocss/pico/css/pico.min.css'
- add the container class to the div app tag in index.html
```html index.html
  <body>
    <div id="app" class="container"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
```

- Update app.vue file
```html
<template>
  <h1>Base project with pico : {{ testConstante }}</h1>
</template>

<script setup>
const testConstante = 'Test'
</script>

```