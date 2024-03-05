<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <v-layout class="rounded rounded-md">
    <!--------------------------->
    <!--------------------------->
    <!----Componente DRAWER------>
    <!--------------------------->
    <!--------------------------->

    <v-navigation-drawer :elevation="30" class="drawer"
      v-model="rail"
      permanent
      >
      <v-divider :thickness="20" class="border-opacity-0"></v-divider>
      <v-container class="text-center">
        <v-img style="width: 100px; margin: auto;background-color: transparent;" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmpoQaaw13BKAmYv1iRPzkz9AkM0ZskCqK_g&usqp=CAU"></v-img>
        <v-divider :thickness="20" class="border-opacity-0"></v-divider>
        <p style="font-size: larger;">German Campos</p>
      </v-container>
        <v-divider :thickness="20" class="border-opacity-0"></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home-city" title="Principal" value="home" class="itemDrawer" to="/"></v-list-item>
          <v-list-item prepend-icon="mdi-folder-multiple" title="Servicios" value="services" class="itemDrawer" to="/servicios" ></v-list-item>
          <v-list-item prepend-icon="mdi-account" title="My Account" value="account" class="itemDrawer"></v-list-item>
          <v-list-item prepend-icon="mdi-account-group-outline" title="Users" value="users" class="itemDrawer"></v-list-item>
        </v-list>
        <template v-slot:append >
          <div class="pa-2">
            <v-btn block style="background-color: #be4d25;color: aliceblue;" >
              Cerrar Sesión
            </v-btn>
          </div>
        </template>
      </v-navigation-drawer>


    <!--------------------------->
    <!--------------------------->
    <!----Barra de navegcion----->
    <!--------------------------->
    <!--------------------------->
    <v-app-bar :elevation="30">
      <template v-slot:prepend>
        <v-btn
              
              variant="text"
              icon="mdi-menu"
              :rail="rail"
              @click.stop="rail = !rail"
              
            ></v-btn>
      </template>
      
      
        <v-img src="@/assets/logoGH.png" width="200px"></v-img>
      

      <template v-slot:append>

        <v-btn icon="mdi-magnify"></v-btn>
        <v-btn icon="mdi-dots-vertical"></v-btn>
      </template>
    </v-app-bar>

    <!--------------------------->
    <!--------------------------->
    <!--Visualizacion de vistas-->
    <!--------------------------->
    <!--------------------------->
    <v-main class="d-flex align-center justify-center" style="min-height: 615px;">
      <router-view />
    </v-main>
  </v-layout>
</template>


<script>
export default {
  data() {
    return {
      drawer: false, // Inicialmente cerrado
      rail: true
    };
  },
};
</script>
<style scoped>
  .itemDrawer{
      background-color: aliceblue;
  }
  .itemDrawer:hover{
    background-color: #a6adad;
  }
  .itemDrawer:focus{
    background-color: #66d6b5;
  }
  .drawer{
    background-color: rgb(255, 255, 255);
  }

</style>