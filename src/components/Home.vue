<template>
  <div class="home">
    <h1 @click="mouseShortPress">home</h1>
    <coolBtn :position="position"></coolBtn>
  </div>
</template>

<script>
  import coolNav from './coolNav/coolNav'
  import coolBtn from './coolBtn/coolBtn'
  import homeDragbtn from './homeDragbtn/homeDragbtn'
export default {
  name: 'Home',
  data(){
    return{
      position:{
        left: 0,
        top: 0,
      }
    }
  },
  components: {
    'cool-nav':coolNav,
    'homeDragbtn':homeDragbtn,
    'coolBtn':coolBtn
  },
  methods:{
    init(){
      let window_Width=window.innerWidth;
      let window_height=window.innerHeight;
      this.position.left=window_Width-60+'px';
      this.position.top=window_height-60+'px';
      // this.top=60;
    },
    mouseShortPress(){
      console.log('短按')
    },
    mouseLongPress(){
      console.log('长按')
    }
  },
  created() {
    this.init();
  },
  mounted() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
