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
    <UserForm v-on:create-user="createUser" />
  </div>
</template>

<script>
import UserForm from './UserForm.vue';

export default {
  name: 'UserList',
  components: {
    UserForm
  },
  data() {
    return {
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
    createUser(user) {
      this.users.push(user);
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
.fab-add {
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