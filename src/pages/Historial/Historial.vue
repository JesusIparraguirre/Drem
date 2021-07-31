<template>
<div class="historial-page">
        <h2 class="page-title">Historial de - <span class="fw-semi-bold">expedientes</span></h2>
    <b-row>
      <b-col>
        <Widget
          customHeader
        >
        <b-row v-show="showTable">
          <b-col cols="10">
            <h5 class="page-title">Lista de - <span class="fw-semi-bold">Expedientes Eliminados</span></h5>
          </b-col>
          <b-col cols="2" v-show="expedientes.length != 0">
            <b-row>
              <b-col class="text-right pr-3 mt-2">
                <p>Cantidad:</p>
              </b-col>
              <b-col class="text-left pl-0">
                <b-form-group id="input-group-perpage"  label-for="input-perpage">
                <b-form-select
                  @change="loadPerPage(perpage)"
                  id="input-perpage"
                  v-model="perpage"
                  :options="optionsPerPage"
                  ></b-form-select>
                </b-form-group>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTable"></i>
        </div>
        <p class="text-center" v-if="expedientes.length == 0 && showTable">No hay registros</p>
        <div v-show="showTable">
          <div class="table-resposive" v-if="expedientes.length != 0">
            <table class="table table-hover" >
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <!--<th>Picture</th>-->
                  <th>Asunto</th>
                  <th class="hidden-sm-down">N° de Registro</th>
                  <th class="hidden-sm-down">Documento</th>
                  <th class="hidden-sm-down">Fecha de Creación</th>
                  <th class="hidden-sm-down">Fecha de baja</th>
                  <th class="hidden-sm-down">Estado</th>
                  <!-- <th class="hidden-sm-down">Activo</th> -->
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in expedientes" :key="row.id_expediente">
                  <td>{{row.id_expediente}}</td>
                  <td>{{row.c_asunto_exp}}</td>
                  <td>
                    <p class="mb-0">
                      <small>
                        <span class="fw-semi-bold">Codigo:</span>
                        <span class="text-muted">&nbsp; {{row.c_cod_exp}}</span>
                      </small>
                    </p>
                    <p>
                      <small>
                        <span class="fw-semi-bold">Serie:</span>
                        <span class="text-muted">&nbsp; {{row.id_tipo_doc_exp}}</span>
                      </small>
                    </p>
                  </td>
                  <td class="text-semi-muted">
                    <div style="width: 150px;white-space: normal;overflow: hidden;text-overflow: Ellipsis"
                     v-for="file in row.arc_exp" :key="file.id_archivos_exp">
                      <a target="_blank" :href="ip+'/public/'+file.c_archivo_exp">
                      {{file.c_archivo_exp}}</a>
                    </div>
                  </td>
                  <td class="text-semi-muted">
                    <span class="fw-semi-bold">{{formatDate(row.d_c_exp)}}</span>
                  </td>
                  <td class="text-semi-muted">
                    <span class="fw-semi-bold">{{formatDate(row.d_u_exp)}}</span>
                  </td>
                  <td class="width-150">
                    <b-badge variant="danger" pill>Eliminado</b-badge>
                  </td>
                  <!-- <td class="width-150" style="text-">
                    <b-form-checkbox  name="check-button" switch>
                    </b-form-checkbox>
                  </td> -->
                </tr>
              </tbody>
            </table>
          </div>

        </div>
        <!-- <div v-show="showRegister" >
             <b-form @submit="onSubmit" @reset="onReset" v-if="show">
                <b-form-group id="input-group-1" label="Nombre del documento:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="form.nameDoc"
                    type="text"
                    placeholder="Ingrese el nombre del documento"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Codigo del documento:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="form.codeDoc"
                    placeholder="Ingrese el codigo del documento"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-3" label="Fecha del documento:" label-for="input-3">
                  <div>
                    <date-picker v-model="form.timeDoc" valueType="format" placeholder="Seleccione una fecha"></date-picker>

                  </div>
                </b-form-group>
                <b-form-group id="input-group-4" label="Carga de documento:" label-for="input-4">
                  <div>
                    <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions"></vue-dropzone>
                  </div>
                </b-form-group>

                <b-button type="submit" variant="primary">Guardar</b-button>
                <b-button type="reset" variant="default">cancelar</b-button>

              </b-form>
            
        </div> -->
          
        <div v-show="expedientes.length != 0" class="mt-3">
          <!-- <h6 class="text-right">Pagi</h6> -->
          <b-pagination v-model="page" :total-rows="allRows" 
          :per-page="perpage" align="right" @input ="loadPage(page)" first-number
          last-number>
          </b-pagination>
        </div>
        </Widget>
      </b-col>
    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import axios from 'axios';

