<script setup>
defineProps({
  src: {
    type: String,
    default: "",
  },
});
</script>

<template>
  <img class="m-0 h-full w-full object-cover blur-2xl" :src="src" />
</template>
