<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="4">
        <v-card class="pa-8" elevation="3" width="440">
          <v-card-title class="text-center mb-6 text_title">
            ¡Inicia sesión para tener acceso a tus datos más recientes!
          </v-card-title>

          <v-card-text>
            <v-form>
              <v-text-field
                v-model="username"
                label="usuario"
                filled
                rounded
                dense
              />

              <v-text-field
                v-model="password"
                label="Password"
                type="password"
                filled
                rounded
                dense
              />

              <v-btn color="#0F4C75" block class="mt-4 py-3 logbtn" @click="login_method">
                Iniciar Sesión
              </v-btn>
            </v-form>

            <div class="text-center mt-4">
              <span>No tienes cuenta. <a class="font-weight-medium" @click="signup_method">Regístrate</a></span>
            </div>
            <div class="text-center mt-1">
              <a href="#" class="grey--text text--darken-1">¿Olvidaste la contraseña?</a>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login_method () {
      // eslint-disable-next-line no-console
      console.log('login')
    },
    signup_method () {
      // eslint-disable-next-line no-console
      console.log('signup')
    }
  }

}
</script>

<style scoped>
.text_title {
  color: black;
  font-family: 'Roboto', sans-serif;
  font-size: 22px;
  font-weight: 400;
}
.fill-height {
  height: 100vh;
  background-color: #397cb3;
}

a {
  text-decoration: none;
  color: #397cb3;
}

.logbtn {
    color: white;
}

.grey--text {
  color: #aaa !important;
}
</style>
