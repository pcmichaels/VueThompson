<template>
    <div v-bind:style="location">
        
    </div>
</template>

<script>
export default {
  name: 'Player',
  props: {
    location: String
  }
}
</script>
