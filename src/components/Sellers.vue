<template>
  <div  class="container">
    <div class="row row-cols-1 row-cols-sm-1 row-cols-md-2 row-cols-lg-3 row-cols-xl-4 row-cols-xxl-5">
      <div v-bind:key="seller.id" v-for="seller in sellers">
        <Seller @image-selected="imageSelected()" v-bind:seller_info="seller" v-bind:selected="this.selected" />
      </div>
    </div>
  </div>
</template>

<script>
  import Seller from './Seller'

export default {
  name: 'Sellers',
  components: { Seller },
  props: {
    sellers:Array,
    selected:Boolean,
  },

  methods:{
    imageSelected(){
      this.$emit('image-selected');
    }
  }
}
</script>

<style scoped>
  .div-container{
    text-align:center;
    justify-content:center;
  }
</style>
