<script setup>
import { onMounted, ref } from 'vue';
import Pie from '../components/Pie.vue'


</script>

<template>
  <main>
    <div class="container">
      <Pie  title="Analysis" />
    </div>
  </main>
</template>

<style>
.container{
  padding: 20px;
  background: #ffffff;
}
</style>
