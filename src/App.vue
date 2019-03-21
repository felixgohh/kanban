<template>
  <div>
    <navbar/>
    <category/>
    <router-view/>
  </div>
</template>

<style>
body {
  background-color: #343a40;
}
</style>

<script>
import Navbar from '@/components/Navbar.vue';
import Category from '@/views/Category.vue';

export default {
  components: {
    Navbar,
    Category,
  },
};
</script>
