<template>
   <nav class="navbar navbar-light bg-dark some-padding">
  <form class="form-inline">
    <button @click="toggle1()" class="btn" :class="this.searching?  'btn-outline-success' : 'btn-sm btn-outline-success'" type="button">Search Images</button>
    <button @click="toggle2()" class="btn" :class="!this.searching?  'btn-outline-success' : 'btn-sm btn-outline-success'" type="button">View Last Invoice</button>
    <i @click="$emit('reset')" class="fa-solid fa-rotate position text-success"></i>
  </form>
</nav>
</template>

:class="condition?' conditionalclasstoappend':''"

<script>
export default {
  name: 'Navbar',
    data(){
        return{searching:true}
    }
  ,methods:{
      toggle1(){
        if (this.searching)
        {
          return;
        }
          this.searching=!this.searching;
          this.$emit('toggle');
      },
      toggle2(){
        if (!this.searching)
        {
          return;
        }
          this.searching=!this.searching;
          this.$emit('toggle');
      }
  }
}
</script>

<style scoped>
.some-padding{
    padding-left:2%;
}
.reset{
  right:1%;position:absolute;
}
.position{

  position:absolute;
  right:1%;
  bottom:15%;
}
</style>