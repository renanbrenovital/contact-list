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
  </div>
</template>

<script>
export default {
  name: 'UserList',
  data() {
    return {
      users: [
        {
          "cpf": "04080757247",
          "name": "José Andrade",
          "email": "jose@test.com.br",
          "phone": "11987654321"
        },
        {
          "cpf": "77797584192",
          "name": "Manoel Silva",
          "email": "manoel@test.com.br",
          "phone": "11987654321"
        },
        {
          "cpf": "45486737688",
          "name": "Augusto Maier",
          "email": "augusto@test.com.br",
          "phone": "11987654321"
        }
      ]
    }
  },
  methods: {
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    editUser(index) {
      console.log('edit user:', index);
    }
  }
}
</script>

<style scoped>
.mdc-list {
  margin-top: 100px;
}
</style>