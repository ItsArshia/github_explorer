<script setup>
import Header from '../components/layout/Header.vue';
</script>

<template>
  <Header/>
  <main>
    <slot>Default Layout</slot>
  </main>
  <footer></footer>
</template>