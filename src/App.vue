<template>
  <div>
    <Header />
    <Navbar @reset="this.reset()" @toggle="this.searching=!this.searching"/>
  <div v-if="searching">
    <Search v-if="!winner()" @search="searchQuerie" />
    <Invoice @update-invoice="updateInvoice()" v-else v-bind:quantity="this.quantity" v-bind:sellerId="this.whowins" />
    <Sellers class="margin-images"  @image-selected="this.selected=true" v-if="showSellers()" v-bind:selected="this.selected" v-bind:sellers="this.sellers" /> 
    <div class="div-message" v-else>
    <p >There are not images to show. Type a query.</p>
    </div>
  </div>
  <div v-else>
    <InvoiceDetails class="margin-invoice" @data-updated="this.updateinvoice=false" v-bind:updateinvoice="this.updateinvoice"/>
  </div>
  <div class="text-white bg-dark" id="footer">
      <Footer class="footer bg-dark text-light" />
    </div>
  </div>
</template>

<script>
import Search from './components/Search';
import Header from './components/Header';
import Sellers from './components/Sellers';
import Footer from './components/Footer';
import Invoice from './components/Invoice';
import Navbar from './components/Navbar';
import InvoiceDetails from './components/InvoiceDetails';

export default {
  name: 'App',
  components: {
    Search,
    Sellers,
    Header,
    Footer,
    Invoice,
    Navbar,
    InvoiceDetails,
  },
  data() {
    return {
      sellers:[],
      selected:false,
      search:"",
      whowins:0,
      quantity:0,
      searching:true,
      updateinvoice:false,
    }
  },
  methods:
  { async reset(){
      var myHeaders = new Headers();
      myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
      myHeaders.append("Content-Type", "text/plain");
      myHeaders.append("Cookie", "PHPSESSID=1jncfm0cfogkg919kcv5epb546");
      var raw = "{\"observations\":\""+0+"\"}";

      var requestOptions = {
        method: 'PUT',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };
      for(let i=1;i<=this.sellers.length;i++){
        fetch("https://api.alegra.com/api/v1/sellers/"+i+"/", requestOptions);
        this.sellers[i-1].score=0;
      }
  },
    updateInvoice(){
    this.updateinvoice=true;

  },
    winner(){
      var temp=0;
      let answer=false;
      for(let i=0;i<this.sellers.length;i++){
        temp=temp+this.sellers[i].score;
        if(this.sellers[i].score>20){
          this.whowins=i+1;
          answer =true;
        }
      }this.quantity=temp;
      return answer;
    }
    ,
    showSellers(){
      if(this.sellers===undefined){
        return false;
      }
      if(this.sellers.length===0){
        return false;
      }
      if(this.sellers[0].url===''){
        return false;
      }
      return true;
    },
    async searchQuerie(value){
      if(value.length==0){
        alert("Please enter a query");
        return;
      }
      if(value===this.search){
        alert("That is the same querie of the last time");
        return;
      }
      let url='https://api.unsplash.com/search/photos?query='+value+'&client_id=aVDUlDlqBs8NRgG4PoOFClGMA_ssU1VljWhMv819CX0';
      var myHeaders = new Headers();
      myHeaders.append("Cookie", "ugid=345eaf3a1db118d65bf3f884de2638c35484680");
      var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'};
      let data = await fetch(url, requestOptions)
      data= await data.json();
      if(data.total==0){
        alert("The is not result for that query in the database");
      }
      for(let i=0;i<this.sellers.length;i++){
        this.sellers[i].url= data.results[i%data.total].urls.thumb; 
      }
      this.selected=false;
      this.search=value;
    },
  },
  async created(){
    var myHeaders = new Headers();
    myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
    myHeaders.append("Cookie", "PHPSESSID=5cvmv84bo4ftjevhbntp9c1bs2");
    var requestOptions = {
      method: 'GET',
      headers: myHeaders,
      redirect: 'follow'
    };
    let data =  await fetch("https://api.alegra.com/api/v1/sellers/", requestOptions);
    data=await data.json();
    this.sellers=data;
    for(let i =0;i<this.sellers.length;i++){
      this.sellers[i].url="";
      this.sellers[i].score=parseInt(this.sellers[i].observations);
    }
  }
}
</script>




<style>
.footer{
  bottom:0;
  position:fixed;
  width:100%;
  padding:3%;
}
.margin-images{
  margin-bottom:10%;
}
.margin-invoice{
  margin-bottom:15%;
}
.div-message{
  justify-content:center;
  text-align:center;
}
</style>
