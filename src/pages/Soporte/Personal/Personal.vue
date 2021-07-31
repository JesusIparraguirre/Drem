<template>
<div class="usuario-page">
        <h2 class="page-title">Gestión de - <span class="fw-semi-bold">personal</span></h2>
    <b-row>
      <b-col>
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Personal</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTable"></i>
        </div>
        <div v-show="showTable">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <!--<th>Picture</th>-->
                  <th>Nombre Cargo</th>
                  <th>Nombre Area de Trabajo</th>
                  <th class="hidden-sm-down">Nombres</th>
                  <th class="hidden-sm-down">Apellidos</th>
                  <th class="hidden-sm-down">E-mail</th>
                  <th class="hidden-sm-down">Celular</th>
                  <th class="hidden-sm-down">Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in users" :key="row.id_usuario">
                  <td>{{row.id_usuario}}</td>
                  <td>
                    {{row.c_nombres_cargo}}
                  </td>
                  <td>
                    {{row.c_nomb_area_trabajo}}
                  </td>
                  <td>
                    {{row.c_nombres_personal}}
                  </td>
                  <td class="text-semi-muted">
                    {{row.c_appat_personal}} {{row.c_apmat_personal}}
                  </td>
                  <td class="text-semi-muted">
                    {{row.c_email_personal}}
                  </td>
                  <td class="text-semi-muted">
                    {{row.c_celular_personal}}
                  </td>
                  <td class="width-150">
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="register">Agregar personal</b-button>
            </div>
          </div> -->
        </div>
        <div v-show="showRegister" >
             <b-form @submit.prevent="onSubmit" @reset.prevent="onReset" v-if="show">
                <b-row>
                  <b-col cols=10>
                    <b-form-group id="input-group-dni" label="Documento de identidad:" label-for="input-dni">
                      <b-form-input
                        id="input-dni"
                        v-model="form.dni"
                        type = "text"
                        maxlength = "8"
                        placeholder="Ingrese documento de identidad"
                        required :disabled="verified == true"
                      ></b-form-input>
                    </b-form-group>
                  </b-col>
                  <b-col cols=2>
                    <div class="data-loader"  v-if="loadingDni">
                      <i class="la la-spinner la-spin"></i>
                    </div>
                    <b-button v-if="!loadingDni" variant="inverse" class="mr-xs" 
                    style="margin-top:28px" size="xl" @click="verifyUser" required :disabled="form.dni.length !== 8">
                      Buscar</b-button>
                  </b-col>
                </b-row>                
                <b-form-group id="input-group-name" label="Nombres:" label-for="input-name" description="">
                  <b-form-input
                    id="input-name"
                    v-model="form.name"
                    type="text"
                    placeholder="Ingrese nombres"
                    required disabled
                  ></b-form-input>
                </b-form-group>
                <b-row>
                  <b-col>
                    <b-form-group id="input-group-apaterno" label="Apellido paterno:" label-for="input-apaterno" description="">
                      <b-form-input
                        id="input-apaterno"
                        v-model="form.lastnamep"
                        type="text"
                        placeholder="Ingrese apellido paterno"
                        required disabled
                      ></b-form-input>
                  </b-form-group>   
                </b-col>
                <b-col>
                  <b-form-group id="input-group-amaterno" label="Apellido materno:" label-for="input-amaterno" description="">
                    <b-form-input
                      id="input-amaterno"
                      v-model="form.lastnamem"
                      type="text"
                      placeholder="Ingrese apellido materno"
                      required disabled
                    ></b-form-input>
                  </b-form-group>
                </b-col>
              </b-row>
              <b-row>
                <b-col cols=8>
                  <b-form-group id="input-group-email" label="Correo electrónico:" label-for="input-email" description="">
                    <b-form-input
                    id="input-email"
                    v-model="form.email"
                    type="email"
                    placeholder="Ingrese un correo electrónico"
                    required :disabled="verified == false"
                    ></b-form-input>
                </b-form-group>
                </b-col>
                <b-col cols=4>
                  <b-form-group id="input-group-phone" label="Número de celular:" label-for="input-phone">
                  <b-form-input
                    id="input-phone"
                    v-model="form.phone"
                    type="text"
                    maxlength = "9"
                    placeholder="Ingrese un número de celular"
                    required :disabled="verified == false"
                  ></b-form-input>
                </b-form-group>
                </b-col>
              </b-row>
              <b-row>
                <b-col>
                  <b-form-group id="input-group-area" label="Área de trabajo:" label-for="input-area">
                    <b-form-select
                    id="input-area"
                    v-model="form.area"
                    :options="areas"
                    required :disabled="verified == false"
                    ></b-form-select>
                  </b-form-group>
                </b-col>
                <b-col>
                  <b-form-group id="input-group-positions" label="Cargo:" label-for="input-positions">
                    <b-form-select
                    id="input-positions"
                    v-model="form.possition"
                    :options="positions"
                    required :disabled="verified == false"
                    ></b-form-select>
                  </b-form-group> 
                </b-col>
              </b-row>
              <b-row>
                <b-col>
                  <b-form-group id="input-group-contract" label="Modalidad de contrato:" label-for="input-contract">
                    <b-form-select
                    id="input-contract"
                    v-model="form.contract"
                    :options="contracts"
                    required :disabled="verified == false"
                    ></b-form-select>
                  </b-form-group>
                </b-col>
                <b-col>
                  <b-form-group id="input-group-profession" label="Profesión:" label-for="input-profession">
                    <b-form-select
                    id="input-profession"
                    v-model="form.profession"
                    :options="professions"
                    required :disabled="verified == false"
                    ></b-form-select>
                  </b-form-group>
                </b-col>
              </b-row>
              <!-- <b-button v-if="!loadingDni" variant="inverse" class="mr-xs">Guardar</b-button> -->
                     
              <b-button type="submit" variant="inverse" v-if="!loadingForm">Guardar</b-button>
              <b-button class="ml-2" type="reset" variant="default" v-if="!loadingForm">cancelar</b-button>
              <div class="data-loader">
                <i class="la la-spinner la-spin" v-if="loadingForm"></i>
              </div>
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
import axios from 'axios';
import config from '../../../config';
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../../components/Sparklines/Sparklines';
import moment from 'moment';
moment.locale('es');

