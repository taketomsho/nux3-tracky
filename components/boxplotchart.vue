<script setup>
import { createTypedChart } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale,LineElement,PointElement,RadialLinearScale  } from 'chart.js'
import {
  BoxPlotController,
  BoxAndWiskers,
} from "@sgratzl/chartjs-chart-boxplot";

ChartJS.register(
  Title, 
  Tooltip, 
  Legend,
  LineElement,
  PointElement, 
  BarElement, 
  CategoryScale, 
  LinearScale,
  RadialLinearScale,
  BoxPlotController,
  BoxAndWiskers,
)

const BoxPlot = createTypedChart('boxplot', BoxPlotController)

const props = defineProps({
  boxdata: Array,
});

const chartData = {
  labels: ['toto'],
    datasets: [
      {
        type: 'boxplot',
        data: [props.boxdata],
      },
    ],
}

const chartOptions = {
  responsive: true,
  indexAxis: 'y',
  legend: {
    position: 'top',
  },
  title: {
    display: true,
    text: 'Chart.js Box Plot Chart',
  },
}

</script>

<template>
  <div>
    <BoxPlot
      id="BoxPlot"
      :options="chartOptions"
      :data="chartData"
    />
  </div>
</template>