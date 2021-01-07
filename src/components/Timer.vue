<template>
    <p>Seconds remaining: {{ seconds }}</p>
</template>

<script>
export default {
  name: 'Timer',
  props: {
    seconds: Number
  }
}
</script>
