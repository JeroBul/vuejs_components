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

Example code for the app.vue file
```
<template>
  <h1>Base project with pico : {{ testConstante }}</h1>
</template>

<script setup>
const testConstante = 'Test'
</script>

```