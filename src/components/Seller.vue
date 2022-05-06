<template>
  <div class="col  seller ">
      <img v-if="this.seller_info.url != undefined & this.seller_info.url != ''" v-bind:src="this.seller_info.url" class="border border-success">
      <div class="seller-info"> 
        <button v-if="! this.selected" class="button btn-sm btn btn-outline-success" @click="onClick">Select.</button>
        <h6>{{seller_info.name}} Score: {{this.seller_info.score}}/20</h6>
        <h6 v-if="this.seller_info.score>20">Winner! 🎉</h6>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Seller',
  props:{
    seller_info:Object,
    selected:Boolean,
  },
  methods:{
    onClick(){
      this.seller_info.score+=3;
      var myHeaders = new Headers();
      myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
      myHeaders.append("Content-Type", "text/plain");
      myHeaders.append("Cookie", "PHPSESSID=1jncfm0cfogkg919kcv5epb546");
      var raw = "{\"observations\":\""+this.seller_info.score+"\"}";

      var requestOptions = {
        method: 'PUT',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      fetch("https://api.alegra.com/api/v1/sellers/"+this.seller_info.id+"/", requestOptions);
      this.$emit('image-selected');
    }
  }
}
</script>

<style scoped>
  .seller{
    text-align:center;
    justify-content:center;
  }
  img{
    margin:0 0 2% 0 ;
    max-width:200px;
    min-width:200px;
    max-height:200px;
    min-height:200px;
  }
  .seller-info{
    position:relative;
    bottom: 0;
    margin: 0 auto;
  }
</style>
