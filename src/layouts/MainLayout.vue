<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="app-header" elevated>
      <q-toolbar class="q-px-md q-px-lg-lg q-py-sm">
        <q-icon name="auto_graph" size="32px" color="cyan-4" class="q-mr-sm" />

        <q-toolbar-title class="text-weight-bold">
          Quamodoro
        </q-toolbar-title>

        <q-space />

        <q-avatar size="34px" class="tiger-badge">
          <svg
            viewBox="0 0 64 64"
            aria-hidden="true"
            role="img"
            class="tiger-svg"
          >
            <defs>
              <linearGradient id="tigerGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                <stop offset="0%" stop-color="#ffd166" />
                <stop offset="100%" stop-color="#f97316" />
              </linearGradient>
            </defs>
            <circle cx="32" cy="32" r="28" fill="url(#tigerGradient)" />
            <path d="M20 18 L26 12 L32 18 L38 12 L44 18" fill="none" stroke="#1f1308" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" />
            <path d="M23 26 C28 22, 36 22, 41 26" fill="none" stroke="#1f1308" stroke-width="4" stroke-linecap="round" />
            <path d="M26 38 C29 34, 35 34, 38 38" fill="none" stroke="#1f1308" stroke-width="4" stroke-linecap="round" />
            <path d="M32 27 L32 45" fill="none" stroke="#1f1308" stroke-width="4" stroke-linecap="round" />
            <circle cx="32" cy="32" r="3.2" fill="#1f1308" />
          </svg>
        </q-avatar>
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

.tiger-badge {
  background: linear-gradient(145deg, rgba(249, 115, 22, 0.95), rgba(245, 158, 11, 0.95));
  box-shadow: 0 0 18px rgba(249, 115, 22, 0.35);
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.tiger-svg {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
