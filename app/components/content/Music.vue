<script setup>
defineProps({
  url: {
    type: String,
    default: ''
  },
})
</script>

<template>
  <iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" style="width:100%;max-width:660px;overflow:hidden;border-radius:10px;" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation" 
    src={{ url }}
></iframe>
</template>

