<script setup>

import { defineProps, computed } from 'vue';

const props = defineProps(["orders"])


const formatDate = function(dateData){
let date
const options = { year: 'numeric', month: 'long', day: 'numeric' };

if(typeof dateData === "string"){
    date = new Date(dateData)
    return date.toLocaleDateString("tr-TR", options)
} else {
    return dateData.toLocaleDateString("tr-TR", options)
}
}

const sortedOrders = computed(() => {
    const ordersCopy = props.orders.slice()
    return ordersCopy.sort((a, b) => new Date(b.insertDate) - new Date(a.insertDate))
})

</script>

<template>
    <table class="alt-table w-full mt-5">
            <thead>
                <tr>
                    <th>Tarih</th>
                    <th>Mecra</th>
                    <th>Müşteri</th>
                    <th>Ürünler</th>
                    <th>Ödeme Yöntemi</th>
                    <th>Tutar</th>
                </tr>
            </thead>
            <tr v-for="order in sortedOrders" :key="order.id" class="hover:bg-gray-100 relative cursor-pointer border-b">
                <td>{{ formatDate(order.insertDate) }}</td>
                <td> {{ order.externalAppName }}</td>
                <td class="capitalize">{{order.customer.customerName}}</td>
                <td title="">
                    <ul class="">
                        <li class="!capitalize custom" v-for="product in order.products" :key="product.id">
                            <p class="" v-if="!product.productName.includes('İstemiyorum')">
                                {{ product.productName.toLocaleLowerCase("tr-TR") }}
                            </p>
                        </li> 
                    
                    </ul>
                </td>
                <td>{{order.payments[0].paymentName}}</td>
                <td>{{order.orderTotal}} ₺</td>
            </tr>
        </table>

</template>

<style scoped>


table.alt-table{
    font-size: 1.4rem;
    font-family: var(--main-font);
    text-align: left;
    background-color: white;
    box-shadow: var(--hb-box-shadow);
}

table.alt-table thead{
    border-bottom: 1px solid #E5E5E5;
    background: linear-gradient(45deg, var(--red) 0%, #FC2D2D 100%);
}

table.alt-table th{
    color: white !important;
    padding: 1rem;

}

table.alt-table tr{
}

table.alt-table td{
    padding: 1rem;

}

li.custom {
    list-style: none;
    display: flex;
}

li.custom p {
    display: flex;
}

li.custom p::before{
    content: url('../assets/images/icon.jpg');
    display: inline-block;
    width: 20px;
    height: 20px;
    margin-bottom: 1rem;
    clip-path: circle();
    margin-right: 10px;
}

</style>