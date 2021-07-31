<template>
  <div class="charts-overview">
    <h1 class="page-title">
      Visual -
      <span class="fw-semi-bold">Dashboard</span>
    </h1>
    <div>
      <b-row>
        <b-col xs="12" lg="12">
          <Widget
              title="<h5>Documentos por <span class='fw-semi-bold'>Area</span></h5>"
              close collapse customHeader
          >
            <apexchart type="bar" height="350" :series="loadChartSeries" :options="loadChartOptions"/>
          </Widget>

        </b-col>
        
        <!-- <b-col xs="12" lg="5">
          <Widget
              title="<h5>Documentos del <span class='fw-semi-bold'>Año anterior</span></h5>"
              close collapse customHeader
          >
            <echart :options="cd.echarts.line" :init-options="initEchartsOptions" style="height: 370px"></echart>
          </Widget>
        </b-col> -->
  
 
  
      </b-row>
        <!-- <b-row>
            <b-col xs="12" lg="12">
              <Widget
                  title="<h5>Estado de los <span class='fw-semi-bold'>Documentos</span></h5>"
                  close collapse customHeader
              >
                <apexchart type="pie" height="200" :series="cd.apex.pie.series" :options="cd.apex.pie.options"/>
              </Widget>
            </b-col>


          </b-row> -->
          <b-row>
            <b-col xs="12" lg="12">
              <Widget class="h-100 mb-0" title="Estado de los documentos" close>
              <highcharts :options="loadCircleOptions"></highcharts>
            </Widget>
            </b-col>


          </b-row>
    </div>
  </div>
</template>

<script>
import Widget from "@/components/Widget/Widget";
import {chartData, liveChart, liveChartInterval} from './mock';
import axios from 'axios';

import mock2 from './mock2';
import ECharts from 'vue-echarts/components/ECharts';
import 'echarts/lib/chart/pie';
import 'echarts/lib/chart/line';
import 'echarts/lib/chart/themeRiver';
import 'echarts/lib/component/tooltip';
import 'echarts/lib/component/legend';

import Highcharts from 'highcharts';
import exporting from 'highcharts/modules/exporting';
import exportData from 'highcharts/modules/export-data';

exporting(Highcharts);
exportData(Highcharts);

import { Chart } from 'highcharts-vue';
import Sparklines from '../../components/Sparklines/Sparklines'


import config from '../../config';
export default {
  name: "Reporte",
  components: { Widget, echart: ECharts, highcharts: Chart, Sparklines },
  data() {
    return {
      mock2,
      cd: chartData,
      ld: liveChart,
      initEchartsOptions: {
        renderer: 'canvas'
      },
      chart: [],
      circle: [],
      circleOptions:[],
      chartOptions: [],
      chartSeries: [],
    
    };
  },
  methods: {
    loadData(){
      axios.get(config.hostname+'expediente/dashboard_expediente')
      .then( (response) =>{
        // console.log(response);
        this.cd.apex.pie.series = [];
        this.cd.apex.pie.options.labels = [];
        this.cd.apex.column.series[0].data = [];
        this.cd.apex.column.options.xaxis.categories = [];
        response.data.barras.forEach(
          e=>{
            this.cd.apex.column.series[0].data.push(Number(e.n_cantidad_exp))
          }
        );
        response.data.barras.forEach(
          e=>{
           this.cd.apex.column.options.xaxis.categories.push(Number(e.c_nomb_area_trabajo))
          }
        );
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    load(){
      axios.get(config.hostname+'expediente/dashboard_expediente')
      .then( (response) =>{
        this.circleOptions = [];
        const seriesCount = response.data.circulo.length;
        const accessories = [];
        const value = [];

        response.data.circulo.forEach(e=>{
          accessories.push(e.c_nomb_estado_exp),
          value.push(Number(e.n_cantidad_exp))
        })

        for (let i = 0; i < seriesCount; i += 1) {
          this.circleOptions.push({
            label: accessories[i],
            data: value[i],
          });
        }
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadChart(){
      axios.get(config.hostname+'expediente/dashboard_expediente')
      .then( (response) =>{
        const categories = [];
        const series = [];
        response.data.barras.forEach(e=>{
          categories.push(e.c_nomb_area_trabajo)
        })
        response.data.barras.forEach(e=>{
          series.push(Number(e.n_cantidad_exp))
        })
        for (let i = 0; i < response.data.barras.length; i += 1) {
          this.chartOptions.push(categories[i]);
        }
        for (let i = 0; i < response.data.barras.length; i += 1) {
          this.chartSeries.push(series[i]);
        }
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    getRevenueData() {
      const data = [];
      const seriesCount = 3;
      const accessories = ['SMX', 'Direct', 'Networks'];

      for (let i = 0; i < seriesCount; i += 1) {
        data.push({
          label: accessories[i],
          data: Math.floor(Math.random() * 100) + 1,
        });
      }

      console.log(data);  
      return data;
    }
  },
  computed: {
  loadChartSeries(){
    return [{
        name: 'Cantidad',
        data: this.chartSeries
     
      }
    ]
       
  },
  loadChartOptions(){
    const colors = config.app.colors;
    const {inverse, info, primary, danger, warning, success, textColor} = colors;
    const chartColors = config.app.chartColors;
    const {axisColor} = chartColors;

    let columnColors = [inverse, primary, info, success, warning, danger, '#EC2A00', '#FF8C22'];
    let lineColors = [primary, success, warning];
    return {
      chart: {
          height: 350,
          type: 'bar',
        },
        colors: columnColors,
        plotOptions: {
          bar: {
            columnWidth: '45%',
            distributed: true
          }
        },
        dataLabels: {
          enabled: false,
        },
        xaxis: {
          categories: this.chartOptions,
          labels: {
            style: {
              colors: columnColors,
              fontSize: '14px'
            }
          },
          axisBorder: {
            show: false
          },
          axisTicks: {
            show: false
          }
        },
        yaxis: {
          labels: {
            style: {
              color: axisColor,
            }
          }
        },
    }
  },
  loadCircleOptions() {
        
        let revenueData = this.circleOptions;
        let {danger, info, primary} = this.appConfig.colors;
        let series = [
          {
            name: 'Cantidad',
            data: revenueData.map(s => {
              return {
                name: s.label,
                y: s.data
              }
            })
          }
        ];
        return {
          chart: {
            type: 'pie',
            height: 120
          },
          credits: {
            enabled: false
          },
          title: false,
          plotOptions: {
            pie: {
              dataLabels: {
                enabled: false
              },
              borderColor: null,
              showInLegend: true,
              innerSize: 60,
              size: 100,
              states: {
                hover: {
                  halo: {
                    size: 1
                  }
                }
              }
            }
          },
          colors: [danger, info, primary],
          legend: {
            align: 'right',
            verticalAlign: 'middle',
            layout: 'vertical',
            itemStyle: {
              color: '#495057',
              fontWeight: 100,
              fontFamily: 'Montserrat'
            },
            itemMarginBottom: 5,
            symbolRadius: 0
          },
          exporting: {
            enabled: false
          },
          series
        };
      }
  },
  created() {
    let user = JSON.parse(window.localStorage.getItem('user'));
    if (user.id_area_trabajo !=8) {
     this.$router.push('/app/partes');
    }
    this.load();
    this.loadChart();
    //this.loadData();
  },
  beforeDestroy() {
    clearInterval(liveChartInterval);
  },
};
</script>
