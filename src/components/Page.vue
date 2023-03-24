<template>
  <!-- basic page to train with VUE and tailwind -->
  <div class="container mx-auto flex flex-col">
    <div class="flex flex-col items-center justify-center">
      <h1 class="text-4xl font-bold text-center text-gray-900">Welcome to {{ msg }}</h1>
      <p class="text-lg text-center text-gray-500">This is a basic page to train with VUE and CESIUM</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pingPage',
  props: {
    msg: String
  }
}
</script>