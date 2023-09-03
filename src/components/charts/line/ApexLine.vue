<template>
  <apexchart height="75%" type="line" :options="options" :series="series"></apexchart>
  <!-- {{ this.chartData }} -->
</template>

<script>
import { defineComponent } from 'vue'
// import { getCssVar } from 'quasar'
import axios from 'axios'

export default defineComponent({
  name: 'Apexline',
  data () {
    return {
      bankBalance: null,
      bankActivities: null,
      bankStats: null,
      totalValueBank: null,
      totalProfitBank: null,
      totalBalanceAmount: null,
      totalBalanceCurrent: null,
      totalBalanceWithdrawal: null,
      chartData: null,
      totalPNL: null,
      options: {
        // title: {
        //   text: 'Your Bank',
        //   align: 'left',
        //   style: {
        //     fontSize: '16px',
        //     fontWeight: 'bold',
        //     textDecoration: 'underline',
        //     fontFamily: undefined,
        //     color: 'white'
        //   }
        // },
        grid: {
          show: true,
          borderColor: '#1A4242', // Define a cor da borda das linhas de grade
          strokeDashArray: 1, // Isso faz com que as linhas de grade sejam sólidas. Ajuste conforme necessário.
          position: 'back', // Isso coloca a grade atrás da série/plots
          xaxis: {
            lines: {
              show: true
            }
          },
          yaxis: {
            lines: {
              show: true
            }
          }
        },
        dataLabels: {
          offsetX: -9,
          enabled: false,
          style: {
            colors: ['dark', '#E91E63', '#9C27B0']
          },
          background: {
            color: 'dark',
            borderRadius: 5
          },
          formatter: (val) => {
            return `$ ${parseFloat(val).toFixed(2)}` // Formata o valor para ter duas casas decimais e adiciona o '$' antes
          }
        },
        theme: {
          mode: 'dark',
          palette: 'palette10',
          monochrome: {
            enabled: false,
            color: '#00BCD4',
            shadeTo: 'dark',
            shadeIntensity: 0.65
          }
        },
        dropShadow: {
          enabled: true,
          top: 0,
          left: 0,
          blur: 3,
          opacity: 0.5
        },
        chart: {
          borderRadius: 10,
          id: 'apex-line',
          background: 'transparent',
          toolbar: {
            show: false
          }
        },
        colors: ['#00BCD4', '#66DA26', '#546E7A', '#E91E63', '#FF9800'],
        markers: {
          size: 1,
          hover: {
            sizeOffset: 1
          }
        },
        yaxis: {
          labels: {
            formatter: (val) => {
              return `$ ${parseFloat(val).toFixed(2)}` // Formata o valor para ter duas casas decimais e adiciona o '$' antes
            },
            style: {
              colors: 'cyan'
            }
          }
        },
        xaxis: {
          axisBorder: {
            show: true,
            color: '#1A4242', // Mude para a cor desejada
            offsetX: 0,
            offsetY: 0
          },
          labels: {
            style: {
              colors: 'transparent'

            }
          },
          style: {
            color: 'dark',
            background: '#00E396'
          }
          // categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998]
        },
        stroke: {
          curve: 'smooth'
        },
        tooltip: {
          enabled: true,
          enabledOnSeries: undefined,
          shared: true,
          followCursor: false,
          intersect: false,
          inverseOrder: false,
          custom: undefined,
          fillSeriesColor: false,
          theme: true,
          style: {
            fontSize: '12px',
            fontFamily: undefined,
            color: 'dark'
          },
          onDatasetHover: {
            highlightDataSeries: false
          },
          x: {
            show: true,
            format: 'dd MMM',
            formatter: undefined
          },
          y: {
            formatter: undefined,
            title: {
              formatter: (seriesName) => seriesName
            }
          },
          z: {
            formatter: undefined,
            title: 'Size: '
          },
          marker: {
            show: true
          },
          items: {
            display: 'flex'
          },
          fixed: {
            enabled: false,
            position: 'topRight',
            offsetX: 0,
            offsetY: 0
          }
        }
      }
      // series: [{
      //   name: 'series-1',
      //   data: this.chartData
      // }]
    }
  },
  methods: {
    // async fetchBankChart () {
    //   const authToken = localStorage.getItem('authToken')
    //   const userUuid = localStorage.getItem('AdminUUID')
    //   try {
    //     const response = await axios.get(`https://api.hunters.bet/profile/${userUuid}/bank`, {
    //       headers: {
    //         'Content-Type': 'application/json',
    //         Authorization: 'Bearer ' + authToken
    //       }
    //     })
    //     this.bankBalance = response.data.bank
    //     this.bankActivities = response.data.actions
    //     this.bankStats = response.data.stats
    //     this.totalValueBank = parseFloat(response.data.stats.initial_balance).toFixed(2)
    //     this.totalProfitBank = parseFloat(response.data.stats.PNL).toFixed(2)
    //     this.totalBalanceAmount = parseFloat(response.data.stats.accumulated_balance).toFixed(2)
    //     this.totalBalanceCurrent = parseFloat(response.data.stats.current_balance).toFixed(2)
    //     this.totalBalanceWithdrawal = parseFloat(response.data.stats.total_withdrawal).toFixed(2)
    //     this.totalPNL = parseFloat(response.data.stats['%PNL']).toFixed(2)
    //     this.chartData = response.data.chart
    //     console.log('chart data', this.chartData)

    //     console.log('bank', response.data)
    //   } catch (e) {
    //     console.error(e)
    //   }
    // }
  },
  computed: {
    series () {
      return [{
        name: 'chart',
        data: [0, 55, 33, 44, 66, 100, 123, 342]
      }]
    }
  },
  mounted () {
    // this.fetchBankChart()
  }
})
</script>
