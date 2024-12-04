<template>
  <v-container fluid class="pa-5">
    <v-row>
      <v-col cols="12">
        <h1 class="page-title">
          Configuración
        </h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="6">
        <v-card class="pa-4 mb-4">
          <v-card-title class="section-title">
            Perfil de Usuario
          </v-card-title>
          <v-card-text>
            <v-text-field label="Nombre de Usuario" outlined dense />
            <v-text-field label="Correo Electrónico" outlined dense />
            <v-text-field label="Contraseña" type="password" outlined dense />
            <v-btn color="primary" class="mt-3">
              Actualizar Perfil
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6">
        <v-card class="pa-4 mb-4">
          <v-card-title class="section-title">
            Preferencias
          </v-card-title>
          <v-card-text>
            <v-switch label="Notificaciones" class="mb-3" />
            <v-switch label="Modo Oscuro" class="mb-3" />
            <v-btn color="primary" class="mt-3">
              Guardar Preferencias
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="6">
        <v-card class="pa-4 mb-4">
          <v-card-title class="section-title">
            Seguridad
          </v-card-title>
          <v-card-text>
            <v-text-field label="Contraseña Actual" type="password" outlined dense />
            <v-text-field label="Nueva Contraseña" type="password" outlined dense />
            <v-text-field label="Confirmar Nueva Contraseña" type="password" outlined dense />
            <v-btn color="primary" class="mt-3">
              Cambiar Contraseña
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6">
        <v-card class="pa-4 mb-4">
          <v-card-title class="section-title">
            Configuración Avanzada
          </v-card-title>
          <v-card-text>
            <v-text-field label="API Key" outlined dense />
            <v-text-field label="URL del Servidor" outlined dense />
            <v-btn color="primary" class="mt-3">
              Guardar Configuración
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card class="pa-4 mb-4">
          <v-card-title class="section-title">
            Buscar Configuración
          </v-card-title>
          <v-card-text>
            <v-text-field label="Buscar" outlined dense append-icon="mdi-magnify" />
            <v-btn color="primary" class="mt-3">
              Buscar
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'dashboard',
  middleware: 'auth'
}
</script>

<style scoped>
.page-title {
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #1B262C;
}

.section-title {
  font-size: 24px;
  font-weight: bold;
  color: #3282B8;
}

.v-card {
  border-radius: 16px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.v-btn {
  border-radius: 24px;
  text-transform: none;
}
</style>
