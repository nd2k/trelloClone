<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <v-form 
          v-if="!loading"
          v-model="valid" 
          @submit.prevent="signUp"
          @keydown.prevent.enter>
          <v-text-field
            v-model="user.email"
            :rules="emailRules"
            label="E-mail"
            type="email"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.displayName"
            :rules="notEmptyRules"
            label="Displayed name"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.password"
            :rules="notEmptyRules"
            label="Password"
            type="password"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.confirmPassword"
            :rules="confirmPasswordRules"
            label="Confirm Password"
            type="password"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.imageUrl"
            :rules="notEmptyRules"
            label="Image URL"
            required
          ></v-text-field>
          <v-btn type="submit" :disabled='!valid'>SignUp</v-btn>
        </v-form>
        <div class="text-xs-center">
          <v-progress-circular
            v-if="loading"
            :size="50"
            color="primary"
            indeterminate
          ></v-progress-circular>
        </div>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import  { mapState } from 'vuex'

export default {
  name: 'signUp',
  data: (vm) => ({
    valid: false,
    user: {
      email: '',
      password: '',
      confirmPassword: '',
      displayName: '',
      imageUrl: ''
    },
    notEmptyRules: [
      value => !!value || 'This field is required'
    ],
    emailRules: [
        value => !!value || 'E-mail is required',
        value => /.+@.+/.test(value) || 'E-mail must be valid'
    ],
    confirmPasswordRules: [
      (confirmPassword) => confirmPassword === vm.user.password || 'Password must match'

    ]
  }),
  computed: {
    ...mapState('users', { loading: 'isCreatePending' })
  },
  methods: {
    signUp() {
      if (this.valid) {
        const { User } = this.$FeathersVuex
        const user = new User (this.user)
        user.save({}).then(user => {
          console.log(user)
          this.$router.push('/login')
        })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
