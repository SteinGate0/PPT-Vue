<template>
  <div class="about">
    <DrawMap/>
  </div>
</template>
<script>
// @ is an alias to /src
import DrawMap from '@/components/DrawMap.vue'

export default {
  name: 'AboutView',
  components: {
    DrawMap
  }
}
</script>
