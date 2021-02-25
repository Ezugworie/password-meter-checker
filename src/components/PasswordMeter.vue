<template>
      <meter 
        class="w-5/6 md:w-1/4 my-2"
        min="0" 
        max="100" 
        low="20" 
        high="68" 
        optimum="89" 
        :value="strength"
      />
</template>

<script>
import { defineComponent, defineProps, ref } from 'vue'
  
export default defineComponent ({
  props: {
    strength: {
      type: Number,
      default: 0
    }
  },
});
</script>

<style>

</style>