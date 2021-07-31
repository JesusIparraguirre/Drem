<template>
<div class="hidrocarburo-page">
    <b-row>
    <b-col cols="8">
      <h2 class="page-title">Area - <span class="fw-semi-bold">Legal</span></h2>
    </b-col>
    <b-col cols="4">
       <b-form class="d-sm-down-none ml-5" >
        <b-form-group>
          <b-input-group >
            <template v-slot:prepend>
              <b-button class="btn-buscar" @click="searchData"> 
                <b-input-group-text class="bordes"><i class='fi flaticon-search-2'/></b-input-group-text>
              </b-button>
            </template>
             <b-form-select 
              id="inline-form-custom-select-pref"
              class="sm"
              :options="[
              {text: 'Buscar por :', value: null},
              { text: 'Asunto', value: 'asunto' }, 
              {text: 'Codigo',value: 'exp'},
              {text: 'Todo',value: 'todo'}]"
              v-model="typeInput"
              :value="null"
            ></b-form-select> 
            <b-form-input id="search-input" placeholder="Buscar..." v-model="searchInput" v-if="typeInput!='todo'" />
          </b-input-group>
        </b-form-group>
      </b-form>
    </b-col>
  </b-row>
    <b-row>
      <b-col>
        <Widget v-show="showList"
          customHeader
        >
        <b-row v-show="showTable">
          <b-col cols="10">
            <h5 class="page-title">Lista de - <span class="fw-semi-bold">Expedientes</span></h5>
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
          <p class="text-center" v-if="expedientes.length == 0">No hay registros</p>
          <div v-show="showTable" class="table-resposive table-hover">
            <table class="table" v-if="expedientes.length != 0">
              <thead>
                <tr>
                 <th class="hidden-sm-down">#</th>
                  <th>Asunto</th>
                  <th class="hidden-sm-down">N° de Registro</th>
                  <th class="hidden-sm-down">Fecha</th>
                  <th class="hidden-sm-down">Documento</th>
                  <th class="hidden-sm-down">Documentos Aprobado</th>
                  <th class="hidden-sm-down">Estado</th>
                  <th class="hidden-sm-down" v-if="user.id_area_trabajo == 8 || user.id_area_trabajo == 5">Completar</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in expedientes" :key="row.id_expediente" >
                  <td>{{row.id_expediente}}</td>
                  <td>{{row.c_asunto_exp}}</td>
                  <td>
                    <p class="mb-0">
                      <small>
                        <span class="fw-semi-bold">Código:</span>
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
                    {{formatDate(row.d_tramite_exp)}}
                  </td>
                  <td class="text-semi-muted">
                    <div style="width: 150px;white-space: normal;overflow: hidden;text-overflow: Ellipsis" v-for="file in row.arc_exp" :key="file.id_archivos_exp">
                        <a target="_blank" :href="ip+'/public/'+file.c_archivo_exp">
                        {{file.c_archivo_exp}}</a>
                      </div>
                  </td>
                  <td class="text-semi-muted">
                    <div style="width: 150px;white-space: normal;overflow: hidden;text-overflow: Ellipsis" v-for="file in row.arc_exp_aprob" :key="file.id_archivos_aprob_exp">
                        <a target="_blank" :href="ip+'/public/'+file.c_informe_tecnico">
                        {{file.c_informe_tecnico}}</a><br>
                        <a target="_blank" :href="ip+'/public/'+file.c_autodirectoral">
                        {{file.c_autodirectoral}}</a><br>
                        <a target="_blank" :href="ip+'/public/'+file.c_decla_impac_amb">
                        {{file.c_decla_impac_amb}}</a>
                      </div>
                  </td>
                  <td>
                    <b-badge variant="success" pill v-if="row.id_estado_exp == 3">Aprobado</b-badge>
                    <b-badge variant="danger" pill v-if="row.id_estado_exp == 4">Observado</b-badge>
                  </td>
                  <td v-if="user.id_area_trabajo == 8 || user.id_area_trabajo == 5">
                     <b-button variant="gray" size="sm" @click="loadRow(row.c_cod_exp)" :id="'loadingApprove-'+row.c_cod_exp">
                      <b-icon icon="file-earmark-text" aria-hidden="true"></b-icon>
                    </b-button>
                    <div class="data-loader" :id="'loaderApprove-'+row.c_cod_exp" style="display:none;"> 
                      <i class="la la-spinner la-spin" ></i>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div v-show="expedientes.length != 0" class="mt-3">
            <!-- <h6 class="text-right">Pagi</h6> -->
            <b-pagination v-model="page" :total-rows="allRows" 
            :per-page="perpage" align="right" @input =" loadPage(page)" first-number
            last-number>
            </b-pagination>
          </div>
        </Widget>
        <Widget v-show="showDetail"
          title="<h5>Detalle del <span class='fw-semi-bold'>Expediente</span></h5>"
          customHeader
        > 
        <br>
          <div v-show="showDetail">

            <div class="widget-padding-md w-100 h-100 text-left border rounded">
            
                <div class="row">
                  <div class="col-sm-4 col-md-4 col-lg-4"><p class="fw-semi-bold">N° de Registro:</p> </div>
                  <div class="col-sm-8 col-md-8 col-md-offset-2 col-lg-8 col-lg-offset-0">{{form.c_cod_exp}}</div>
                </div>
                <div class="row">
                  <div class="col-sm-4 col-md-4 col-lg-4"><p class="fw-semi-bold">Administrado</p> </div>
                  <div class="col-sm-8 col-md-8 col-md-offset-2 col-lg-8 col-lg-offset-0">{{form.c_nomb_adm}} {{form.c_appat_adm}}  {{form.c_apmat_adm}}</div>
                </div>
                <div class="row">
                  <div class="col-sm-4 col-md-4 col-lg-4"><p class="fw-semi-bold">Fecha:</p> </div>
                  <div class="col-sm-8 col-md-8 col-md-offset-2 col-lg-8 col-lg-offset-0">{{format_date(form.d_tramite_exp)}}</div>
                </div>
                <div class="row">
                  <div class="col-sm-4 col-md-4 col-lg-4"><p class="fw-semi-bold">Documento:</p> </div>
                  <div class="col-sm-8 col-md-8 col-md-offset-2 col-lg-8 col-lg-offset-0"> 
                    <div v-for="file in form.arc_exp" :key="file.id_archivos_exp">
                      <a target="_blank" :href="ip+'/public/'+file.c_archivo_exp">
                    {{file.c_archivo_exp}}</a>
                    </div>
                  </div>                </div>
                <div class="row">
                  <div class="col-sm-4 col-md-4 col-lg-4"><p class="fw-semi-bold">Estado:</p> </div>
                  <div class="col-sm-8 col-md-8 col-md-offset-2 col-lg-8 col-lg-offset-0"><b-badge variant="success" pill>Aprobado</b-badge></div>
                </div>
              
            </div> 
            <br><br>
            <div class="widget-padding-md w-100 h-100 text-left border rounded">
              <b-form @submit.prevent="onSubmit" @reset.prevent="onReset" v-if="show">
                <b-form-group label="Especificacion C/S Observaciones" v-slot="{ ariaDescribedby }">
                  <b-form-radio-group
                    v-model="csObservaciones"
                    :options="options"
                    :aria-describedby="ariaDescribedby"
                    name="radio-inline"
                  ></b-form-radio-group>
                </b-form-group>

                <b-form-group v-show="csObservaciones==true" id="input-group-4" label="Informe de observaciones:" label-for="input-4">
                  <div class="text-right">
                    <b-button @click="removeObservaciones" variant="danger">Quitar todo</b-button>
                    <vue-dropzone ref="dropzoneObservaciones" id="dropzoneObservaciones" :options="dropzoneOptions"
                    @vdropzone-complete="afterCompleteObservaciones"></vue-dropzone>
                  </div>
                </b-form-group>
                <b-form-group  v-show="csObservaciones==false"  id="input-group-4" label="Informe Legal:" label-for="input-4">
                  <div class="text-right">
                    <b-button @click="removeLegal" variant="danger">Quitar todo</b-button>
                    <vue-dropzone ref="dropzoneLegal" id="dropzoneLegal" :options="dropzoneOptions"
                    @vdropzone-complete="afterCompleteLegal"></vue-dropzone>
                  </div>
                </b-form-group>
                <b-form-group v-show="csObservaciones==false" id="input-group-4" label="AutoDirectoral:" label-for="input-4">
                  <div class="text-right">
                    <b-button @click="removeAutoDirectoral" variant="danger">Quitar todo</b-button>
                    <vue-dropzone ref="dropzoneAutoDirectoral" id="dropzoneAutoDirectoral" :options="dropzoneOptions"
                    @vdropzone-complete="afterCompleteAutoDirectoral"></vue-dropzone>
                  </div>
                </b-form-group>
                <b-form-group v-show="csObservaciones==false" id="input-group-4" label="Proyecta el Oficio:" label-for="input-4">
                  <div class="text-right">
                    <b-button @click="removeOficio" variant="danger">Quitar todo</b-button>
                    <vue-dropzone ref="dropzoneOficio" id="dropzoneOficio" :options="dropzoneOptions"
                    @vdropzone-complete="afterCompleteOficio"></vue-dropzone>
                  </div>
                </b-form-group>
                <b-form-group  v-show="csObservaciones==false"  id="input-group-4" label="Proyecta la Resolución:" label-for="input-4">
                  <div class="text-right">
                    <b-button @click="removeResolucion" variant="danger">Quitar todo</b-button>
                    <vue-dropzone ref="dropzoneResolucion" id="dropzoneResolucion" :options="dropzoneOptions"
                    @vdropzone-complete="afterCompleteResolucion"></vue-dropzone>
                  </div>
                </b-form-group>

                <div class="data-loader" v-if="loadingForm">
                  <i class="la la-spinner la-spin" ></i>
                </div>
                <b-button type="submit" v-if="!loadingForm" variant="inverse">Guardar</b-button>
                <b-button type="reset" v-if="!loadingForm" variant="default">cancelar</b-button>

              </b-form>
              <!--<b-card class="mt-3" header="Form Data Result">
                <pre class="m-0">{{ form }}</pre>
              </b-card>-->
            </div>

          </div>
        </Widget>
      </b-col>
    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import config from '../../config'
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../components/Sparklines/Sparklines'
import moment from 'moment'
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'



