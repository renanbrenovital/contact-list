<template>
  <header class="mdc-top-app-bar mdc-top-app-bar--short">
    <div class="mdc-top-app-bar__row">
      <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-start">
        <span class="mdc-top-app-bar__title">{{title}}</span>
      </section>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: ['title']
}
</script>