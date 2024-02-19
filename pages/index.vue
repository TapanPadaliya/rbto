<template>
  <div class="flex items-center justify-center">
    <div class="container">
      Index page
      <div class="flex items-center justify-center">
        <ClientOnly>
          <FormsRegister />
        </ClientOnly>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
</script>
