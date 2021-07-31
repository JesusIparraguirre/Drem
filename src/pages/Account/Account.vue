<template>
<div class="log-page">
        <h2 class="page-title">Cuenta <span class="fw-semi-bold"></span></h2>
    <b-row>
      <b-col>
        <Widget
          title="<h5>Datos de <span class='fw-semi-bold'>Usuario</span></h5>"
          customHeader
        >
        
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingData"></i>
        </div>
        <div class="ml-5 mr-5 text-center" v-show="!loadingData">
            <b-form >
                <b-row class="mt-3">
                  <b-col cols=6>
                    <span class='fw-semi-bold'>Documento de identidad</span>
                    <br>{{user.c_dni_personal}}
                  </b-col>
                  <b-col cols=6>
                    <span class='fw-semi-bold'>Nombre</span>
                    <br>{{user.c_nombres_personal}}
                  </b-col>
                </b-row>         
                <b-row class="mt-3">
                  <b-col cols=6>
                    <span class='fw-semi-bold'>Apellido paterno</span>
                    <br>{{userData.c_appat_personal}}
                  </b-col>
                  <b-col cols=6>
                    <span class='fw-semi-bold'>Apellido materno</span>
                    <br>{{user.c_apmat_personal}}
                  </b-col>
                </b-row>         
                <b-row class="mt-3">
                <b-col cols=6>
                  <span class='fw-semi-bold'>Correo electrónico</span>
                  <br>{{user.c_usuario}}
                </b-col>
                <b-col cols=6>
                  <span class='fw-semi-bold'>Celular</span>
                  <br>{{userData.c_celular_personal}}
                </b-col>
              </b-row>
              <b-row class="mt-3">
                <b-col cols=6>
                  <span class='fw-semi-bold'>Area de trabajo</span>
                  <br>{{user.c_nomb_area_trabajo}}
                </b-col>
                <b-col cols=6>
                  <span class='fw-semi-bold'>Profesión</span>
                  <br>{{profesion}}
                </b-col>
              </b-row>
              <b-row class="mt-3">
                <b-col cols=6>
                  <span class='fw-semi-bold'>Cargo</span>
                  <br>{{cargo}}
                </b-col>
                <b-col cols=6>
                  <span class='fw-semi-bold'>Modalidad de contrato</span>
                  <br>{{modalidad}}
                </b-col>
              </b-row>
              <br>         
              <!-- <b-row class="mt-3">
                <b-col cols=6>
                  <span class='fw-semi-bold'>Cargo</span>
                  <br>Gerente
                </b-col>
                <b-col cols=6>
                  <span class='fw-semi-bold'>Área de trabajo</span>
                  <br>Gerente
                </b-col>
              </b-row>  
              <b-row class="mt-3">
                <b-col cols=6>
                  <span class='fw-semi-bold'>Modalidad de contrato</span>
                  <br>Gerente
                </b-col>
                <b-col cols=6>
                  <span class='fw-semi-bold'>Profesión</span>
                  <br>Gerente
                </b-col>
              </b-row>   -->

             
            </b-form>
        </div>
          
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
  name: 'Account',
  components: {
    Widget,
    Sparklines,
  },
  data() {
    return {
      data:[],
      loadingData: true,
      user: {},
      userData: {},
      profesion:'',
      cargo: '',
      modalidad: '',
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
        this.userData = response.data.data[0];
        this.loadProfessions(response.data.data[0].id_profesion);
        this.loadPositions(response.data.data[0].id_cargo);
        this.loadContracts(response.data.data[0].id_modalidad_contrato);
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadProfessions(id){
      axios.get(config.hostname+'maestro/obtener_profesion')
      .then( (response) =>{
        this.professions = response.data.data;
        this.professions.forEach( (element) =>{
        if(element.id_profesion == id){
            this.profesion = element.c_nomb_profesion
            }
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPositions(id){
      axios.get(config.hostname+'maestro/obtener_cargos')
      .then( (response) =>{
        this.positions = response.data.data;
        this.positions.forEach( (element) =>{
        if(element.id_cargo == id){
            this.cargo = element.c_nombres_cargo
            }
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadContracts(id){
      axios.get(config.hostname+'maestro/obtener_modalidad_contratado')
      .then( (response) =>{
        this.contracts = response.data.data;
        this.contracts.forEach( (element) =>{
        if(element.id_modalidad_contrato == id){
            this.modalidad = element.c_nomb_modalidad_contrato
            }
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
    this.user = JSON.parse(window.localStorage.getItem('user'));
    this.loadData(this.user.c_dni_personal);
  }
};
</script>

<style src="./Account.scss" lang="scss" scoped />