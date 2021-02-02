<template>
    <div v-bind:style="location">
        <img :src="image" />
    </div>
</template>

<script>
export default {
  name: 'GameObject',
  props: {
    type: String,
    location: String,
    image: String
  }
}
</script>
