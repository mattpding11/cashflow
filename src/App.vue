<template>
  <!-- <SplashScreen /> -->
  <!-- <Home /> -->
  <Suspense>
    <!-- recibe dos slots, default y fallback -->
    <template #default>
      <Home />
    </template>  
    <!-- Primero se ejecuta este se muestra mientras el otro carga -->
    <template #fallback>
      <SplashScreen />
    </template>  
  </Suspense>
</template>

<script>
import SplashScreen from './components/SplashScreen'
import {defineAsyncComponent} from 'vue'
export default {
  name: 'App',
  components: {
    // aqui se define componente asincrono que espera un momento para ser definido y mostrado en la vista
    Home: defineAsyncComponent(() => 
      new Promise((resolve) => {
        setTimeout(() => {
          resolve(import("./components/Home.vue"))
        }, 2500)
      })
    ),
    SplashScreen
  }
}
</script>

<style>

/* Se crean variables en elemento raiz del documento */
:root{
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