export default {
  name: 'Legal',
  components: {
    Widget,
    Sparklines,
    vueDropzone: vue2Dropzone
  },
  data() {
    return {
      user: {},
      ip: config.ip,
      page:1,
      perpage:10,
      allRows: 0,
      showTable: true,
      showDetail: false,

      loadingTable:true,
      loadingForm: false,
      showList: true,
      showTable: false,
      expedientes: [],
      
      typeInput: null,
      searchInput: '',
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        maxFilesize: 500,
        headers: { "My-Awesome-Header": "header value" }
      },
      selected: 'withoutObs',
      csObservaciones: false,
      options: [
        { text: 'Sin observaciones', value: false },
        { text: 'Con observaciones', value: true }
      ],

      form: {},
      fileObservaciones: [],
      fileLegal: [],
      fileAutoDirectoral: [],
      fileOficio: [],
      fileResolucion: [],
      show: true,

      optionsPerPage: [
        {text: '10',value: 10},
        {text: '20', value: 20},
        {text: '30',value: 30}
      ],
    };
  },
  methods: {
    format_date(value){
      if (value) {
        return moment(String(value)).format('L')
      }
    },
    removeObservaciones(){
      this.fileObservaciones = [];
      this.$refs.dropzoneObservaciones.removeAllFiles();
    },
    removeLegal(){
      this.fileLegal = [];
      this.$refs.dropzoneLegal.removeAllFiles();
    },
    removeAutoDirectoral(){
      this.fileAutoDirectoral = [];
      this.$refs.dropzoneAutoDirectoral.removeAllFiles();
    },
    removeOficio(){
      this.fileOficio = [];
      this.$refs.dropzoneOficio.removeAllFiles();
    },
    removeResolucion(){
      this.fileObservaciones = [];
      this.$refs.dropzoneResolucion.removeAllFiles();
    },
    afterCompleteObservaciones(file){
      this.fileObservaciones.push(file);
    },
    afterCompleteLegal(file){
      this.fileLegal.push(file);
    },
    afterCompleteAutoDirectoral(file){
      this.fileAutoDirectoral.push(file);
    },
    afterCompleteOficio(file){
      this.fileOficio.push(file);
    },
    afterCompleteResolucion(file){
      this.fileResolucion.push(file);
    },
    searchData(){
      if(this.typeInput === "todo"){
        this.loadAll();
      }
      else if(this.searchInput ==null || this.searchInput == "" ||
      this.typeInput == null || this.typeInput == ""){
        this.$toasted.error('Ingrese los campos de búsqueda', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingTable = true;
        this.showTable = false;
        axios.post(config.hostname+'expediente/buscar_expediente',
        {
          "search": this.searchInput,
          "origen": this.typeInput,
          "id_area_trabajo":5,
          "page":1,
          "perpage":10
        })
        .then( (response) =>{
          if(response.data.data.length == 0){
            this.$toasted.error('No se encontraron resultados', {
              duration: 1500,
              position: 'top-center'
            });
            this.loadingTable = false;
            this.showTable = true;
          }
          else{
            this.$toasted.success('Resultados', {
              duration: 1500,
              position: 'top-center'
            });
            this.expedientes = response.data.data;
            this.loadingTable = false;
            this.showTable = true;
          }
          
        })
        .catch( (error) =>{
          console.log(error);
        });
      }
    },
    loadAll(){
      this.loadingTable = true;
      this.showTable = false;
      axios.get(config.hostname+'expediente/obtener_expediente_area/5/'+this.page+'/'+this.perpage)
      .then( (response) =>{
        this.expedientes = response.data.data;
        this.loadingTable = false;
        this.showTable = true;
        this.$toasted.success('Resultados', {
          duration: 1500,
          position: 'top-center'
        });
        })
        .catch( (error) =>{
          this.loadingTable = false;
          this.showTable = true;
          console.log(error);
        });
    },
    loadPerPage(perpage){
      this.page = 1;
      axios.get(config.hostname+'expediente/obtener_expediente_area/5/1/'+perpage)
      .then( (response) =>{
        this.expedientes = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPage(page){
      axios.get(config.hostname+'expediente/obtener_expediente_area/5/'+page+'/'+this.perpage)
      .then( (response) =>{
        this.expedientes = response.data.data;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadRow(id){
      document.getElementById('loadingApprove-'+id).style.display = 'none';
      document.getElementById('loaderApprove-'+id).style.display = 'block';
       axios.post(config.hostname+'expediente/buscar_expediente',
        {
          "search": id,
          "origen": "exp",
          "id_area_trabajo":5,
          "page":1,
          "perpage":10
        })
        .then( (response) =>{
          document.getElementById('loadingApprove-'+id).style.display = 'block';
          document.getElementById('loaderApprove-'+id).style.display = 'none';
          if(response.data.data.length == 0){
            this.$toasted.error('No se encontraron resultados', {
              duration: 1500,
              position: 'top-center'
            });
          }
          else{
            this.form = response.data.data[0];
            this.showList = false;
            this.showDetail = true;
          }
        })
        .catch( (error) =>{
          console.log(error);
        });
    },
    formatDate(value){
      if (value) {
        return moment(String(value)).format('L')
      }
    },
    loadData(){
       axios.get(config.hostname+'expediente/obtener_expediente_area/5/'+this.page+'/'+this.perpage)
      .then( (response) =>{
        this.expedientes = response.data.data;
        this.allRows = Number(response.data.total);
        this.loadingTable = false;
        this.showTable = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    selectExp: function (){
      this.showTable = false;
      this,this.showDetail = true;
    },
    onSubmit() {
      if(this.csObservaciones == true){
        if(this.fileObservaciones.length == 0){
           this.$toasted.error('Suba un archivo de observaciones', {
            duration: 1500,
            position: 'top-center'
          });
        }
        else{
          this.loadingForm = true;
          var fd = new FormData();
          this.fileObservaciones.forEach(
            e=> fd.append('c_informe_tecnico',e),
          );
          this.fileObservaciones.forEach(
            e=> fd.append('c_autodirectoral',e),
          );
          this.fileObservaciones.forEach(
            e=> fd.append('c_proyecta_oficio',e),
          );
          this.fileObservaciones.forEach(
            e=> fd.append('c_proyecta_resolucion',e),
          );
          fd.append('id_expediente',this.form.id_expediente);
          axios.post(config.hostname+'expediente/observar_expediente_legal',fd,
            {
              headers: {
                'Content-Type':'multipart/form-data; ',
                'token':window.localStorage.getItem('token')
              }
            })
            .then( (response) =>{
              if(response.data.status === 'success'){
                if(config.hosts == true){
                  axios.post(config.hostname_bd+'expediente/observar_expediente_legal',fd,
                  {
                    headers: {
                      'Content-Type':'multipart/form-data; ',
                      'token':window.localStorage.getItem('token')
                    }
                  }).then( (response) =>{
                    if(response.data.status === 'success'){
                      this.$refs.dropzoneObservaciones.removeAllFiles();
                      this.fileObservaciones = [];
                      this.loadingForm = false;
                      this.$toasted.success(response.data.message, {
                        duration: 1000,
                        position: 'top-center'
                      });
                      this.loadData();
                      this.showList = true;
                      this.showDetail = false;
                    }else{
                      this.loadingForm = false;
                      this.$toasted.error(response.data.message, {
                        duration: 1000,
                        position: 'top-center'
                      });
                      this.showList = true;
                      this.showDetail = false;
                    }
                  })
                  .catch( (error) =>{
                    this.loadingForm = false;
                    this.$toasted.error(response.data.message, {
                      duration: 1000,
                      position: 'top-center'
                    });
                    this.showList = true;
                    this.showDetail = false;
                  })
                }else{
                  this.$refs.dropzoneObservaciones.removeAllFiles();
                  this.fileObservaciones = [];
                  this.loadingForm = false;
                  this.$toasted.success(response.data.message, {
                    duration: 1000,
                    position: 'top-center'
                  });
                  this.loadData();
                  this.showList = true;
                  this.showDetail = false;
                }
              }
              else{
                this.loadingForm = false;
                this.$toasted.error(response.data.message, {
                  duration: 1000,
                  position: 'top-center'
                });
                this.showList = true;
                this.showDetail = false;
              }
            })
            .catch( (error) =>{
              this.loadingForm = false;
              this.$toasted.error('Error', {
                duration: 1000,
                position: 'top-center'
              });
              this.showList = true;
              this.showDetail = false;
            });
        }
      }
      else{
        if(this.fileLegal.length == 0 || this.fileAutoDirectoral.length == 0 || 
         this.fileOficio.length == 0 || this.fileResolucion.length == 0){
           this.$toasted.error('Sube los archivos necesarios', {
            duration: 1500,
            position: 'top-center'
          });
        }
        else{
          this.loadingForm = true;
            var fd = new FormData();
            this.fileLegal.forEach(
            e=> fd.append('c_informe_tecnico',e),
            );
            this.fileAutoDirectoral.forEach(
              e=> fd.append('c_autodirectoral',e),
            );
            this.fileOficio.forEach(
              e=> fd.append('c_proyecta_oficio',e),
            );
            this.fileResolucion.forEach(
              e=> fd.append('c_proyecta_resolucion',e),
            );
            fd.append('id_expediente',this.form.id_expediente);
            axios.post(config.hostname+'expediente/aprobar_expediente_legal',fd,
              {
                headers: {
                  'Content-Type':'multipart/form-data; ',
                  'token':window.localStorage.getItem('token'),
                }
              })
              .then( (response) =>{
                if(response.data.status === 'success'){
                  if(config.hosts == true){
                    axios.post(config.hostname_bd+'expediente/aprobar_expediente_legal',fd,
                    {
                      headers: {
                        'Content-Type':'multipart/form-data; ',
                        'token':window.localStorage.getItem('token'),
                      }
                    }).then( (response) =>{
                      if(response.data.status === 'success'){
                        this.$refs.dropzoneLegal.removeAllFiles();
                        this.$refs.dropzoneAutoDirectoral.removeAllFiles();
                        this.$refs.dropzoneOficio.removeAllFiles();
                        this.$refs.dropzoneResolucion.removeAllFiles();
                        this.fileLegal = [];
                        this.fileAutoDirectoral = [];
                        this.fileOficio = [];
                        this.fileResolucion= [];
                        this.loadingForm = false;
                        this.$toasted.success(response.data.message, {
                          duration: 1500,
                          position: 'top-center'
                        });
                        this.loadData();
                        this.showList = true;
                        this.showDetail = false;
                      }else{
                        this.loadingForm = false;
                        this.$toasted.error(response.data.message, {
                          duration: 1500,
                          position: 'top-center'
                        });
                        this.showList = true;
                        this.showDetail = false;
                      }
                    })
                    .catch( (error) =>{
                      this.loadingForm = false;
                      this.$toasted.error('Error', {
                        duration: 1500,
                        position: 'top-center'
                      });
                      this.showList = true;
                      this.showDetail = false;
                    });
                  }else{
                    this.$refs.dropzoneLegal.removeAllFiles();
                    this.$refs.dropzoneAutoDirectoral.removeAllFiles();
                    this.$refs.dropzoneOficio.removeAllFiles();
                    this.$refs.dropzoneResolucion.removeAllFiles();
                    this.fileLegal = [];
                    this.fileAutoDirectoral = [];
                    this.fileOficio = [];
                    this.fileResolucion= [];
                    this.loadingForm = false;
                    this.$toasted.success(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                    this.loadData();
                    this.showList = true;
                    this.showDetail = false;
                  }
                }
                else{
                  this.loadingForm = false;
                  this.$toasted.error(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
                  this.showList = true;
                  this.showDetail = false;
                }
              })
              .catch( (error) =>{
                this.loadingForm = false;
                this.$toasted.error('Error', {
                  duration: 1500,
                  position: 'top-center'
                });
                this.showList = true;
                this.showDetail = false;
              });
        }
      }
    },
    onReset(event) {
      //event.preventDefault()
      // Reset our form values
      this.form = {};
      this.fileObservaciones= [];
      this.fileLegal=[];
      this.fileAutoDirectoral= [];
      this.fileOficio=[];
      this.fileResolucion= [];

      this.show = false
      this.$nextTick(() => {
        this.show = true,
        this.showList = true,
        this.showDetail = false
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
    this.user = JSON.parse(window.localStorage.getItem('user'));
    this.loadData();
  }
};
</script>

<style src="./Legal.scss" lang="scss" scoped />