import config from '../../config'
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../components/Sparklines/Sparklines'

import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'
import moment from 'moment'




export default {
  name: 'Historial',
  components: {
    Widget,
    Sparklines,
    DatePicker,
    vueDropzone: vue2Dropzone
  },
  data() {
    return {
      page: 1,
      perpage: 10,
      allRows: 0,
      ip: config.ip,
      showTable: false,
      showRegister: false,
      loadingTable: true,

      expedientes: [],
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        maxFilesize: 0.5,
        headers: { "My-Awesome-Header": "header value" }
      },
      optionsPerPage: [
        {text: '10',value: 10},
        {text: '20', value: 20},
        {text: '30',value: 30}
      ],
      form: {
        nameDoc: null,
        codeDoc: null,
        timeDoc: null,
        checked: []
      },
      show: true, 

     
    };
  },
  methods: {
    formatDate(value){
      if (value) {
        return moment(String(value)).format('L')
      }
    },
    loadData(){
      axios.get(config.hostname+'expediente/obtener_expediente_area/7/1/'+this.perpage)
      .then( (response) =>{
        this.loadingTable = false;
        this.showTable = true;
        this.allRows = Number(response.data.total);
        this.expedientes = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPerPage(perpage){
      this.page = 1;
      axios.get(config.hostname+'expediente/obtener_expediente_area/7/1/'+perpage)
      .then( (response) =>{
        this.expedientes = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPage(page){
      axios.get(config.hostname+'expediente/obtener_expediente_area/7/'+page+'/'+this.perpage)
      .then( (response) =>{
        setTimeout(function(){document.body.scrollTop = 0; }, 50);
        this.expedientes = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    register(){
      // this.$router.push('/app/registerPartes');
      this.showTable = false;
      this.showRegister = true
    },

    onSubmit(event) {
      //event.preventDefault()
      // alert(JSON.stringify(this.form))
      this.showTable = true;
      this,this.showRegister = false;
    },
    onReset(event) {
      event.preventDefault()
      // Reset our form values
      this.form.nameDoc = ''
      this.form.codeDoc = ''
      this.form.timeDoc = null
     
      this.show = false
      this.$nextTick(() => {
        this.show = true,
        this.showTable = true,
        this.showRegister = false
      })
    },

    parseDate(date) {
      const dateSet = date.toDateString().split(' ');
      return `${date.toLocaleString('en-us', { month: 'long' })} ${dateSet[2]}, ${dateSet[3]}`;
    },
    checkAll(ev, checkbox) {
      const checkboxArr = (new Array(this[checkbox].length)).fill(ev.target.checked);
      Vue.set(this, checkbox, checkboxArr);
    },
    changeCheck(ev, checkbox, id) {
      this[checkbox][id] = ev.target.checked;
      if (!ev.target.checked) {
        this[checkbox][0] = false;
      }
    },
    getRandomData() {
      const result = [];

      for (let i = 0; i <= 8; i += 1) {
        result.push(Math.floor(Math.random() * 20) + 1);
      }

      return [{data: result}];
    },
    getRandomColor() {
      const {primary, success, info, danger} = this.appConfig.colors;
      const colors = [info, primary, danger, success];
      return {colors: [colors[Math.floor(Math.random() * colors.length)]]}
    }
  },
  computed: {
    
  },
  created() {
    this.loadData();
  }
};
</script>

<style src="./Historial.scss" lang="scss" scoped />