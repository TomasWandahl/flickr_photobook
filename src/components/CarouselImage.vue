<template>
  <transition name="left">
    <div v-show="visibleImages.includes(index)" class="carousel-image">
      <slot> </slot>
    </div>
  </transition>
</template>

<script>


export default {
  name: 'CarouselImage',
  props: ['visibleImages', 'index'],
  data() {
    return {
     
    }
  },
  methods: {
   
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .carousel-image {
    position: absolute;
  }
  .left-enter-active {
    animation: inAnimation 3s ease;
  }
  .left-leave-active {
    animation: outAnimation 3s ease;
  }

  @keyframes outAnimation {
    from {transform: translateX(0%);}
    to {transform: translateX(-100%);}
  }
  @keyframes inAnimation {
    from {transform: translateX(100%);}
    to {transform: translateX(0%);}
  }
</style>
