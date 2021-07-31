<template>
<div class="log-page">
        <h2 class="page-title">Log <span class="fw-semi-bold"></span></h2>
    <b-row>
      <b-col>
        <Widget
          title="<h5>Historial de <span class='fw-semi-bold'>Log</span></h5>"
          customHeader
        >
        
        <table class="table table-hover" v-if="data.length != 0">
              <thead>
                <tr>
                  <!-- <th class="hidden-sm-down">#</th>
                  <th>Asunto</th> -->
                  <th class="hidden-sm-down">#</th>
                  <th class="hidden-sm-down">Nombre log</th>
                  <th class="hidden-sm-down">Error log</th>
                  <th class="hidden-sm-down">Usuario</th>
                  <th class="hidden-sm-down">Fecha y hora</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in data" :key="row.id_log">
                  <td>{{row.id_log}}</td>
                  <td class="width-150">
                    {{row.c_nombre_log}}
                  </td>
                  <td>
                    <div style="width: 150px;white-space: normal;overflow: hidden;text-overflow: Ellipsis">
                      {{row.c_error_log}}
                    </div>
                  </td>
                  <td>
                    {{row.c_usuario}}
                  </td>
                  <td>
                    {{row.fecha}} {{row.hora}}
                  </td>
                </tr>
              </tbody>
        </table>
          
        </Widget>
      </b-col>
    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import config from '../../config';
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../components/Sparklines/Sparklines'

import VueTextareaAutosize from "vue-textarea-autosize";
Vue.use(VueTextareaAutosize);




export default {
  name: 'Log',
  components: {
    Widget,
    Sparklines,
  },
  data() {
    return {
      ip:config.ip,
      data:[],
      loadingData: true,
      user: {},
      contracts: [],
      positions: [],
      professions: [],
    };
  },
  methods: {
    loadData(dni){
      axios.get(config.hostname+'personal/obtener_personal/'+dni)
      .then( (response) =>{
        this.loadingData = false;
        this.user = response.data.data[0];
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadProfessions(){
      axios.get(config.hostname+'maestro/obtener_profesion')
      .then( (response) =>{
        console.log(response);
        this.professions = response.data.data;
        this.professions.forEach(function (element) {
          element.text = element.c_nomb_profesion;
          element.value = element.id_profesion;
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPositions(){
      axios.get(config.hostname+'maestro/obtener_cargos')
      .then( (response) =>{
        console.log(response);
        this.positions = response.data.data;
        this.positions.forEach(function (element) {
          element.text = element.c_nombres_cargo;
          element.value = element.id_cargo;
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadContracts(){
      axios.get(config.hostname+'maestro/obtener_modalidad_contratado')
      .then( (response) =>{
        console.log(response);
        this.contracts = response.data.data;
        this.contracts.forEach(function (element) {
          element.text = element.c_nomb_modalidad_contrato;
          element.value = element.id_modalidad_contrato;
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    },

  },
  computed: {
    
  },
  created(){
    // let data = window.localStorage.getItem('user');
    // let data2 = JSON.parse(data);
    // this.loadData(data2[0].c_dni_personal);
    let user = JSON.parse(window.localStorage.getItem('user'));
    if (user.id_area_trabajo !=8) {
     this.$router.push('/app/partes');
    }
    axios.get(config.hostname+'eventos/logs_todos')
      .then( (response) =>{
        console.log(response);
        this.data = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
  }
};
</script>

<style src="./Log.scss" lang="scss" scoped />