<template>
  <div class="collect">
    <CollectCompon v-bind:root="post.root" />
  </div>
</template>
<script>
import CollectCompon from '@/components/CollectCompon.vue'
export default {
   name: 'collect',
     components: {
    CollectCompon
  },data(){
    return{
        post:{root:window.location.href.substr(0,window.location.href.indexOf('collect'))}
    }
  }
}
</script>