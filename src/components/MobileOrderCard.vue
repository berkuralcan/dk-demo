<script setup>

import { defineProps } from 'vue';

const props = defineProps(["order"])

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


</script>


<template>
   <details class="bg-white mt-8 w-full p-4 flex mb-4 shadow shadow-lg cursor-pointer">
    
    
    <summary class="text-2xl">
            <div class="flex items-center justify-between mb-10">
                <div class="flex">
                    <img class="rounded-full mr-4" :src="require(`../assets/images/icon.jpg`)">
                    <h4  class="text-2xl capitalize font-bold">{{props.order.customer.customerName}}</h4>

                </div>
                <div class="date flex items-center">
                    <p class="text-2xl text-gray-500 ml-1">{{formatDate(props.order.insertDate)}}</p>
                </div>
            </div>
        <div class="amount w-full text-right text-3xl">₺{{props.order.orderTotal }}</div>
        <div class="provider-container">
            <img v-if="props.order.externalAppName === 'YemekSepeti'" class="" :src="require(`../assets/images/ys.png`)">
            <img v-else-if="props.order.externalAppName === 'Getir Yemek'" class="" :src="require(`../assets/images/getir.png`)">
            <img v-else-if="props.order.externalAppName === 'Trendyol'" class="" :src="require(`../assets/images/ty.png`)">
            <img v-else-if="props.order.externalAppName === 'Restajet'" class="" :src="require(`../assets/images/restajet.png`)">
        </div>
    </summary>




    <div class="border-t mt-6 border-gray-200">
        <div class="mt-6">
            <strong class="text-2xl text-bold underline">Ürünler</strong>
            <ul class="text-2xl mt-8">
                        <li class="!capitalize custom" v-for="product in order.products" :key="product.id">
                            <p class="" v-if="!product.productName.includes('İstemiyorum')">
                                {{ product.productName.toLocaleLowerCase("tr-TR") }}
                            </p>
                        </li> 
                    
                    </ul>
        </div>
        <div class="mt-4 text-2xl  flex">
            <span>
                <strong>Ödeme Yöntemi: </strong>
            </span>
            <span class=""> {{props.order.payments[0].paymentName}} </span>
        </div>
        
    </div>
</details> 

</template>

<style scoped>

details {
    border-radius: var(--st-border-radius);
    box-shadow: var(--hb-box-shadow);
    font-family: "Inter", sans-serif;
}
details summary {
    list-style: none;
    position: relative;

}

.provider-container{
    position: absolute;
    bottom: 0;
    left: 45%;
    opacity: 20%;
    overflow: hidden;
}

.provider-container img{
    width: 100px;
    object-fit: fill;
}
</style>
