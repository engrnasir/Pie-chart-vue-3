<template>
    <div>
      <h1>{{ title }}</h1>
      <canvas ref="chartRef"></canvas>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import axios from 'axios'
  import Chart from 'chart.js/auto'

  const props = defineProps(['title', 'labels', 'data'])
  

      const chartRef = ref(null)
      const chartData = ref({
        labels: ['hello'],
        data: ['100'],
        colors: ["#FF5733", "#C70039", "#900C3F", "#581845", "#8E44AD", "#5B2C6F", "#4A235A", "#1B4F72", "#0E6251", "#117A65", "#196F3D", "#1E8449", "#DC7633", "#F7DC6F", "#E74C3C", "#EC7063", "#F1948A", "#C0392B", "#922B21", "#7B241C","#641E16", "#F1C40F", "#F7DC6F", "#F0B27A", "#F5B041"],
        hoverOffset: 4,
      })
    
      const createChart = () => {
        const ctx = chartRef.value.getContext('2d')
        new Chart(ctx, {
          type: 'pie',
          data: {
              labels: chartData.value.labels,
                datasets: [{
                    backgroundColor: chartData.value.colors,
                    data: chartData.value.data,
                }]
        },
        options:{
            plugins:{
                legend:{
                    position:'bottom'
                }
            }
          }
        })
      }
      onMounted(()=>{
        async function fetchdata(){    
        const options = {
        method: 'GET',
        url: 'https://football-prediction-api.p.rapidapi.com/api/v2/predictions',
        params: {market: 'classic', iso_date: '2018-12-01', federation: 'UEFA'},
        headers: {
            'X-RapidAPI-Key': '74e62bcf0amsh761de93bfc1b01ep1cef5ejsne5bc2d5b1103',
            'X-RapidAPI-Host': 'football-prediction-api.p.rapidapi.com'
        }
        };

        axios.request(options).then(function (response) {
            let res = response.data.data
            let won=0, lost=0
            res.forEach(el => {
                if(el.status==='won'){
                    won++

                }
                if(el.status==='lost'){
                    lost++
                }
            });
            chartData.value.labels = ['won', 'lost']
            chartData.value.data = [won, lost]
            createChart()
        }).catch(function (error) {
            console.error(error);
        });
        }

        fetchdata()
        
      })
  
  </script>
  
  <style>
  h1{
    text-align: center;
    color: #000;
  }
  canvas {
    width: 100%;
    height: 100%;
  }
  </style>
  