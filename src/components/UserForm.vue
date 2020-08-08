<template>
  <div class="form" v-if="formIsVisible">
    <button class="mdc-fab inline-demo-fab mdc-ripple-upgraded" v-on:click="closeForm">
      <div class="mdc-fab__ripple"></div>      
      <i aria-hidden="true" class="material-icons mdc-fab__icon">close</i>
    </button>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" aria-labelledby="name" v-model="name">
      <span class="mdc-floating-label" id="cpf">Nome Completo</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" type="text" ref="cpf" aria-labelledby="cpf" v-model="cpf">
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
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserForm',
  props: ['user', 'formIsVisible'],
  data() {
    return {
        cpf: '',
        name: '',
        email: '',
        phone: '',
    }
  },
  methods: {
    saveUser() {
      if(!this.cpf){
        this.cpf.focus
        console.log('preencha o cpf')
        return
      }      
      
      this.$emit('save-user', {
        index: this.user.index,
        cpf: this.cpf,
        name: this.name,
        email: this.email,
        phone: this.phone
      })
    },
    closeForm() {
      this.$emit('close-form')
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
  position: absolute;
  width: 100vw;
  height: 100vh;
  background: var(--color-background);
  left: 0;
  top: 0;
  z-index: 5;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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
  transition: opacity 0.3s;
}
.mdc-text-field__input:focus ~ .mdc-line-ripple::before {
  border-bottom-color: var(--color-primary) !important;
}
.mdc-text-field__input:focus + .mdc-floating-label {
  opacity: 0;
}
.mdc-button {
  width: 230px;
  height: 50px;
  font-size: 0.9rem;
  background-color: var(--color-primary);
  box-shadow: 0px 4px 0px var(--color-secondary);
  color: var(--color-text);
  text-transform: uppercase;
  font-weight: 600;
  border: none;
  outline: 0;
}
.mdc-fab {
  background: var(--color-secondary);
  color: var(--color-text);
  position: absolute;
  right: 1rem;
  top: 1rem;
}
</style>