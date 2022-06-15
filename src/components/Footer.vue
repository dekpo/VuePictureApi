<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
 <footer class="bg-dark text-center text-white p-3">
   &copy; {{msg}}
 </footer>
</template>

<style scoped>

</style>
