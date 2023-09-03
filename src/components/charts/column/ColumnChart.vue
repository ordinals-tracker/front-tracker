<template>
  <div class="example">
    <apexchart width="500" height="500" type="bar" :options="chartOptions" :series="series"></apexchart>
  </div>
</template>

<script>
export default {
  name: 'MixedExample',
  data: function () {
    return {
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
            isDumbbell: true,
            horizontal: false,
            endingShape: 'rounded',
            columnWidth: ['70%'],
            borderRadius: 3,
          },
        },
        dataLabels: {
          enabled: false
        },
        stroke: {
          show: false,
          width: 2,
          colors: ['cyan']
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
          show: false, // Desativar todas as linhas de grade
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
          y: {
            formatter: function (val) {
              return val + " $ BTC"
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
      series: [
        {
          name: 'Mint',
          data: [0.05, 0.06, 0.077, 0.1, 0.12, 0.11, 0.13, 0.14, 0.14],
          // bar: {
          //   borderRadius: [5, 5, 5, 5]
          // }
        },
        {
          name: 'Buy',
          data: [0.03, 0.02, 0.087, 0.12, 0.02, 0.056, 0.13, 0.14, 0.14],
          bar: {
            borderRadius: 5,
            horizontal: false,
          }
        },
        {
          name: 'Sell',
          data: [-0.044, -0.043, -0.054, -0.074, -0.044, -0.07, -0.044, -0.044, -0.044],
          bar: {
            borderRadius: 2
          }
        }
      ]
    }
  }
}
</script>

<style></style>
