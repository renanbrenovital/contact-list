<template>
  <div>
    <div class="form">
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="cpf" v-model="cpf">
        <span class="mdc-floating-label" id="cpf">CPF</span>
        <span class="mdc-line-ripple"></span>
      </label>
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="name" v-model="name">
        <span class="mdc-floating-label" id="cpf">Nome Completo</span>
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
        <button class="mdc-button mdc-button--touch" v-on:click="createUser">
          <div class="mdc-button__ripple"></div>
          <span class="mdc-button__label">Adicionar Contato</span>
          <div class="mdc-button__touch"></div>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserForm',
  data() {
    return {
      cpf: '',
      name: '',
      email: '',
      phone: ''
    }
  },
  methods: {
    createUser() {
      this.$emit('create-user', {
        cpf: this.cpf,
        name: this.name,
        email: this.email,
        phone: this.phone
      });
    },
  },
}
</script>

<style scoped>
.form {
  height: 50vh;
  width: 50vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
label {
  margin-bottom: 10px;
}
</style>