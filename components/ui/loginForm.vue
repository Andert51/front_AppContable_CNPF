<template>
  <v-row align="center" justify="center" class="fill-height">
    <v-col cols="12" sm="8" md="4">
      <v-card class="pa-8" elevation="3" max-width="440">
        <v-card-title class="text-center mb-6 text_title">
          ¡Inicia sesión para tener acceso a tus datos más recientes!
        </v-card-title>

        <v-card-text>
          <form>
            <v-text-field
              v-model="form.username"
              label="usuario"
              filled
              rounded
              dense
            />

            <v-text-field
              v-model="form.password"
              label="Password"
              type="password"
              filled
              rounded
              dense
            />

            <v-btn color="#0F4C75" block class="mt-4 py-3 logbtn" @click="login_method">
              Iniciar Sesión
            </v-btn>
          </form>

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
</template>

<script>
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      errorMessage: ''
    }
  },
  methods: {
    async login_method () {
      // eslint-disable-next-line no-console
      console.log('login')
      try {
        const response = await this.$auth.loginWith('local', {
          data: this.form
        })
        // eslint-disable-next-line no-console
        console.log('@res =>', response)
        if (response.data.success) {
          this.$router.push('/dashboard')
        }
      } catch (error) {
        this.errorMessage = error
      }
    },
    signup_method () {
      // eslint-disable-next-line no-console
      console.log('signup')
    }
  }
}
</script>

<style scoped>
.fill-height {
  height: 100vh;
  background-color: #3282B8; /* Fondo azul */
}

.text_title {
  color: black;
  font-family: 'Roboto', sans-serif;
  font-size: 22px;
  font-weight: 400;
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
