<template>
  <div>
    <Header v-bind:title="title" />
    <UserForm v-on:save-user="saveUser" v-bind:user="user" />
    <ul class="mdc-list inline-demo-list mdc-list--avatar-list">
      <li class="mdc-list-item mdc-ripple-upgraded" v-if="users.length <= 0">
        <span class="mdc-list-item__text no-register">
          Nenhum contado cadastrado
        </span>
      </li>
      <li class="mdc-list-item mdc-ripple-upgraded" v-for="(user, index) in users" v-bind:key="user.cpf">
        <span class="material-icons mdc-card__action mdc-card__action--icon">
          person
        </span>      
        <span class="mdc-list-item__text">
          {{user.name}}
        </span>
        <span aria-hidden="true" class="mdc-list-item__meta">
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Editar Usuário" v-on:click="editUser(user)">edit</button>
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Deletar Usuário" v-on:click="deleteUser(index)">delete</button>
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
import Header from './Header.vue'
import UserForm from './UserForm.vue'

export default {
  name: 'UserList',
  components: {
    Header,
    UserForm
  },
  data() {
    return {
      title: {
        header: 'Contatos',
        button: 'Novo'
      },
      users: [],
      user: {}
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
    editUser(userData) {
      this.user = userData
    },
    saveUser(userData) {
      this.users.push(userData);
    },
    deleteUser(index) {
      this.users.splice(index, 1)
    },
  },
  watch: {
    users() {
      localStorage.users = JSON.stringify(this.users)
      this.user = {}
    }
  },
}
</script>

<style scoped>
.mdc-list {
  max-width: 600px;
  margin: 2rem auto;
}
.mdc-list-item__text, .material-icons {
  color: var(--color-text);
}
.no-register {
  margin: 0 auto;
}
</style>