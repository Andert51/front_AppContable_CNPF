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
              label="Name"
              placeholder="Write your name"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.username"
              dense
              rounded
              filled
              label="Username"
              placeholder="Write your Username"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.email"
              dense
              rounded
              filled
              label="Email"
              placeholder="Write your email"
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
              label="Phone"
              placeholder="Write your phone"
            />
          </v-col>
          <v-col cols="4">
            <v-combobox
              v-model="user.role"
              :items="['Admin', 'User', 'Guest', 'SuperAdmin', 'SuperUser', 'SuperGuest']"
              dense
              rounded
              filled
              label="Role"
              placeholder="Select your role"
            />
          </v-col>
          <v-col cols="4">
            <v-file-input
              v-model="user.image"
              dense
              rounded
              filled
              label="Profile Picture"
              placeholder="Upload your profile picture"
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
              label="Password"
              placeholder="Write your password"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="confirmPassword"
              dense
              type="password"
              rounded
              filled
              label="Confirm Password"
              placeholder="Confirm your password"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="user.info"
              dense
              rounded
              filled
              label="Info"
              placeholder="Write your info"
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
        <div class="text-center" style="font-size: 14px;">
          Tienes cuenta?
          <a class="font-weight-medium" style="color: #1B262C; cursor: pointer;" @click="gotoLogin">Inicia Sesión</a>
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
