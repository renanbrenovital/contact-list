<template>
  <div class="form">
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" aria-labelledby="name" v-model="name">
      <span class="mdc-floating-label" id="cpf">Nome Completo</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" aria-labelledby="cpf" v-model="cpf">
      <span class="mdc-floating-label" id="cpf">CPF</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" aria-labelledby="email" v-model="email">
      <span class="mdc-floating-label" id="email">E-mail</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" aria-labelledby="phone" v-model="phone">
      <span class="mdc-floating-label" id="phone">Telefone</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <div class="mdc-touch-target-wrapper">
      <button class="mdc-button mdc-button--touch" v-on:click="saveUser">
        <div class="mdc-button__ripple"></div>
        <span class="mdc-button__label">
          SALVAR
        </span>
        <div class="mdc-button__touch"></div>
      </button>
      <button class="cancel mdc-button mdc-button--touch" v-on:click="cancel">
        <div class="mdc-button__ripple"></div>
        <span class="mdc-button__label">
          CANCELAR
        </span>
        <div class="mdc-button__touch"></div>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserForm',
  props: ['user'],
  data() {
    return {
        cpf: '',
        name: '',
        email: '',
        phone: ''
    }
  },
  methods: {
    saveUser() {
      this.$emit('save-user', {
        cpf: this.cpf,
        name: this.name,
        email: this.email,
        phone: this.phone
      })
    },
    cancel() {
      console.log('limpar form e fechar');
    }
  },
  watch: {
    user() {
      this.cpf = this.user.cpf
      this.name = this.user.name
      this.email = this.user.email
      this.phone = this.user.phone
    }
  }
}
</script>

<style scoped>
.form {
  width: 100vw;
  height: 100vh;
  left: 0;
  top: 0;
  z-index: 10;
}
.mdc-text-field {
  background-color: #202024;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  align-items: center;
}
.mdc-text-field__input,
.mdc-floating-label {
  color: var(--color-text) !important;
  caret-color: var(--color-text);
  transition: opacity 0.5s;
}
.mdc-text-field__input:focus ~ .mdc-line-ripple::before {
  border-bottom-color: var(--color-primary) !important;
}
.mdc-text-field__input:focus + .mdc-floating-label {
  opacity: 0;
}
.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.mdc-button {
  width: 100px;
  font-size: 0.9rem;
  background-color: var(--color-primary);
  box-shadow: 0px 4px 0px var(--color-secondary);
  color: var(--color-text);
  text-transform: uppercase;
  font-weight: 600;
  border: none;
  outline: 0;
}
.mdc-button.cancel {
  background-color: var(--color-dark);
  margin-left: 1rem;
}
</style>