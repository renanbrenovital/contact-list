<template>
  <div>
    <header class="mdc-top-app-bar mdc-top-app-bar--short">
      <div class="mdc-top-app-bar__row">
        <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-start">
          <span class="mdc-top-app-bar__title">Contatos</span>
        </section>
      </div>
    </header>
    <ul class="mdc-list inline-demo-list mdc-list--avatar-list">
      <li class="mdc-list-item mdc-ripple-upgraded" v-if="users.length <= 0">Nenhum contado cadastrado</li>
      <li class="mdc-list-item mdc-ripple-upgraded" v-for="(user, index) in users" v-bind:key="user.cpf">
        <span class="material-icons mdc-card__action mdc-card__action--icon">
          person
        </span>      
        <span class="mdc-list-item__text">
          {{user.name}}
        </span>
        <span aria-hidden="true" class="mdc-list-item__meta">
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Editar Usuário" v-on:click="editUser(index)">edit</button>
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Deletar Usuário" v-on:click="deleteUser(index)">delete</button>
        </span>
      </li>
    </ul>
    <div class="form">
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="cpf" v-model="form.cpf">
        <span class="mdc-floating-label" id="cpf">CPF</span>
        <span class="mdc-line-ripple"></span>
      </label>
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="name" v-model="form.name">
        <span class="mdc-floating-label" id="cpf">Nome Completo</span>
        <span class="mdc-line-ripple"></span>
      </label>
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="email" v-model="form.email">
        <span class="mdc-floating-label" id="email">E-mail</span>
        <span class="mdc-line-ripple"></span>
      </label>
      <label class="mdc-text-field mdc-text-field--filled">
        <span class="mdc-text-field__ripple"></span>
        <input class="mdc-text-field__input" type="text" aria-labelledby="phone" v-model="form.phone">
        <span class="mdc-floating-label" id="phone">Telefone</span>
        <span class="mdc-line-ripple"></span>
      </label>
      <div class="mdc-touch-target-wrapper">
        <button class="mdc-button mdc-button--touch" v-on:click="createUser">
          <div class="mdc-button__ripple"></div>
          <span class="mdc-button__label">Cadastrar Contato</span>
          <div class="mdc-button__touch"></div>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserList',
  data() {
    return {
      form: {
        cpf: '',
        name: '',
        email: '',
        phone: ''
      },
      users: []
    }
  },
  mounted() {
    this.getUsers()
  },
  methods: {
    getUsers() {
      if (localStorage.users) {
        this.users = JSON.parse(localStorage.users)
      }
      else 
      {
        fetch('https://api.mocki.io/v1/a2790e8c')
          .then(response => response.json())
          .then(data => this.users = data)
          .catch(error => console.error(error))
      } 
    },
    createUser() {
      this.users.push({
        cpf: this.form.cpf,
        name: this.form.name,
        email: this.form.email,
        phone: this.form.phone
      });
    },
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    editUser(index) {
      console.log('edit user:', index);
    }
  },
  watch: {
    users() {
      localStorage.users = JSON.stringify(this.users)
    }
  },
}
</script>

<style scoped>
.mdc-list {
  margin-top: 100px;
}
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