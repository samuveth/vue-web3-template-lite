<script setup lang="ts">
import TheFooter from './components/TheFooter.vue'
import TheNavbar from './components/TheNavbar.vue'
import ModalConnect from './components/ModalConnect.vue'
</script>

<template>
  <div>
    <TheNavbar />
    <router-view />
    <TheFooter />
    <Teleport to="#modals">
      <ModalConnect />
    </Teleport>
  </div>
</template>