export default {
  name: 'Personal',
  components: {
    Widget,
    Sparklines,
  },
  data() {
    return {
      verified: false,
      loadingTable: true,
      showTable: true,
      showRegister: false,
      loadingForm: false,
      loadingDni: false,
      users: [],
      contracts: [],
      positions: [],
      professions: [],
      form: {
        name: null,
        lastnamep: null,
        lastnamem: null,
        email: null,
        phone: null,
        dni: '',
        contract: null,
        profession: null,
        possition: null,
        area: null,
        checked: []
      },
      show: true, 
    };
  },
  methods: {
    register(){
      // this.$router.push('/app/registerPartes');
      this.showTable = false;
      this.showRegister = true
    },

    onSubmit() {
      this.loadingForm=true;
      axios.post(config.hostname+'personal/registrar_personal', {
      "id_personal": 1,
      "id_cargo":this.form.possition, 
	    "id_modalidad_contrato":this.form.contract,
      "id_area_trabajo":this.form.area, 
      "id_profesion": this.form.profession,
      "c_nombres_personal": this.form.name, 
      "c_appat_personal": this.form.lastnamep, 
      "c_apmat_personal": this.form.lastnamem, 
      "c_foto_personal": "prueba", 
      "c_dni_personal": this.form.dni, 
      "c_email_personal": this.form.email, 
      "c_celular_personal": this.form.phone,
      "d_inicio_laboral": null,
      "d_cese_laboral": null,
      "d_create_personal": "2021-05-28T19:30:17.006Z",
      "d_update_personal": null,
      " b_personal": true
      })
      .then( (response) =>{
        this.loadingForm = false;
                  console.log(response);

        if(response.data.status === 'success'){
          this.$toasted.success(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
          this.verified = false
          this.form.nameDoc = '';
          this.form.codeDoc = '';
          this.form.lastnamem = '';
          this.form.lastnamep = '';
          this.form.dni = '';
          this.form.email = '';
          this.form.name = '';
          this.form.phone = '';
          this.form.contract = null;
          this.form.profession = null;
          this.form.possition = null;
          this.form.area = null;

          this.load();
          this.showTable = true;
          this.showRegister = false;
        }
        else{
          this.$toasted.error(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
        }
      })
      .catch( (error) =>{
        // this.$toasted.success('Error', {
        //     duration: 1500,
        //     position: 'top-center'
        // });
        this.loadingForm = true;
      });
    },
    onReset(event) {
      // event.preventDefault();
      this.verified = false
      this.form.nameDoc = '';
      this.form.codeDoc = '';
      this.form.lastnamem = '';
      this.form.lastnamep = '';
      this.form.dni = '';
      this.form.email = '';
      this.form.name = '';
      this.form.contract = null;
      this.form.profession = null;
      this.form.possition = null;
      this.form.area = null;
     
      this.show = true;
      this.showTable = true;
      this.showRegister = false;
      // this.$nextTick(() => {
      //   this.show = true,
      //   this.showTable = true,
      //   this.showRegister = false
      // })
    },
    verifyUser(){
      this.loadingDni = true;
      axios.get(config.hostname+'personal/obtener_persona/'+this.form.dni)
      .then((response) =>{
        this.loadingDni = false;
        let obj = response.data.data;
        if(Object.keys(obj).length === 0 && obj.constructor === Object){
          this.$toasted.error('Persona no encontrada', {
            duration: 2000,
            position: 'top-center'
          });
        }
        else{
          this.$toasted.success('Resultados', {
            duration: 2000,
            position: 'top-center'
          });
          this.form.name = obj.c_nombres_personal;
          this.form.lastnamep = obj.c_appat_personal;
          this.form.lastnamem = obj.c_apmat_personal;
          this.verified = true;
        }   
      })
      .catch( (error) =>{
        console.log(error);
      });
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
    },
    load(){
      axios.get(config.hostname+'personal/obtener_usuarios')
      .then( (response) =>{
        this.users = response.data.usuarios;
        this.loadingTable = false;
        this.showTable = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadContracts(){
      axios.get(config.hostname+'maestro/obtener_modalidad_contratado')
      .then( (response) =>{
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
    loadPositions(){
      axios.get(config.hostname+'maestro/obtener_cargos')
      .then( (response) =>{
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
    loadProfessions(){
      axios.get(config.hostname+'maestro/obtener_profesion')
      .then( (response) =>{
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
    loadAreas(){
      axios.get(config.hostname+'maestro/obtener_area_trabajo')
      .then( (response) =>{
        this.areas = response.data.data;
        this.areas.forEach(function (element) {
          element.text = element.c_nomb_area_trabajo;
          element.value = element.id_area_trabajo;
        });
      })
      .catch( (error) =>{
        console.log(error);
      });
    }
  },
  computed: {
    
  },
  created() {
    let user = JSON.parse(window.localStorage.getItem('user'));
    if (user.id_area_trabajo !=8) {
     this.$router.push('/app/partes');
    }
    axios.get(config.hostname+'personal/obtener_usuarios')
      .then( (response) =>{
        this.users = response.data.usuarios;
        this.loadingTable = false;
        this.showTable = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
      this.load();
      this.loadContracts();
      this.loadPositions();
      this.loadProfessions();
      this.loadAreas();
  }
};
</script>

<style src="./Personal.scss" lang="scss" scoped />