<template>
  <div>
    <ThreeJs /> 
  </div>
</template>

<script>
// @ is an alias to /src
import ThreeJs from "@/components/ThreeJs.vue"

export default {
  name: 'HomeView',
  components: {
    ThreeJs
  }
}
</script>
