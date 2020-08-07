<template>
  <header class="mdc-top-app-bar mdc-top-app-bar--short">
    <div class="mdc-top-app-bar__row">
      <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-start">
        <span class="mdc-top-app-bar__title">{{title.header}}</span>
      </section>
      <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-end">
        <button class="mdc-button mdc-button--raised">
          <div class="mdc-button__ripple"></div>
          <i class="material-icons mdc-button__icon" aria-hidden="true">add</i>
          <span class="mdc-button__label">{{title.button}}</span>
        </button>
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

<style scoped>
.mdc-top-app-bar {
  background: var(--color-primary);
  color: var(--color-text);
  position: relative;
}
.mdc-top-app-bar__title {
  font-weight: bold;
}
.mdc-button--raised {
  background: var(--color-secondary);
  color: var(--color-text);
}
</style>