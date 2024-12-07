<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" class="text-center">
        <h1>Dashboard Home</h1>
        <p>Bienvenido a tu aplicacion de gestion de clientes e ingresos.</p>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="6">
        <v-card class="mb-4 hover-card" @click="gotoClients">
          <v-card-title>
            <v-icon left>
              mdi-account-multiple
            </v-icon>
            Contactos
          </v-card-title>
          <v-card-text>
            <p>Administra la informacion de tus clientes.</p>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6">
        <v-card class="mb-4 hover-card" @click="gotoSales">
          <v-card-title>
            <v-icon left>
              mdi-cart
            </v-icon>
            Punto de Ventas
          </v-card-title>
          <v-card-text>
            <p>Administra tus ventas y realiza compras.</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="4">
        <v-card class="mb-4 hover-card" @click="gotoIncome">
          <v-card-title>
            <v-icon left>
              mdi-cash
            </v-icon>
            Ingresos
          </v-card-title>
          <v-card-text>
            <p>Sigue tus ingresos y gastos.</p>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card class="mb-4 hover-card" @click="gotoStorage">
          <v-card-title>
            <v-icon left>
              mdi-warehouse
            </v-icon>
            Inventarios
          </v-card-title>
          <v-card-text>
            <p>Administra tus inventarios.</p>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card class="mb-4 hover-card" @click="gotoSettings">
          <v-card-title>
            <v-icon left>
              mdi-cog
            </v-icon>
            Configuración
          </v-card-title>
          <v-card-text>
            <p>Configura tu cuenta y ajustes.</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" class="text-center">
        <v-img src="path/to/illustration.png" aspect-ratio="1.7" class="my-4" />
        <p>Nuestra aplicación lo ayuda a administrar su negocio de manera eficiente al brindarle herramientas para realizar un seguimiento de sus ventas, administrar su inventario y mantener actualizada la información de sus clientes..</p>
      </v-col>
    </v-row>
    <v-footer class="footer">
      <v-col class="text-center">
        <p>&copy; 2024 Your Company. All rights reserved.</p>
      </v-col>
    </v-footer>
  </v-container>
</template>

<script>
export default {
  layout: 'dashboard',
  middleware: 'auth',
  methods: {
    gotoClients () {
      this.$router.push('/dashboard/contactos')
    },
    gotoSales () {
      this.$router.push('/dashboard/ventas')
    },
    gotoInventories () {
      this.$router.push('/dashboard/inventarios')
    },
    gotoSettings () {
      this.$router.push('/dashboard/configuracion')
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #1B262C;
}

.v-card-title {
  font-size: 24px;
  font-weight: bold;
  color: #3282B8;
}

.v-card {
  border-radius: 16px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.v-card-text {
  font-size: 16px;
  color: #1B262C;
}

.hover-card:hover {
  transform: translateY(-10px);
}

.footer {
  background-color: #0F4C75;
  color: white;
  padding: 20px 0;
}
</style>
