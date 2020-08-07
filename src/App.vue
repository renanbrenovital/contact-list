<template>
  <div id="app">
    <UserList />
  </div>
</template>

<script>
import UserList from './components/UserList.vue'

export default {
  name: 'App',
  components: {
    UserList
  }
}
</script>

<style>
:root {
  --color-background: #121212;
  --color-dark: #111111;
  --color-text: #E1E1E1;
  --color-primary: #7A4CE5;
  --color-secondary: #202024;
  --color-alert: #fd951f;
  --color-success: #77D562;
  --color-error: #ce4a4a;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: var(--color-background);
}
</style>
