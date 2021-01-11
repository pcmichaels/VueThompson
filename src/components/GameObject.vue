<template>
    <div v-bind:style="location">
        
    </div>
</template>

<script>
export default {
  name: 'GameObject',
  props: {
    type: String,
    location: String
  }
}
</script>
