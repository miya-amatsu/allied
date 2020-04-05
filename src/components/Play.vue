<template>
  <div>
    <h1>PlayVideo</h1>
    <iframe width="560" height="315" v-bind:src=" 'https://www.youtube.com/embed/'+ id "
    frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
  </div>
</template>

<script>

  export default {
    name: 'YoutubeList',
    props: {},
    data() {
      return {
       str:window.location.href,
       id:""
      }
    },

    created() {
     if(this.str.indexOf('?v=') == -1){
         console.log(-1);
     }
     this.videoLink()
    },
    mounted() {

    },
    updated() {

    },
    computed: {},

    methods: {
      videoLink(){
        let str=this.str;
        let id = str.substr(str.indexOf("?v=")+3,11) 
        let test = str.substr(0,str.indexOf("video")) 
        console.log(test);
        
        this.id=id;
        return id 
      }
    }
  }
</script>