<template>
  <div class="form" v-if="formIsVisible">
    <button class="mdc-fab inline-demo-fab mdc-ripple-upgraded" v-on:click="closeForm">
      <div class="mdc-fab__ripple"></div>      
      <i aria-hidden="true" class="material-icons mdc-fab__icon">close</i>
    </button>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" v-bind:class="{ error: error.name }" v-on:input="cleanInput('name')" type="text" aria-labelledby="name" v-model="name">
      <span class="mdc-floating-label" id="cpf" v-if="!name">Nome Completo</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.name">{{error.name}}</span>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" v-bind:class="{ error: error.cpf }" v-on:input="cleanInput('cpf')" type="text" aria-labelledby="cpf" v-model="cpf">
      <span class="mdc-floating-label" id="cpf" v-if="!cpf">CPF</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.cpf">{{error.cpf}}</span>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" v-bind:class="{ error: error.email }" v-on:input="cleanInput('email')" type="text" aria-labelledby="email" v-model="email">
      <span class="mdc-floating-label" id="email" v-if="!email">E-mail</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.email">{{error.email}}</span>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" v-bind:class="{ error: error.phone }" v-on:input="cleanInput('phone')" type="text" aria-labelledby="phone" v-model="phone">
      <span class="mdc-floating-label" id="phone" v-if="!phone">Telefone</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.phone">{{error.phone}}</span>
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
        name: '',
        cpf: '',
        email: '',
        phone: '',
        error: {
          name: '',
          cpf: '',
          email: '',
          phone: '',
        }
    }
  },
  methods: {
    validate() {
      const nameValid = name => {
        if(typeof name !== "string") return false
        return true
      }

      const cpfValid = cpf => {
        if (typeof cpf !== "string") return false
        if (cpf.length !== 11) return false
        return true
      }

      const emailValid = email => {
        if (typeof email !== "string") return false
        return true
      }

      const phoneValid = phone => {
        if (typeof phone !== "string") return false
        return true
      }

      const isValidName = () => {
        if(!this.name)
          this.error.name = 'Nome é obrigatório'
        else if (!nameValid(this.name))
          this.error.name = 'Nome inválido'
        else
          this.error.name = ''

        return this.error.name === ''
      }

      const isValidCPF = () => {
        if(!this.cpf)
          this.error.cpf = 'CPF é obrigatório'
        else if (!cpfValid(this.cpf))
          this.error.cpf = 'CPF inválido'
        else
          this.error.cpf = ''

        const isValid = this.error.cpf === ''

        return isValid
      }

      const isValidEmail = () => {
        if(!this.email)
          this.error.email = 'E-mail é obrigatório'
        else if (!emailValid(this.email))
          this.error.email = 'E-mail inválido'
        else
          this.error.email = ''
        
        return this.error.email === ''
      }

      const isValidPhone = () => {
        if(!this.phone)
          this.error.phone = 'Telefone é obrigatório'
        else if (!phoneValid(this.phone)) 
          this.error.phone = 'Telefone inválido'
        else
          this.error.phone = ''
        return this.error.phone === ''
      }

      return { 
        isValidName,
        isValidCPF,
        isValidEmail,
        isValidPhone
      }
    },    
    saveUser() {
      const { isValidName, isValidCPF, isValidEmail, isValidPhone } = this.validate();    
      if(isValidName() && isValidCPF() && isValidEmail() && isValidPhone()) {
        this.$emit('save-user', {
          index: this.user.index,
          cpf: this.cpf,
          name: this.name,
          email: this.email,
          phone: this.phone
        })
      } 
    },
    cleanInput(input) {
      this.error[input] = ''
    },
    cleanForm() {      
      this.error = {
        name: '',
        cpf: '',
        email: '',
        phone: ''
      }
    },
    closeForm() {
      this.cleanForm()
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
.error-message {
  color: var(--color-error);
  margin-bottom: 0.5rem;
}
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
.mdc-text-field__input.error ~ .mdc-line-ripple::before,
.mdc-text-field__input.error:focus ~ .mdc-line-ripple::before {
  border-bottom-color: var(--color-error) !important;
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