<template>
  <v-app>
    <v-app-bar app color="white" dark elevation="0">
      <v-spacer />

      <div class="user-profile">
        <v-avatar size="48">
          <img src="https://randomuser.me/api/portraits/lego/6.jpg">
        </v-avatar>
        <div class="user-info">
          <div class="user-name">
            {{ client.username }}
          </div>
          <div class="user-role">
            {{ client.role }}
          </div>
        </div>
      </div>
    </v-app-bar>

    <v-navigation-drawer
      class="nav-menu"
      permanent
      app
      color="#0F4C75"
      width="300px"
      height="100vh"
    >
      <div class="dashtitle logo">
        LOGO
      </div>

      <v-list nav dense>
        <v-list-item
          v-for="(item, index) in items"
          :key="index"
          class="menu-item"
          link
          :to="item.path"
        >
          <v-list-item-content>
            <v-list-item-title class="dashtitle">
              {{ item.title }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container fluid>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      items: [
        { title: 'HOME', path: '/dashboard/home' },
        { title: 'CONTACTOS', path: '/dashboard/contactos' },
        { title: 'PUNTO DE VENTAS', path: '/dashboard/ventas' },
        { title: 'INGRESOS', path: '/dashboard/ingresos' },
        { title: 'INVENTARIOS', path: '/dashboard/inventarios' },
        { title: 'CONFIGURACION', path: '/dashboard/configuracion' }
      ],
      client: {
        username: '',
        role: ''
      }
    }
  },
  created () {
    this.getClientInfo()
  },
  methods: {
    async getClientInfo () {
      try {
        const response = await this.$axios.get('/auth/user')
        this.client = response.data
      } catch (error) {
        // eslint-disable-next-line no-console
        console.error(error)
      }
    }
  }
}
</script>

<style scoped>
.nav-menu {
    border-top-right-radius: 20px;
    border-bottom-right-radius: 20px;
    overflow-y: auto;
}

.dashtitle {
    color: white;
    font-size: 20px;
    padding-left: 16px;
    font-family: Lato;
    font-size: 18px;
    font-weight: 700;
    line-height: 21.6px;
    text-align: left;
    text-underline-position: from-font;
    text-decoration-skip-ink: none;
}

.logo {
    padding: 20px 0;
    font-size: 24px;
    text-align: center;
    font-family: Lato;
}

.menu-item {
    border-radius: 12px;
    margin: 8px 16px;
    transition: background-color 0.3s;
}

.menu-item:hover {
    background-color: rgba(255, 255, 255, 0.1);
}

.menu-item:active, .menu-item--active {
    background-color: rgba(255, 255, 255, 0.2);
}

/* Estilos para el perfil de usuario en la barra superior */
.user-profile {
    display: flex;
    align-items: center;
    color: white;
    margin-right: 20px;
}

.user-info {
    margin-left: 10px;
    text-align: right;
    color: black;
}

.user-name {
    font-size: 16px;
    font-weight: bold;
    color: black;
}

.user-role {
    font-size: 14px;
    color: black;
}
</style>
