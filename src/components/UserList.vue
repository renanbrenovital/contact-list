<template>
  <div>
    <header class="mdc-top-app-bar mdc-top-app-bar--short">
      <div class="mdc-top-app-bar__row">
        <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-start">
          <span class="mdc-top-app-bar__title">Contatos</span>
        </section>
        <section class="mdc-top-app-bar__section mdc-top-app-bar__section--align-end">
          <button class="mdc-button mdc-button--raised" v-on:click="openForm">
            <div class="mdc-button__ripple"></div>
            <i class="material-icons mdc-button__icon" aria-hidden="true">add</i>
            <span class="mdc-button__label">Novo</span>
          </button>
        </section>
      </div>
    </header>
    <div role="progressbar" class="mdc-linear-progress mdc-linear-progress--indeterminate" v-bind:class="{'mdc-linear-progress--closed': !loading }" aria-label="Loading...">
      <div class="mdc-linear-progress__buffer">
        <div class="mdc-linear-progress__buffer-bar"></div>
        <div class="mdc-linear-progress__buffer-dots"></div>
      </div>
      <div class="mdc-linear-progress__bar mdc-linear-progress__primary-bar">
        <span class="mdc-linear-progress__bar-inner"></span>
      </div>
      <div class="mdc-linear-progress__bar mdc-linear-progress__secondary-bar">
        <span class="mdc-linear-progress__bar-inner"></span>
      </div>
    </div>
    <ul class="mdc-list inline-demo-list mdc-list--avatar-list">
      <li class="mdc-list-item mdc-ripple-upgraded" v-if="!loading && users.length <= 0">
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
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Editar Usuário" v-on:click="editUser(index, user)">edit</button>
          <button class="material-icons mdc-icon-button mdc-card__action mdc-card__action--icon" title="Deletar Usuário" v-on:click="deleteUser(index)">delete</button>
        </span>
      </li>
    </ul>
    <UserForm
      v-on:save-user="saveUser"
      v-on:close-form="closeForm"
      v-bind:user="user"
      v-bind:formIsVisible="formIsVisible" 
    />
  </div>
</template>

<script>
import UserForm from './UserForm.vue'

export default {
  name: 'UserList',
  components: {
    UserForm
  },
  data() {
    return {
      users: [],
      user: {},
      formIsVisible: false,
      loading: false
    }
  },
  mounted() {
    this.getUsers()
  },
  methods: {
    async getUsers() {
      try {
        this.loading = true
        
        if (localStorage.users)
          this.users = JSON.parse(localStorage.users)
        else
          this.users = await (await fetch('https://api.mocki.io/v1/a2790e8c')).json()
            
        this.loading = false
      }
      catch (error) {
        console.log(error)
      }
    },
    editUser(index, userData) {
      userData.index = index
      this.user = userData
      this.openForm()
    },
    saveUser(userData) {
      if(isNaN(userData.index)) {
        this.users.push(userData);
      }
      else {
        this.users.splice(userData.index, 1, userData)
      }
    },
    deleteUser(index) {
      this.users.splice(index, 1)
    },
    openForm() {
      this.formIsVisible = true
    },
    closeForm() {
      this.user = {}
      this.formIsVisible = false
    }
  },
  watch: {
    users() {
      localStorage.users = JSON.stringify(this.users)
      this.closeForm()
    }
  },
}
</script>

<style scoped>
.mdc-top-app-bar {
  background: var(--color-primary);
  color: var(--color-text);
  position: relative;
}
.mdc-top-app-bar__title {
  font-weight: bold;
}
.mdc-button--raised {
  background: var(--color-secondary);
  color: var(--color-text);
}
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