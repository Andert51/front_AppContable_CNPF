<template>
  <v-card width="1000" height="500" color="white" class="pa-5 rounded-card">
    <v-card-title class="text-h5 text-center" style="color: black;">
      Registrarse
    </v-card-title>

    <v-card-text>
      <v-form>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="user.name"
              dense
              rounded
              filled
              label="Nombre"
              placeholder="Escribe tu nombre completo"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.username"
              dense
              rounded
              filled
              label="Usuario"
              placeholder="Escoge tu nombre de usuario"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.email"
              dense
              rounded
              filled
              label="Email"
              placeholder="Ingresa tu email"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="user.phone"
              dense
              rounded
              filled
              label="Telefono"
              placeholder="Escribe tu telefono"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.address"
              dense
              rounded
              filled
              label="Direccion"
              placeholder="Escribe tu direccion"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.city"
              dense
              rounded
              filled
              label="Ciudad"
              placeholder="Escribe tu ciudad"
            />
          </v-col>
          <v-col cols="4">
            <v-combobox
              v-model="user.role"
              :items="['Admin', 'User', 'Guest', 'SuperAdmin', 'SuperUser', 'SuperGuest']"
              dense
              rounded
              filled
              label="Rol"
              placeholder="Selecciona tu rol"
            />
          </v-col>
          <v-col cols="4">
            <v-file-input
              v-model="user.image"
              dense
              rounded
              filled
              label="Imagen de Perfil"
              placeholder="Sube tu imagen de perfil"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="user.password"
              dense
              type="password"
              rounded
              filled
              label="Contraseña"
              placeholder="Escribe tu contraseña"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="confirmPassword"
              dense
              type="password"
              rounded
              filled
              label="Confirma tu contraseña"
              placeholder="Confirma tu contraseña"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.info"
              dense
              rounded
              filled
              label="Estado del cliente"
              placeholder="Escribe el estado del cliente"
            />
          </v-col>
        </v-row>
        <v-row align="center" justify="center" class="ma-3">
          <v-btn color="#1B262C" class="custom-btn" @click="registerClient">
            Registrarse
          </v-btn>
        </v-row>
      </v-form>
    </v-card-text>

    <v-card-actions>
      <v-row align="center" justify="center" class="ma-3">
        <div class="text-center" style="font-size: 14px; color:white;">
          Ya tienes cuenta?
          <a class="font-weight-medium" style="color: white; cursor: pointer;" @click="gotoLogin">Inicia Sesión</a>
        </div>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      user: {},
      confirmPassword: ''
    }
  },
  methods: {
    gotoLogin () {
      this.$router.push('/')
    },
    async registerClient () {
      if (this.user.password === this.confirmPassword) {
        const formData = new FormData()
        for (const key in this.user) {
          formData.append(key, this.user[key])
        }
        const response = await this.$axios.post('/client/add', formData)
        if (response.data.success) {
          this.user = {}
          this.$router.push('/')
        }
      }
    }
  }
}
</script>

<style scoped>
.rounded-card {
    border-radius: 12px;
}

.custom-input .v-input__control {
    background-color: #F2F2F2 !important;
    border-radius: 24px !important;
    font-size: 16px;
}

.custom-btn {
    background-color: #1B262C !important;
    color: white !important;
    border-radius: 10px !important;
    height: 48px;
    width: 300px;
}

.custom-btn:hover {
    background-color: #163B4D !important;
}

.v-card-title {
    font-weight: bold;
}

</style>
