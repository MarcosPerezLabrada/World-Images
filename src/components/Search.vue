<template>
  <div class="input-group search-div">
  <input id="image-search" type="search" class="form-control rounded border border-success" placeholder="Type a query" aria-label="Search" aria-describedby="search-addon" />
  <button @click="OnClick" type="button" class="btn btn-outline-success">search</button>

</div>
</template>

<script>
export default {
  name: 'Search',
  
  methods:{
    OnClick(){
      this.$emit('search',document.querySelector('#image-search').value);
    },
  }
}
</script>

<style scoped>
  .search-div{
  
    padding:2% 20% 2% 20%;
  }
  #image-search{
    width:20%;
  }
</style>
