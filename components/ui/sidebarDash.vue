<template>
  <v-navigation-drawer
    class="nav-menu"
    permanent
    color="#0F4C75"
    width="300px"
    height="100vh"
  >
    <div class="dashtitle logo">
      LOGO
    </div>

    <v-list nav dense>
      <v-list-item link class="menu-item">
        <v-list-item-title class="dashtitle">
          HOME
        </v-list-item-title>
      </v-list-item>
      <v-list-item link class="menu-item">
        <v-list-item-title class="dashtitle">
          CONTACTOS
        </v-list-item-title>
      </v-list-item>
      <v-list-item link class="menu-item">
        <v-list-item-title class="dashtitle">
          VENTAS
        </v-list-item-title>
      </v-list-item>
      <v-list-item link class="menu-item">
        <v-list-item-title class="dashtitle">
          INVENTARIOS
        </v-list-item-title>
      </v-list-item>
      <v-list-item link class="menu-item">
        <v-list-item-title class="dashtitle">
          CONFIGURACION
        </v-list-item-title>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {

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
}

.logo {
    padding: 20px 0;
    font-size: 24px;
    text-align: center;
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
</style>
