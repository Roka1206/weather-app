<template>
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString(`en-us`, {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>
</template>

<script setup>

</script>

<style  scoped>

</style>