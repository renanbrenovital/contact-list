<template>
  <div>
    <h1>Usuários</h1>
    <ul class="mdc-list">
      <li class="mdc-list-item" tabindex="0" v-for="user in users" v-bind:key="user.cpf">
        <span class="mdc-list-item__ripple"></span>
        <span class="mdc-list-item__text">{{user.name}}</span>
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
  }
}
</script>

<style scoped>
</style>