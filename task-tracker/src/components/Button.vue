<template>
  <button 
    class="btn"
    @click="$emit('btn-click')"
    :style="{background: color}"
  >
    {{text}}
  </button>
</template>

<script>
  export default {
    name: 'Button',
    props: {
      text: String,
      color: String
    },
  }
</script>