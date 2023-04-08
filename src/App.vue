<script setup>

import "./assets/styles/backgrounds.css"
import "./assets/styles/resets.css"
import "./assets/styles/variables.css"

import MainBox from "./components/MainBox.vue"
import AcTable from "./components/AcTable.vue"
import MostSold from "./components/MostSold.vue"
import OrderSources from "./components/OrderSources.vue"
import AcHeader from "./components/AcHeader.vue"
import MobileOrderCard from "./components/MobileOrderCard.vue"

import { onBeforeMount, ref, computed } from 'vue';

import axios from "axios"


const activeOrders = ref(0)


const getData = async function(){

    // get the date of 10 days ago
    const date = new Date();
    date.setDate(date.getDate() - 10);

    // convert the date to this format: yyyy-MM-dd HH:mm:ss
    const dateStr = date.toISOString().split('T')[0] + " 00:00:00";

    const params = {
        "startDate": dateStr,
        "page": 4,
    }

    const url = "https://ext.adisyo.com/api/External/v2/CompletedOrders"
    const headers = {
        "x-api-consumer": process.env.VUE_APP_CONSUMER,
        "x-api-key": process.env.VUE_APP_KEY,
        "x-api-secret": process.env.VUE_APP_SECRET
    }



    const response = await axios.get(url, {headers: headers, params: params}).catch(() => {
        alert("Geçmiş siparişleri alırken rate limit'e takıldık.")
    })

    data.value = response.data
}

const getActiveOrders = async function(){
    const url = "https://ext.adisyo.com/api/External/v2/RecentOrders"
    const headers = {
        "x-api-consumer": process.env.VUE_APP_CONSUMER,
        "x-api-key": process.env.VUE_APP_KEY,
        "x-api-secret": process.env.VUE_APP_SECRET
    }

    const response = await axios.get(url, {headers: headers}).catch(() => {
      alert("Aktif siparişleri alırken rate limit'e takıldık.")
    })


    activeOrders.value = response.data.totalCount
}

onBeforeMount(async () => {
  await getActiveOrders()
  await getData()
})


const data = ref()

const todaysOrders = computed( () => {
    return data.value.orders.filter((order) => {
      return order.insertDate.includes(new Date().toISOString().split('T')[0]);
    });
}  )

const getTotalAmountOfTodaysOrders = () => {
  let totalAmount = 0;
  todaysOrders.value.forEach((order) => {
    totalAmount += order.orderTotal;
  });

  totalAmount = totalAmount.toLocaleString("tr-TR")

  return `${totalAmount} ₺`;
};

const getTotalOfAllORders = () => {
  let totalAmount = 0;
  data.value.orders.forEach((order) => {
    totalAmount += order.orderTotal;
  });

  totalAmount = totalAmount.toLocaleString("tr-TR")

  return `${totalAmount} ₺`;
};

const isMobile = function(){
    return window.innerWidth < 850
}


</script>

<template>
  <div class="global-container">

    <ac-header></ac-header>
    <div class="main-container mt-20">
      <MainBox class="col-start-1 col-end-2 card-red" icon="1" title="Aktif Sipariş" :data="activeOrders" />
      <MainBox class="col-start-2 col-end-3 card-yellow" icon="2" title="Son 10 Gün Sipariş" :data="data.totalCount" />
      <MainBox class="col-start-1 col-end-2 card-yellow" icon="3" title="Bugün Toplam" :data="getTotalAmountOfTodaysOrders()" />
      <MainBox class="col-start-2 col-end-3 card-red" icon="4" title="Son 100 Sipariş" :data="getTotalOfAllORders()" />
      <OrderSources class="col-start-3 col-end-5 row-start-1 row-end-3" :orders="data.orders" ></OrderSources>
      <MostSold :orders="data.orders" class="row-start-1 row-end-3 col-start-5 col-end-7"/>
      <div class="table-container col-start-1 col-end-7">
        <h2 class="subtitle"> Son Siparişler </h2>
        <AcTable v-if="!isMobile()" :orders="data.orders" class=""></AcTable>
        <mobile-order-card v-else v-for="order in data.orders" :key="order.id" :order="order"></mobile-order-card>
      </div>
    </div>

  </div>

</template>


<style scoped>

h2.subtitle{
  font-size: 2.4rem;
  color: rgb(67, 67, 67);
  margin: 2rem 0;
}

</style>