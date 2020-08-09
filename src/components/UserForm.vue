<template>
  <div class="form" v-if="formIsVisible">
    <button class="mdc-fab inline-demo-fab mdc-ripple-upgraded" v-on:click="closeForm">
      <div class="mdc-fab__ripple"></div>      
      <i aria-hidden="true" class="material-icons mdc-fab__icon">close</i>
    </button>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" ref="name" v-bind:class="{ error: error.name }" v-on:input="cleanInput('name')" type="text" aria-labelledby="name" v-model="name">
      <span class="mdc-floating-label" id="cpf" v-if="!name">Nome Completo</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.name">{{error.name}}</span>
    <label class="mdc-text-field mdc-text-field--filled" v-bind:class="{ disabled: !isNaN(this.user.index) }">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" maxlength="11" ref="cpf" v-bind:class="{ error: error.cpf }" v-on:input="cleanInput('cpf')" type="text" aria-labelledby="cpf" v-model="cpf" :disabled="!isNaN(this.user.index)">
      <span class="mdc-floating-label" id="cpf" v-if="!cpf">CPF</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.cpf">{{error.cpf}}</span>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" ref="email" v-bind:class="{ error: error.email }" v-on:input="cleanInput('email')" type="text" aria-labelledby="email" v-model="email">
      <span class="mdc-floating-label" id="email" v-if="!email">E-mail</span>
      <span class="mdc-line-ripple"></span>
    </label>
    <span class="error-message" v-if="error.email">{{error.email}}</span>
    <label class="mdc-text-field mdc-text-field--filled">
      <span class="mdc-text-field__ripple"></span>
      <input class="mdc-text-field__input" maxlength="11" ref="phone" v-bind:class="{ error: error.phone }" v-on:input="cleanInput('phone')" type="text" aria-labelledby="phone" v-model="phone">
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
        },
        users: []
    }
  },
  mounted() {
    if (localStorage.users)
      this.users = JSON.parse(localStorage.users)
  },
  methods: {
    validate() {      
      const cpfValid = cpf => {
        const numberOfDigits = /\d{11}/
        if (!numberOfDigits.test(cpf)) return false
        
        const anyInvalidSequence = /0{11}|1{11}|2{11}|3{11}|4{11}|5{11}|6{11}|7{11}|8{11}|9{11}/
        if (anyInvalidSequence.test(cpf)) return false

        const calculateDigit = digits => {
          const total = digits.length + 1
          const sumOfMultiplications = digits.reduce((accumulator, current, index) => {
            return accumulator + current * (total - index)
          }, 0)

          const restOfDivision = (sumOfMultiplications * 10) % 11
          const resultDigit = (restOfDivision === 10) ? 0 : restOfDivision

          return resultDigit
        }
        
        const firstDigitIsValid = () => {
          const firstNineDigitsVector = cpf.slice(0,9).split('')
          const firstDigitAfterHyphen = Number(cpf.slice(9,10))
          const firstDigitCalculated = Number(calculateDigit(firstNineDigitsVector))
          const isValid = firstDigitCalculated === firstDigitAfterHyphen

          return isValid
        }

        const secondDigitIsValid = () => {
          const firstTenDigitsVector = cpf.slice(0,10).split('')
          const secondDigitAfterHyphen = Number(cpf.slice(10))
          const secondDigitCalculated = Number(calculateDigit(firstTenDigitsVector))
          const isValid = secondDigitCalculated === secondDigitAfterHyphen

          return isValid
        }

        if(!firstDigitIsValid() || !secondDigitIsValid()) return false

        return true
      }

      const cpfExists = cpf => {
        const exists = this.users.find(user => user.cpf === cpf)
        return Boolean(exists)
      }

      const emailValid = email => {
        const regex = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/
        if(!regex.test(email)) return false
        return true
      }
      
      const emailExists = email => {
        const exists = this.users.find(user => user.email === email)
        return Boolean(exists)
      }

      const phoneValid = phone => {
        const numberOfDigits = /\d{10,11}/
        if (!numberOfDigits.test(phone)) return false
        return true
      }

      const isValidName = () => {
        this.error.name = ''

        if(!this.name)
          this.error.name = 'Nome é obrigatório'
        else if (this.name.length < 3)
          this.error.name = 'Nome com 3 caracteres ou mais!'          

        const isValid = this.error.name === ''
        if(!isValid) this.$refs.name.focus()

        return isValid
      }

      const isValidCPF = () => {
        this.error.cpf = ''
        
        if(!this.cpf)
          this.error.cpf = 'CPF é obrigatório'
        else if (!cpfValid(this.cpf))
          this.error.cpf = 'CPF inválido'
        else if (isNaN(this.user.index) && cpfExists(this.cpf))
          this.error.cpf = 'CPF já cadastrado'     

        const isValid = this.error.cpf === ''
        if(!isValid) this.$refs.cpf.focus()

        return isValid
      }

      const isValidEmail = () => {
        this.error.email = ''

        if(!this.email)
          this.error.email = 'E-mail é obrigatório'
        else if (!emailValid(this.email))
          this.error.email = 'E-mail inválido'
        else if (isNaN(this.user.index) && emailExists(this.email))
          this.error.email = 'E-mail já cadastrado'          

        const isValid = this.error.email === ''
        if(!isValid) this.$refs.email.focus()
        
        return isValid
      }

      const isValidPhone = () => {
        this.error.phone = ''
        
        if(!this.phone)
          this.error.phone = 'Telefone é obrigatório'
        else if (!phoneValid(this.phone)) 
          this.error.phone = 'Telefone inválido'

        const isValid = this.error.phone === ''
        if(!isValid) this.$refs.phone.focus()
        
        return isValid
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
  background-color: var(--color-secondary);
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  align-items: center;
}
.mdc-text-field.disabled {
  background-color: var(--color-primary);
  opacity: 0.3;
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