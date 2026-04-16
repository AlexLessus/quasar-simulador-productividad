<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="app-header" elevated>
      <q-toolbar class="q-px-md q-px-lg-lg q-py-sm">
        <q-icon name="auto_graph" size="32px" color="cyan-4" class="q-mr-sm" />

        <q-toolbar-title class="text-weight-bold">
          Quamodoro · Simulador de Productividad
        </q-toolbar-title>

        <q-badge color="positive" text-color="dark" label="Modo presentación" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { onMounted } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()

onMounted(() => {
  $q.dark.set(true)
})
</script>

<style scoped>
.app-header {
  backdrop-filter: blur(10px);
  background: linear-gradient(90deg, rgba(15, 23, 42, 0.92), rgba(2, 132, 199, 0.28));
  border-bottom: 1px solid rgba(0, 229, 255, 0.2);
}
</style>
