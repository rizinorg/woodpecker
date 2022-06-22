<template>
  <div class="w-full max-w-5xl p-2 md:pb-0 lg:px-0 mx-auto">
    <slot />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'FluidContainer',
});
</script>
