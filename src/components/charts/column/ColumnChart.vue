<template>
  <div v-if="!loadedDataChart" class="example column flex full-width flex justify-center items-center">
    <div class="filters gt-sm col">
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1w' ? 'orange': 'white'" cl @click="filterData('1w')">1 Week</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1m' ? 'orange': 'white'" cl @click="filterData('1m')">1 Month</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '3m' ? 'orange': 'white'" cl @click="filterData('3m')">3 Months</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '6m' ? 'orange': 'white'" cl @click="filterData('6m')">6 Months</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1y' ? 'orange': 'white'" cl @click="filterData('1y')">1 Year</q-btn>
    </div>
    <div style="white-space: nowrap;" class="full-width flex row filters lt-md col">
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1w' ? 'orange': 'white'" cl @click="filterData('1w')">1 Week</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1m' ? 'orange': 'white'" cl @click="filterData('1m')">1 Month</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '3m' ? 'orange': 'white'" cl @click="filterData('3m')">3 Months</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '6m' ? 'orange': 'white'" cl @click="filterData('6m')">6 Months</q-btn>
      <q-btn flat dense class="q-mx-sm q-mb-md text-bold" :color="this.periodChange == '1y' ? 'orange': 'white'" cl @click="filterData('1y')">1 Year</q-btn>
    </div>
    <apexchart class="gt-sm col"  width="500" height="500" type="bar" :options="chartOptions" :series="series"></apexchart>
    <apexchart class="lt-md col"  width="300" height="300" type="bar" :options="chartOptions" :series="series"></apexchart>
  </div>
  <div v-else class="justify-center items-center flex flex-center full-width">
    <q-spinner color="orange" ></q-spinner>
  </div>
</template>


<script>
import axios from 'axios'

export default {
  name: 'MixedExample',
  data: function () {
    return {
      dataChartMint: [],
      dataChartSell: [],
      dataChartBuy: [],
      dataChartDates: [],
      originalDataChartMint: [],
      originalDataChartBuy: [],
      originalDataChartSell: [],
      originalDataChartDates: [],
      periodChange: '',
      loadedDataChart: true,
      chartOptions: {
        chart: {
          type: 'bar',
          height: 350,
          stacked: true,
          toolbar: {
            show: false
          },
          foreColor: '#fff',
        },
        legend: {
          style: {
            paddingtOP: 20
          },
          show: true
        },
        plotOptions: {
          bar: {
            // isDumbbell: true,
            horizontal: false,
            endingShape: 'rounded',
            columnWidth: 17,
            // borderRadius: 3,
          },
        },
        dataLabels: {
          enabled: false
        },
        stroke: {
          show: false,
        },
        xaxis: {
          labels: {
            show: false
          },
          axisBorder: {
            show: false
          },
          axisTicks: {
            show: false
          }
        },
        yaxis: {
          forceNiceScale: true,
          labels: {
            show: false
          },
          axisBorder: {
            show: false
          }
        },
        grid: {
          show: false,
        },
        fill: {
          opacity: 1,
          colors: ['#0E579B', '#329239', '#F7931A']
        },
        tooltip: {
          theme: 'dark',
          style: {
            fontSize: '12px',
            fontFamily: 'Arial, sans-serif',
            colors: ['#fff']
          },
          x: {
            formatter: function (val, { dataPointIndex }) {
              return this.dataChartDates[dataPointIndex]
            }.bind(this)
          },
          y: {
            formatter: function (val) {
              return val.toString().replace('-', '+') + " $BTC"
            }
          }
        },
        annotations: {
          yaxis: [{
            y: 0,
            borderColor: '#fff',
            strokeWidth: 5,
            label: {
              show: false
            }
          }]
        }
      },
      // series: [
      //   {
      //     name: 'Mint',
      //     data: [0.05, 0.06, 0.077, 0.1, 0.12, 0.11, 0.13, 0.14, 0.14],
      //     // bar: {
      //     //   borderRadius: [5, 5, 5, 5]
      //     // }
      //   },
      //   {
      //     name: 'Buy',
      //     data: [0.03, 0.02, 0.087, 0.12, 0.02, 0.056, 0.13, 0.14, 0.14],
      //     bar: {
      //       borderRadius: 5,
      //       horizontal: false,
      //     }
      //   },
      //   {
      //     name: 'Sell',
      //     data: [-0.044, -0.043, -0.054, -0.074, -0.044, -0.07, -0.044, -0.044, -0.044],
      //     bar: {
      //       borderRadius: 2
      //     }
      //   }
      // ]
    }
  },
  methods: {
    filterData(period) {
      this.periodChange = period
      let length;
      switch (period) {
        case '1w':
          length = 7; // 7 dias
          break;
        case '1m':
          length = 30; // Aproximadamente 30 dias
          break;
        case '3m':
          length = 90;
          break;
        case '6m':
          length = 180;
          break;
        case '1y':
          length = 365;
          break;
        default:
          length = this.originalDataChartDates.length;
      }

      this.dataChartMint = this.originalDataChartMint.slice(-length);
      this.dataChartBuy = this.originalDataChartBuy.slice(-length);
      this.dataChartSell = this.originalDataChartSell.slice(-length);
      this.dataChartDates = this.originalDataChartDates.slice(-length);
    },
    async getTrackedData() {
      const wallets = this.$route.params.id
      try {
        const response = await axios.get(`https://api.ordinalstracker.io/track/${wallets}`, {
          headers: {
            'Content-Type': 'application/json'
          }
        })
        this.originalDataChartMint = response.data['ordinals-track'].chart[0]['Mint']
        this.originalDataChartBuy = response.data['ordinals-track'].chart[1]['Buy']
        this.originalDataChartSell = response.data['ordinals-track'].chart[2]['Sell']
        this.originalDataChartDates = response.data['ordinals-track'].chart[3]['Dates']
        this.filterData('6m')
        this.loadedDataChart = false
        console.log('chart dates', this.dataChartDates)

      } catch (e) {
        console.error(e)
      }
    },
  },
  mounted() {
    this.getTrackedData()
  },
  computed: {
    series() {
      return [
        {
          name: 'Mint',
          data: this.dataChartMint
        },
        {
          name: 'Buy',
          data: this.dataChartBuy
        },
        {
          name: 'Sell',
          data: this.dataChartSell
        },
      ]
    }
  }
}
</script>

<style></style>
