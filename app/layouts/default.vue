<script setup>
import Header from '../components/layout/Header.vue';
</script>

<template>
  <Header/>
  <main class="max-w-7xl mx-auto">
    <slot>Default Layout</slot>
  </main>
  <footer></footer>
</template>