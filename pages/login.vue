<template>
  <v-row align="center" justify="center">
    <v-col cols="10">
      <v-card class="pa-16" elevation="4" rounded="xl">
        <v-row align="center" justify="center">
          <v-col align-self="center">
            <v-img
              :src="require('~/assets/svg/undraw_step_to_the_sun_nxqq.svg')"
              class="mx-auto"
              width="80%"
            ></v-img>
          </v-col>
          <v-divider vertical class="mr-5" />
          <v-col cols="3">
            <v-card elevation="0">
              <v-card-text>
                <h1 class="text-center login-title">
                  {{ isLogin ? 'Sign In' : 'Sign Up' }}
                </h1>
                <v-form v-if="isLogin" ref="form">
                  <v-text-field
                    v-model="email"
                    prepend-inner-icon="mdi-email"
                    label="Email"
                    :rules="rules.emailRules"
                    outlined
                  />
                  <v-text-field
                    v-model="password"
                    prepend-inner-icon="mdi-lock"
                    label="Password"
                    :rules="rules.passwordRules"
                    outlined
                    type="password"
                  />
                  <v-checkbox label="Remember me"> </v-checkbox>
                </v-form>
                <v-form v-else ref="form">
                  <v-text-field
                    v-model="email"
                    prepend-inner-icon="mdi-email"
                    label="Email"
                    :rules="rules.emailRules"
                    outlined
                  />
                  <v-text-field
                    v-model="username"
                    prepend-inner-icon="mdi-account"
                    label="Username"
                    :rules="rules.usernameRules"
                    outlined
                  />
                  <v-text-field
                    v-model="password"
                    prepend-inner-icon="mdi-lock"
                    label="Password"
                    :rules="rules.passwordRules"
                    outlined
                    type="password"
                  />
                  <v-text-field
                    v-model="passwordConfirm"
                    prepend-inner-icon="mdi-lock"
                    label="Password Confirmation"
                    :rules="rules.passwordConfirmRules"
                    outlined
                    type="password"
                  />
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-btn block color="primary" @click="submitHandler">{{
                  isLogin ? 'Signin' : 'Signup'
                }}</v-btn>
              </v-card-actions>
              <div class="text-body text-center my-5">or</div>
              <v-card-actions>
                <v-btn block color="default" @click="changeLogin">{{
                  isLogin ? 'Signup' : 'Signin'
                }}</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import {useNuxtApp, ref} from '@nuxtjs/composition-api'

export default {
  name: 'LoginPage',
  layout: 'login',
  setup() {
    // initialize
    const { $api } = useNuxtApp()

    // field
    const form = ref(null)
    const email = ref('')
    const username = ref('')
    const password = ref('')
    const passwordConfirm = ref('')

    // rules
    const emailRules = [
      (v) => !!v || 'email is required',
      (v) => /[a-z0-9]+@[a-z]+\.[a-z]{2,3}/.test(v) || 'email must be valid',
    ]
    const usernameRules = [
      (v) => !!v || 'username is required',
      (v) => (v && v.length >= 3) || 'Username must more than 3 characters',
    ]
    const passwordRules = [
      (v) => !!v || 'Password is required',
      (v) => (v && v.length >= 6) || 'Password must more than 6 characters',
    ]
    const passwordConfirmRules = [
      ...passwordRules,
      (v) =>
        v === password.value ||
        'Password confirmation need match with password',
    ]
    const rules = {
      emailRules,
      usernameRules,
      passwordRules,
      passwordConfirmRules,
    }

    // mode
    const isLogin = ref(true)
    const changeLogin = () => {
      isLogin.value = !isLogin.value
      form.value.reset()
    }

    // submit handler
    const submitHandler = async () => {
      const result = form.value.validate()
      if (result) {
        try {
          if (isLogin) {
          const login = await $api.post('/user/register')
          console.log(login.data)
        }
        } catch (error) {

        }
        // submit value to server
      }
    }

    return {
      form,
      email,
      username,
      password,
      passwordConfirm,
      isLogin,
      changeLogin,
      rules,
      submitHandler,
    }
  },
}
</script>

<style>
.login-title {
  margin-bottom: 50px !important;
}
</style>
