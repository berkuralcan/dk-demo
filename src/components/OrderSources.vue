<script setup>
import { onBeforeMount, reactive, ref, defineProps } from "vue"
import VueApexChart from "vue3-apexcharts";


const props = defineProps(["orders"])

const checkOrderSources = function(orders){

    const sources = {};
    for (const item of orders) {
        
        const sourceName = item.externalAppName;
            if (sourceName in sources) {
            sources[sourceName]++;
        } else {
            sources[sourceName] = 1;
        }
        }

        // Sort the sources object alphabetically by keys

        const sortedSources = Object.fromEntries(
            Object.entries(sources).sort()
        );

        chartOptions.labels = Object.keys(sortedSources);


        chartSeries.value = Object.values(sortedSources);

        console.log(sources)
     
}

const chartOptions = reactive({
    chart: {
        type: 'pie',
        toolbar: {
            show: false,
        },
    },

    legend:{
        position: "bottom",
        fontSize: "14px",
        markers: {
            width: 14,
            height: 14,
        }
    },

    dataLabels: {
        enabled: false
    },

    colors: [],

    labels: [],

})

const determineColors = function(){
    if(chartSeries.value.length === 3){
        chartOptions.colors = ["#4c3398", "#F27A15", '#fa0050']
    } else {
        chartOptions.colors = ["#4c3398", "#2EA3F2", "#F27A15", '#fa0050']
    }
}

const chartSeries = ref([])

onBeforeMount(()=> {
    checkOrderSources(props.orders)
    determineColors()
})

</script>

<template>
    <div class="most-sold-container">
        <div class="header">
            <h3>Son 100 Sipariş - Sipariş Kanalları</h3>
        </div>
        <div class="chart-container">
            <VueApexChart height="100%" :options="chartOptions" :series="chartSeries" />
        </div>
    </div>
</template>

<style>
    .most-sold-container{
        background-color: white;
        border: 1px solid #E5E7EB;
        border-radius: var(--st-border-radius);
        box-shadow: var(--hb-box-shadow);
        padding: .3rem .5rem .5rem .5rem;
        color: white;
    }

    .header{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        padding: 1rem;
        font-size: 1.6rem;
        text-transform: uppercase;
        border-bottom: 1px solid #E5E7EB;
        color: black;
    }

    .chart-container{
        height: 30vh;
    }

    .apexcharts-yaxis{
        text-transform: capitalize;
    }
</style>