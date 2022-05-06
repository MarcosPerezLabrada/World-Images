<template>
    <div>
        <h3 class="some-margin">Invoice Information:</h3>
        <div v-if="this.data">
        <p class="some-margin">Date: {{this.data.date}}</p>
        <p class="some-margin">Expiration: {{this.data.dueDate}}</p>
        <p class="some-margin">Client: {{this.data.client.name}}</p>
        <p class="some-margin">Seller: {{this.data.seller.name}}</p>
        <p class="some-margin">Items: {{this.data.items[0].name}} </p>
        <p class="some-margin">Quantity: {{this.data.items[0].quantity}}</p>
        <p class="some-margin">Total: {{this.data.total}} $</p>
        </div>
    </div>
</template>


<script>

export default {
    name:'InvoiceDetails',
    props:{
        updateinvoice:Boolean,
    },
    data(){
        return{data:undefined}
    },
    watch:{
        updateInvoice: function() {this.getdata()},
    },
    async created(){
        var myHeaders = new Headers();
        myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
        myHeaders.append("Cookie", "PHPSESSID=1eljkp89m6c4aal8d8oq8m1id6");
        var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
        };
        let data = await fetch("https://api.alegra.com/api/v1/invoices/", requestOptions)
        data= await data.json();
        this.data=data[0];
    },
    methods:{
        async getData(){
        var myHeaders = new Headers();
        myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
        myHeaders.append("Cookie", "PHPSESSID=1eljkp89m6c4aal8d8oq8m1id6");
        var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
        };
        let data = await fetch("https://api.alegra.com/api/v1/invoices/", requestOptions)
        data= await data.json();
        this.data=data[0];
        this.$emit("data-updated");
    },
    }
}
</script>


<style scoped>
div{
  text-align:center;
  justify-content:center;
}
.some-margin{
    margin:2%;
}
</style>