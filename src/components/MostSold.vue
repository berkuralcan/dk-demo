<script setup>
import { onBeforeMount, reactive, defineProps } from "vue"
import VueApexChart from "vue3-apexcharts";


const props = defineProps(["orders"])

const calculateMostSold = function(orders){

    const productOccurrences = {};
    for (const item of orders) {
        for (const product of item.products){
            const productName = product.productName.toLocaleLowerCase("tr-TR");
                if(!product.productName.includes("İstemiyorum")){
                        if (productName in productOccurrences) {
                        productOccurrences[productName]++;
                    } else {
                        productOccurrences[productName] = 1;
                    }
                    }
                }

        }
    
     // Sort the object by value in descending order
        const sortedProductOccurrences = Object.fromEntries(
            Object.entries(productOccurrences).sort(([,a],[,b]) => b-a)
        );

        // Get the first 6 items

        const firstTen = Object.keys(sortedProductOccurrences).slice(0, 6);
        const firstTenValues = Object.values(sortedProductOccurrences).slice(0, 6);

        chartOptions.xaxis.categories = firstTen;
        chartSeries[0].data = firstTenValues;
        
}

const chartOptions = reactive({
    chart: {
        type: 'bar',
        width: "100%",
        offsetX: -20,
        toolbar: {
        show: false,

    }
    },
    legend: {
        show: false
    },
    plotOptions: {
        bar: {
            horizontal: true,
            distributed: true,
        }
    },
    dataLabels: {
        enabled: false
    },
   fill: {
        colors: ['#DA291B', '#FDC62E']
    },
    tooltip: {
        enabled: false
    },
    xaxis: {
        categories: [],
      },

      yaxis: {
        labels: {
            align: "left",
            style: {
                fontSize: "13px",
                fontFamily: "Inter, sans-serif",

            }
        }
      }

})

const chartSeries = reactive([{
    name: 'series-1',
    data: [44, 55, 41, 64, 22, 43, 21]
}])

onBeforeMount(()=> {
    calculateMostSold(props.orders)
})

</script>

<template>
    <div class="most-sold-container">
        <div class="header">
            <h3>Son 100 Sipariş - En Çok Satılan Ürünler</h3>
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