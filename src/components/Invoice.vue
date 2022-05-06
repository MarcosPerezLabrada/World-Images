<template>
    <div>
        <button @click="createInvoice()" type="button" class="btn btn-outline-success btn-lg">Create Invoice.</button>
    </div>
</template>


<script>
export default {
    name:'Invoice',
    props:{
        sellerId:Number,
        quantity:Number,
    },
    methods:{
        async createInvoice(){
            var myHeaders = new Headers();
            myHeaders.append("Authorization", "Basic bWFyY29zcGVyZXpsYWJyYWRhMkBnbWFpbC5jb206YjllMjNkMjY0ZDc0NDE1ZmM0NmM=");
            myHeaders.append("Content-Type", "text/plain");
            myHeaders.append("Cookie", "PHPSESSID=ns7578noisgltrp27u5lv38rq0");
         //   var raw = "{\"seller\":"+this.sellerId+",\"date\":\"2022-02-23\",\n\"dueDate\":\"2022-02-23\",\n\"client\":1,\n\"items\":[\n   {\n      \"id\":1,\n     \"quantity\":"+this.quantity+",\n     \"price\":1000\n   }\n]\n}";
            var raw = "{\"seller\":"+this.sellerId+",\"date\":\"2022-02-23\",\n\"dueDate\":\"2022-02-23\",\n\"client\":1,\n\"items\":[\n   {\n      \"id\":1,\n      \"quantity\":"+this.quantity+",\n      \"price\":1000\n   }\n]\n}";

            console.log(raw);
           

            var requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: 'follow'
            };
            fetch("https://api.alegra.com/api/v1/invoices", requestOptions)
            this.$emit("update-invoice");
        }
    }
}
</script>


<style scoped>
    button{
        margin: 4%;
    }
    div{
        text-align:center;
        justify-content:center;
    }
</style>