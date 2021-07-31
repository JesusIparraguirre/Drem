<template>
<div class="administracion-page">
        <h2 class="page-title">Area - <span class="fw-semi-bold">administración</span></h2>
    <b-row>
      <b-col>
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Documentos</span></h5>"
          customHeader settings close
        >
        <br>
        <div v-show="showTable">
          <div class="table-resposive">
            <table class="table">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <!--<th>Picture</th>-->
                  <th>Nombre</th>
                  <th class="hidden-sm-down">N° de documento</th>
                  <th class="hidden-sm-down">Tipo</th>
                  <th class="hidden-sm-down">Fecha</th>
                  <th class="hidden-sm-down">Documento</th>
                  <th class="hidden-sm-down">Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in tableStyles" :key="row.id">
                  <td>{{row.id}}</td>
                  <!--<td>
                    <img class="img-rounded" :src="row.picture" alt="" height="50" />
                  </td>-->
                  <td>
                    {{row.description}}
                  </td>
                  <td>
                    <p class="mb-0">
                      <small>
                        <span class="fw-semi-bold">Type:</span>
                        <span class="text-muted">&nbsp; {{row.info.type}}</span>
                      </small>
                    </p>
                    <p>
                      <small>
                        <span class="fw-semi-bold">Dimensions:</span>
                        <span class="text-muted">&nbsp; {{row.info.dimensions}}</span>
                      </small>
                    </p>
                  </td>
                  <td class="text-semi-muted">
                    {{parseDate(row.date)}}
                  </td>
                  <td class="text-semi-muted">
                    {{row.size}}
                  </td>
                  <td><a href="">ver</a> </td>
                  <td class="width-150">
                    <b-badge variant="warning" pill>Pending</b-badge>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="register">Agregar</b-button>

            </div>
          </div>
        </div>
          <div v-show="showRegister" >
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
              <b-form-group id="input-group-3" label="Modalidad de contrato:" label-for="input-3">
                  <b-form-select
                    id="input-3"
                    v-model="form.tipoDoc"
                    :options="tiposDoc"
                    required
                  ></b-form-select>
              </b-form-group>
              <b-form-group id="input-group-4" label="Modalidad de contrato:" label-for="input-4">
                  <b-form-select
                    id="input-4"
                    v-model="form.catDoc"
                    :options="catsDoc"
                    required
                  ></b-form-select>
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
            <!--<b-card class="mt-3" header="Form Data Result">
              <pre class="m-0">{{ form }}</pre>
            </b-card>-->
          </div>

        </Widget>
      </b-col>
    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../components/Sparklines/Sparklines'

import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'



export default {
  name: 'Administracion',
  components: {
    Widget,
    Sparklines,
    DatePicker,
    vueDropzone: vue2Dropzone
  },
  data() {
    return {
      showTable: true,
      showRegister: false,
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        maxFilesize: 0.5,
        headers: { "My-Awesome-Header": "header value" }
      },
      form: {
        nameDoc: null,
        codeDoc: null,
        tipoDoc: null,
        catDoc: null,
        timeDoc: null,

        checked: []
      },
      show: true, 
      tiposDoc: [{ text: 'Seleccionar', value: null }, 'Caja chica', 'Pagos'], 
      catsDoc: [{ text: 'Seleccionar', value: null }, 'Pago CAS', 'Pagos de alquiler casa', 'Pago de Pensionistas', 'Pago de Seguridad', 'Pago de Sentencia Judicial', 'Pago de Director', 'Pago de Locadores'], 
      tableStyles: [
        {
          id: 1,
          picture: require('../../assets/tables/1.jpg'), // eslint-disable-line global-require
          description: 'Palo Alto',
          info: {
            type: 'JPEG',
            dimensions: '200x150',
          },
          date: new Date('September 14, 2012'),
          size: '45.6 KB',
          progress: {
            percent: 29,
            colorClass: 'success',
          },
        },
        {
          id: 2,
          picture: require('../../assets/tables/2.jpg'), // eslint-disable-line global-require
          description: 'The Sky',
          info: {
            type: 'PSD',
            dimensions: '2400x1455',
          },
          date: new Date('November 14, 2012'),
          size: '15.3 MB',
          progress: {
            percent: 33,
            colorClass: 'warning',
          },
        },
        {
          id: 3,
          picture: require('../../assets/tables/3.jpg'), // eslint-disable-line global-require
          description: 'Down the road',
          label: {
            colorClass: 'danger',
            text: 'INFO!',
          },
          info: {
            type: 'JPEG',
            dimensions: '200x150',
          },
          date: new Date('September 14, 2012'),
          size: '49.0 KB',
          progress: {
            percent: 38,
            colorClass: 'inverse',
          },
        },
        {
          id: 4,
          picture: require('../../assets/tables/4.jpg'), // eslint-disable-line global-require
          description: 'The Edge',
          info: {
            type: 'PNG',
            dimensions: '210x160',
          },
          date: new Date('September 15, 2012'),
          size: '69.1 KB',
          progress: {
            percent: 17,
            colorClass: 'danger',
          },
        },
        {
          id: 5,
          picture: require('../../assets/tables/5.jpg'), // eslint-disable-line global-require
          description: 'Fortress',
          info: {
            type: 'JPEG',
            dimensions: '1452x1320',
          },
          date: new Date('October 1, 2012'),
          size: '2.3 MB',
          progress: {
            percent: 41,
            colorClass: 'primary',
          },
        },
      ],

    };
  },
  methods: {
    register(){
      // this.$router.push('/app/registerPartes');
      this.showTable = false;
      this.showRegister = true
    },
    onSubmit(event) {
      event.preventDefault()
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
    let user = JSON.parse(window.localStorage.getItem('user'));
    if (user.id_area_trabajo !=1) {
     this.$router.push('/app/partes');
    }
  }
};
</script>

<style src="./Administracion.scss" lang="scss" scoped />