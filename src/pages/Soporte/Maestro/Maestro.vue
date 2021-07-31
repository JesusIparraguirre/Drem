<template>
<div class="maestro-page">
        <h2 class="page-title">Gestión de - <span class="fw-semi-bold">maestro</span></h2>
    <b-row>
      <b-col log="6">
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Modalidades de Contrato</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTableContract"></i>
        </div>
        <div v-show="showTableContract">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <th>Nombre</th>
                  <th class="hidden-sm-down">Estado</th>
                  <th class="hidden-sm-down">Eliminar</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in contract" :key="row.id">
                  <td>{{row.id_modalidad_contrato}}</td>
                  <td>
                    {{row.c_nomb_modalidad_contrato}}
                  </td>
                  <td>
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                  <td>
                    <b-button variant="danger" size="sm" @click="openModalContractDelete(row.id_modalidad_contrato)">
                      <b-icon icon="trash" aria-hidden="true"></b-icon>
                    </b-button>
                     <b-modal :id="'modal-contract-delete-'+row.id_modalidad_contrato" title="Eliminar Modalidad de Contrato" header-bg-variant="dark" body-bg-variant="light"
                    header-text-variant="white" :hide-footer="true">
                    ¿Desea eliminar la modalidad de contrato? <br>
                    <div class="text-right">
                      <b-button v-if="!loadingModalContract" variant="secondary"  @click="hideModalContractDelete(row.id_modalidad_contrato,row);">No</b-button>
                      <b-button v-if="!loadingModalContract" variant="danger" @click="deleteContract(row.id_modalidad_contrato);">Si</b-button>
                       <div class="data-loader">
                        <i class="la la-spinner la-spin" v-if="loadingModalContract"></i>
                      </div>
                    </div>
                  </b-modal>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="showRegister_contrato=true" v-if="!showRegister_contrato" >
                Agregar</b-button>
            </div>
          </div>
        </div>
        <div v-show="showRegister_contrato" ><br>
             <b-form @submit.prevent="addContract" @reset.prevent="onResetContract" v-if="show">
                <b-form-group id="input-group-1" label="Nombre del contrato:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="nameContract"
                    type="text"
                    placeholder="Ingrese el nombre del contrato"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Descripción:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="desContract"
                    placeholder="Ingrese una descripción"
                    required
                  ></b-form-input>
                </b-form-group>

                <div class="data-loader" v-if="loadingModalContract">
                  <i class="la la-spinner la-spin" ></i>
                </div>
                <b-button v-if="!loadingModalContract" type="submit" variant="inverse">Guardar</b-button>
                <b-button v-if="!loadingModalContract" type="reset" variant="default" @click="showRegister_contrato=false">cancelar</b-button>
              </b-form>
        </div>
          
        </Widget>
      </b-col>
     
      <!--Profesiones -->
      <b-col log="6">
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Profesiones</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTableProfession"></i>
        </div>
        <div v-show="showTableProfession">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <th>Nombre</th>
                  <!-- <th>Descripción</th> -->
                  <th class="hidden-sm-down">Estado</th>
                  <th class="hidden-sm-down">Eliminar</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in profession" :key="row.id_profesion">
                  <td>{{row.id_profesion}}</td>
                  <td>
                    {{row.c_nomb_profesion}}
                  </td>
                  <td class="width-150">
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                  <td>
                    <b-button variant="danger" size="sm" @click="openModalProfessionDelete(row.id_profesion)">
                      <b-icon icon="trash" aria-hidden="true"></b-icon>
                    </b-button>
                     <b-modal :id="'modal-profession-delete-'+row.id_profesion" title="Eliminar Profesion" header-bg-variant="dark" body-bg-variant="light"
                    header-text-variant="white" :hide-footer="true">
                    ¿Desea eliminar la profesion? <br>
                    <div class="text-right">
                      <b-button v-if="!loadingModalProfession" variant="secondary"  @click="hideModalProfessionDelete(row.id_profesion,row);">No</b-button>
                      <b-button v-if="!loadingModalProfession" variant="danger" @click="deleteProfession(row.id_profesion);">Si</b-button>
                       <div class="data-loader">
                        <i class="la la-spinner la-spin" v-if="loadingModalProfession"></i>
                      </div>
                    </div>
                  </b-modal>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="showRegister_profesion=true" v-if="!showRegister_profesion">
                Agregar</b-button>
            </div>
          </div>
        </div>
        <div v-show="showRegister_profesion" ><br>
             <b-form @submit.prevent="addProfession" @reset.prevent="onResetProfession" v-if="show">
                <b-form-group id="input-group-1" label="Nombre de la profesion:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="nameProfession"
                    type="text"
                    placeholder="Ingrese el nombre de la profesion"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Descripción:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="desProfession"
                    placeholder="Ingrese una descripción"
                    required
                  ></b-form-input>
                </b-form-group>

                <div class="data-loader" v-if="loadingModalProfession">
                  <i class="la la-spinner la-spin"></i>
                </div>
                <b-button v-if="!loadingModalProfession" type="submit" variant="inverse">Guardar</b-button>
                <b-button v-if="!loadingModalProfession" type="reset" variant="default" @click="showRegister_profesion=false">cancelar</b-button>
              </b-form>
        </div>
          
        </Widget>
      </b-col>

      <!-- Areas de trabajo -->
      <b-col log="6">
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Áreas de Trabajo</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTableArea"></i>
        </div>
        <div v-show="showTableArea">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <th>Nombre</th>
                  <!-- <th>Descripción</th> -->
                  <th class="hidden-sm-down">Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in area" :key="row.id_area_trabajo">
                  <td>{{row.id_area_trabajo}}</td>
                  <td>
                    {{row.c_nomb_area_trabajo}}
                  </td>
                  <!-- <td>
                    {{row.c_des_area_trabajo}}
                  </td> -->
                  <td class="width-150">
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="showRegister_area=true" v-if="!showRegister_area">
                Agregar</b-button>
            </div>
          </div>
        </div>
        <div v-show="showRegister_area" >
             <b-form @submit.prevent="addArea" @reset.prevent="onResetArea" v-if="show">
                <b-form-group id="input-group-1" label="Nombre del area:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="nameArea"
                    type="text"
                    placeholder="Ingrese el nombre del area"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Descripción:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="desArea"
                    placeholder="Ingrese una descripción"
                    required
                  ></b-form-input>
                </b-form-group>

                <div class="data-loader" v-if="loadingModalArea">
                  <i class="la la-spinner la-spin"></i>
                </div>
                <b-button type="submit" variant="inverse" v-if="!loadingModalArea">Guardar</b-button>
                <b-button type="reset" variant="default" @click="showRegister_area=false"
                v-if="!loadingModalArea">cancelar</b-button>
              </b-form>
        </div>
          
        </Widget>
      </b-col>


    <!--Cargos-->
    <b-col log="6">
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Cargos de Trabajo</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTablePosition"></i>
        </div>
        <div v-show="showTablePosition">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <th>Nombre</th>
                  <!-- <th>Descripción</th> -->
                  <th class="hidden-sm-down">Estado</th>
                  <th class="hidden-sm-down">Eliminar</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in position" :key="row.id_cargo">
                  <td>{{row.id_cargo}}</td>
                  <td>
                    {{row.c_nombres_cargo}}
                  </td>
                  <!-- <td>
                    {{row.c_des_area_trabajo}}
                  </td> -->
                  <td class="width-150">
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                  <td>
                    <b-button variant="danger" size="sm" @click="openModalPositionDelete(row.id_cargo)">
                      <b-icon icon="trash" aria-hidden="true"></b-icon>
                    </b-button>
                     <b-modal :id="'modal-position-delete-'+row.id_cargo" title="Eliminar Cargo" header-bg-variant="dark" body-bg-variant="light"
                    header-text-variant="white" :hide-footer="true">
                    ¿Desea eliminar el cargo? <br>
                    <div class="text-right">
                      <b-button v-if="!loadingModalPosition" variant="secondary"  @click="hideModalPositionDelete(row.id_cargo,row);">No</b-button>
                      <b-button v-if="!loadingModalPosition" variant="danger" @click="deletePosition(row.id_cargo);">Si</b-button>
                       <div class="data-loader">
                        <i class="la la-spinner la-spin" v-if="loadingModalPosition"></i>
                      </div>
                    </div>
                  </b-modal>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="showRegister_position=true" v-if="!showRegister_position">
                Agregar</b-button>
            </div>
          </div>
        </div>
        <div v-show="showRegister_position" >
             <b-form @submit.prevent="addPosition" @reset.prevent="onResetPosition" v-if="show">
                <b-form-group id="input-group-1" label="Nombre del cargo:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="namePosition"
                    type="text"
                    placeholder="Ingrese el nombre del area"
                    required
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-2" label="Descripción:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="desPosition"
                    placeholder="Ingrese una descripción"
                    required
                  ></b-form-input>
                </b-form-group>

                <div class="data-loader" v-if="loadingModalPosition">
                  <i class="la la-spinner la-spin"></i>
                </div>
                <b-button type="submit" variant="inverse" v-if="!loadingModalPosition">Guardar</b-button>
                <b-button type="reset" variant="default" v-if="!loadingModalPosition" @click="showRegister_position=false">cancelar</b-button>
              </b-form>
        </div>
          
        </Widget>
      </b-col>




      <!--Tipo de expediente-->
      <b-col log="6">
        <Widget
          title="<h5>Lista de <span class='fw-semi-bold'>Tipos de Expediente</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loadingTableTypeDoc"></i>
        </div>
        <div v-show="showTableTypeDoc">
          <div class="table-resposive" >
            <table class="table table-hover">
              <thead>
                <tr>
                  <th class="hidden-sm-down">#</th>
                  <th>Nombre</th>
                  <th class="hidden-sm-down">Estado</th>
                  <th class="hidden-sm-down">Eliminar</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in typeDoc" :key="row.id_tipo_doc_exp">
                  <td>{{row.id_tipo_doc_exp}}</td>
                  <td>
                    {{row.c_nomb_tipo_doc_exp}}
                  </td>
                  <td class="width-150">
                    <b-badge variant="success" pill>Activo</b-badge>
                  </td>
                  <td>
                    <b-button variant="danger" size="sm" @click="openModalTypeDelete(row.id_tipo_doc_exp)">
                      <b-icon icon="trash" aria-hidden="true"></b-icon>
                    </b-button>
                     <b-modal :id="'modal-type-delete-'+row.id_tipo_doc_exp" title="Eliminar Tipo de Expediente" header-bg-variant="dark" body-bg-variant="light"
                    header-text-variant="white" :hide-footer="true">
                    ¿Desea eliminar el tipo de expediente? <br>
                    <div class="text-right">
                      <b-button v-if="!loadingModalType" variant="secondary"  @click="hideModalTypeDelete(row.id_tipo_doc_exp,row);">No</b-button>
                      <b-button v-if="!loadingModalType" variant="danger" @click="deleteType(row.id_tipo_doc_exp);">Si</b-button>
                       <div class="data-loader">
                        <i class="la la-spinner la-spin" v-if="loadingModalType"></i>
                      </div>
                    </div>
                  </b-modal>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="clearfix">
            <div class="float-right">
              <b-button variant="inverse" class="mr-xs" size="sm" @click="showRegister_tipo=true" v-if="!showRegister_tipo">
                Agregar</b-button>
            </div>
          </div>
        </div>
        <div v-show="showRegister_tipo" >
             <b-form @submit.prevent="addType" @reset.prevent="onResetType" v-if="show">
                <b-form-group id="input-group-1" label="Nombre de tipo de expediente:" label-for="input-1" description="">
                  <b-form-input
                    id="input-1"
                    v-model="nameType"
                    type="text"
                    placeholder="Ingrese el nombre del tipo de expediente"
                    required
                  ></b-form-input>
                </b-form-group>

                <!-- <b-form-group id="input-group-2" label="Descripción:" label-for="input-2">
                  <b-form-input
                    id="input-2"
                    v-model="desType"
                    placeholder="Ingrese una descripción"
                    required
                  ></b-form-input>
                </b-form-group> -->

                <div class="data-loader" v-if="loadingModalType">
                  <i class="la la-spinner la-spin"></i>
                </div>
                <b-button v-if="!loadingModalType" type="submit" variant="inverse">Guardar</b-button>
                <b-button v-if="!loadingModalType" type="reset" variant="default" @click="showRegister_tipo=false">cancelar</b-button>
              </b-form>
        </div>
          
        </Widget>
      </b-col>

    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import axios from 'axios';
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../../components/Sparklines/Sparklines'
import config from '../../../config'

export default {
  name: 'Maestro',
  components: {
    Widget,
    Sparklines,
  },
  data() {
    return {
      ip:config.ip,
      showTableTypeDoc: false,
      loadingTableTypeDoc: true,
      typeDoc: [],
      showTableArea: false,
      loadingTableArea: true,
      area: [],
      showTableContract: false,
      loadingTableContract: true,
      contract: [],
      showTableProfession: false,
      loadingTableProfession: true,
      profession: [],

      showTablePosition: false,
      loadingTablePosition: true,
      position:[],

      showTable_contrato: true,
      showRegister_contrato: false,

      showtable_profession: true,
      showRegister_profesion: false,

      showTable_area: true,
      showRegister_area: false,

      showTable_position: true,
      showRegister_position: false,

      showTable_tipo: true,
      showRegister_tipo: false,

      nameContract: '',
      desContract: '',
      nameProfession: '',
      desProfession: '',
      nameArea: '',
      desArea: '',
      namePosition: '',
      desPosition: '',
      nameType:'',
      desType: '',

      loadingModalContract : false,
      loadingModalProfession: false,
      loadingModalArea: false,
      loadingModalType: false,
      loadingModalPosition: false,

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
    addContract(){
      if(this.nameContract == '' || this.nameContract == null ||
      this.desContract == '' || this.desContract == null){
        this.$toasted.error('Ingrese todos los campos', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingModalContract = true;
        axios.post(config.hostname+'maestro/registrar_modalidad_contratado',{
        'c_nomb_modalidad_contrato':this.nameContract, 
        "c_des_modalidad_contrato": this.desContract, 
        "b_modalidad_contrato":true
        })
        .then( (response) =>{
          if(response.data.status === 'success'){
            if(config.hosts == true){
              axios.post(config.hostname_bd+'maestro/registrar_modalidad_contratado',{
              'c_nomb_modalidad_contrato':this.nameContract, 
              "c_des_modalidad_contrato": this.desContract, 
              "b_modalidad_contrato":true
              }).then( (response) =>{
                if(response.data.status === 'success'){
                  this.loadingModalContract = false;
                  this.loadContract();
                  this.nameContract = '';
                  this.desContract = '';
                  this.showTable_contrato = true;
                  this.showRegister_contrato = false;
                  this.$toasted.success(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
                }else{
                  this.loadingModalContract = false;
                  this.$toasted.error(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
                }
              })
              .catch( (error) =>{
                this.loadingModalContract = false;
                this.$toasted.error(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
              })
            }else{
              this.loadingModalContract = false;
              this.loadContract();
              this.nameContract = '';
              this.desContract = '';
              this.showTable_contrato = true;
              this.showRegister_contrato = false;
              this.$toasted.success(response.data.message, {
                duration: 1500,
                position: 'top-center'
              });
            }
          }
          else{
            this.loadingModalContract = false;
            this.$toasted.error(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
          }
        })
        .catch( (error) =>{
          this.loadingModalContract = false;
          this.$toasted.error('Error', {
              duration: 1500,
              position: 'top-center'
            });
        });
      }
    },
    onResetContract(event){
      // event.preventDefault()
      this.nameContract = '';
      this.desContract = '';
      this.showTable_contrato = true,
      this.showRegister_contrato = false
      // this.$nextTick(() => {
      //   this.showTable_contrato = true,
      //   this.showRegister_contrato = false
      // })
    },
    openModalContractDelete(id) {
      this.$root.$emit('bv::show::modal', 'modal-contract-delete-'+id)
    },
    hideModalContractDelete(id,row) {
      this.$root.$emit('bv::hide::modal', 'modal-contract-delete-'+id);
    },
    deleteContract(id){
      this.loadingModalContract = true;
      axios.delete(config.hostname+'maestro/eliminar_modalidad_contratado', {
        data:{
          "id_modalidad_contrato": id
        }
      })
      .then( (response) =>{
        if(response.data.status === 'success'){
          if(config.hosts == true){
            axios.delete(config.hostname_bd+'maestro/eliminar_modalidad_contratado', {
              data:{
                "id_modalidad_contrato": id
              }
            }).then( (response) =>{
              if(response.data.status === 'success'){
                this.loadingModalContract = false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.loadContract();
                this.hideModalContractDelete(id);
              }else{
                this.loadingModalContract = false;
                this.$toasted.error(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.hideModalContractDelete(id);
              }
            })
            .catch( (error) =>{
              this.loadingModalContract = false;
              this.$toasted.error('Error', {
                  duration: 1500,
                  position: 'top-center'
                });
              this.hideModalContractDelete(id);
            })
          }else{
            this.loadingModalContract = false;
            this.$toasted.success(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
            this.loadContract();
            this.hideModalContractDelete(id);
          }
        }
        else{
          this.loadingModalContract = false;
          this.$toasted.error(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
        }
      })
      .catch((error) =>{
        this.loadingModalContract = false;
        this.$toasted.error('Error', {
            duration: 1500,
            position: 'top-center'
        });
      });
    },

    addProfession(){
      if(this.nameProfession == '' || this.nameProfession == null ||
      this.desProfession == '' || this.desProfession == null){
        this.$toasted.error('Ingrese todos los campos', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingModalProfession = true;
        axios.post(config.hostname+'maestro/registrar_profesion',{
          'c_nomb_profesion':this.nameProfession, 
          "c_des_profesion": this.desProfession, 
          "b_profesion":true
        })
        .then( (response) =>{
            if(response.data.status === 'success'){
              if(config.hosts == true){
                axios.post(config.hostname_bd+'maestro/registrar_profesion',{
                'c_nomb_profesion':this.nameProfession, 
                "c_des_profesion": this.desProfession, 
                "b_profesion":true
                }).then( (response) =>{
                  if(response.data.status === 'success'){
                    this.loadingModalProfession = false;
                    this.loadProfession();
                    this.nameProfession = '';
                    this.desProfession = '';
                    this.showTable_profesion = true,
                    this.showRegister_profesion = false
                    this.$toasted.success(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }else{
                    this.loadingModalProfession = false;
                    this.$toasted.error(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }
                })
                .catch( (error) =>{
                  this.loadingModalProfession = false;
                  this.$toasted.error('Error', {
                    duration: 1500,
                    position: 'top-center'
                  });
                })
              }else{
                 this.loadingModalProfession = false;
                  this.loadProfession();
                  this.nameProfession = '';
                  this.desProfession = '';
                  this.showTable_profesion = true,
                  this.showRegister_profesion = false
                  this.$toasted.success(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
              }
              
            }else{
              this.loadingModalProfession = false;
              this.$toasted.error(response.data.message, {
                duration: 1500,
                position: 'top-center'
              });
            }
        })
        .catch( (error) =>{
          this.loadingModalProfession = false;
          this.$toasted.error('Error', {
            duration: 1500,
            position: 'top-center'
          });
        });
      }
    },
    onResetProfession(event){
      // event.preventDefault()
      this.nameProfession = '';
      this.desProfession = '';
      this.showTable_profesion = true;
      this.showRegister_profesion = false;
      // this.$nextTick(() => {
      //   this.showTable_profesion = true,
      //   this.showRegister_profesion = false
      // })
    },
    openModalProfessionDelete(id) {
      this.$root.$emit('bv::show::modal', 'modal-profession-delete-'+id)
    },
    hideModalProfessionDelete(id,row) {
      this.$root.$emit('bv::hide::modal', 'modal-profession-delete-'+id);
    },
    deleteProfession(id){
      this.loadingModalProfession = true;
      axios.delete(config.hostname+'maestro/eliminar_profesion', {
        data:{
          "id_profesion": id
        }
      })
      .then( (response) =>{
        if(response.data.status === 'success'){
          if(config.hosts == true){
            axios.delete(config.hostname_bd+'maestro/eliminar_profesion', {
              data:{
                "id_profesion": id
              }
            }).then( (response) =>{
              if(response.data.status === 'success'){
                this.loadingModalProfession = false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.loadProfession();
                this.hideModalProfessionDelete(id);
              }else{
                this.loadingModalProfession = false;
                this.$toasted.error(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.hideModalProfessionDelete(id);
              }
            })
            .catch( (error) =>{
              this.loadingModalProfession = false;
              this.$toasted.error('Error', {
                duration: 1500,
                position: 'top-center'
              });
              this.hideModalProfessionDelete(id);
            })
          }else{
            this.loadingModalProfession = false;
            this.$toasted.success(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
            this.loadProfession();
            this.hideModalProfessionDelete(id);
          }
        }
        else{
          this.loadingModalProfession = false;
          this.$toasted.error(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
          this.hideModalProfessionDelete(id);
        }
      })
      .catch((error) =>{
        this.loadingModalProfession = false;
        this.$toasted.error('Error', {
          duration: 1500,
          position: 'top-center'
        });
        this.hideModalProfessionDelete(id);
      });
    },




    openModalPositionDelete(id) {
      this.$root.$emit('bv::show::modal', 'modal-position-delete-'+id)
    },
    hideModalPositionDelete(id,row) {
      this.$root.$emit('bv::hide::modal', 'modal-position-delete-'+id);
    },
    deletePosition(id){
      this.loadingModalPosition = true;
      axios.delete(config.hostname+'maestro/eliminar_eliminar_cargos', {
        data:{
          "id_cargo": id
        }
      })
      .then( (response) =>{
        if(response.data.status === 'success'){
          if(config.hosts == true){
            axios.delete(config.hostname_bd+'maestro/eliminar_eliminar_cargos', {
              data:{
                "id_cargo": id
              }
            }).then( (response) =>{
              if(response.data.status === 'success'){
                this.loadingModalPosition= false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.loadPositions();
                this.hideModalPositionDelete(id);
              }else{
                this.loadingModalPosition= false;
                this.$toasted.error(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.hideModalPositionDelete(id);
              }
            })
            .catch( (error) =>{
              this.loadingModalPosition= false;
              this.$toasted.error('Error', {
                duration: 1500,
                position: 'top-center'
              });
              this.hideModalPositionDelete(id);
            })
          }else{
            this.loadingModalPosition= false;
            this.$toasted.success(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
            this.loadPositions();
            this.hideModalPositionDelete(id);
          }
        }
        else{
          this.loadingModalPosition= false;
          this.$toasted.error(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
          this.hideModalPositionDelete(id);
        }
      })
      .catch((error) =>{
        this.loadingModalPosition= false;
        this.$toasted.error('Error', {
          duration: 1500,
          position: 'top-center'
        });
        this.hideModalPositionDelete(id);
      });
    },
    addPosition(){
      if(this.namePosition == '' || this.namePosition == null ||
      this.desPosition == '' || this.desPosition == null){
        this.$toasted.error('Ingrese todos los campos', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingModalPosition = true;
        axios.post(config.hostname+'maestro/registrar_registrar_cargos',{
          'c_nombres_cargo':this.namePosition, 
          "c_desc_cargo": this.desPosition, 
          "b_area_trabajo":true
        })
        .then( (response) =>{
            if(response.data.status === 'success'){
              if(config.hosts == true){
                axios.post(config.hostname_bd+'maestro/registrar_registrar_cargos',{
                  'c_nombres_cargo':this.namePosition, 
                  "c_desc_cargo": this.desPosition, 
                  "b_area_trabajo":true
                }).then( (response) =>{
                  if(response.data.status === 'success'){
                    this.loadingModalPosition = false;
                    this.loadPositions();
                    this.namePosition = '';
                    this.desPosition = '';
                    this.showTable_position = true;
                    this.showRegister_position = false;
                    this.$toasted.success(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }else{
                    this.loadingModalPosition = false;
                    this.$toasted.error(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }
                })
                .catch( (error) =>{
                  this.loadingModalPosition = false;
                  this.$toasted.error('Error', {
                    duration: 1500,
                    position: 'top-center'
                  });
                })
              }else{
                this.loadingModalPosition = false;
                this.loadPositions();
                this.namePosition = '';
                this.desPosition = '';
                this.showTable_position = true;
                this.showRegister_position = false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
              }
            }else{
              this.loadingModalPosition = false;
              this.$toasted.error(response.data.message, {
                duration: 1500,
                position: 'top-center'
              });
            }
        })
        .catch( (error) =>{
          this.loadingModalPosition = false;
          this.$toasted.error('Error', {
            duration: 1500,
            position: 'top-center'
          });
        });
      }
    },
    onResetPosition(event){
      // event.preventDefault()
      this.namePosition = '';
      this.desPosition = '';
      this.showTable_position = true;
      this.showRegister_position = false;
      // this.$nextTick(() => {
      //   this.showTable_area = true,
      //   this.showRegister_area = false
      // })
    },


    addArea(){
      if(this.nameArea == '' || this.nameArea == null ||
      this.desArea == '' || this.desArea == null){
        this.$toasted.error('Ingrese todos los campos', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingModalArea = true;
        axios.post(config.hostname+'maestro/registrar_area_trabajo',{
          'c_nomb_area_trabajo':this.nameArea, 
          "c_des_area_trabajo": this.desArea, 
          "b_area_trabajo":true
        })
        .then( (response) =>{
            if(response.data.status === 'success'){
              if(config.hosts == true){
                axios.post(config.hostname_bd+'maestro/registrar_area_trabajo',{
                  'c_nomb_area_trabajo':this.nameArea, 
                  "c_des_area_trabajo": this.desArea, 
                  "b_area_trabajo":true
                }).then( (response) =>{
                  if(response.data.status === 'success'){
                    this.loadingModalArea = false;
                    this.loadArea();
                    this.nameArea = '';
                    this.desArea = '';
                    this.showTable_area = true;
                    this.showRegister_area = false;
                    this.$toasted.success(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }else{
                    this.loadingModalArea = false;
                    this.$toasted.error(response.data.message, {
                      duration: 1500,
                      position: 'top-center'
                    });
                  }
                })
                .catch( (error) =>{
                  this.loadingModalArea = false;
                  this.$toasted.error('Error', {
                    duration: 1500,
                    position: 'top-center'
                  });
                })
              }else{
                this.loadingModalArea = false;
                this.loadArea();
                this.nameArea = '';
                this.desArea = '';
                this.showTable_area = true;
                this.showRegister_area = false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
              }
            }else{
              this.loadingModalArea = false;
              this.$toasted.error(response.data.message, {
                duration: 1500,
                position: 'top-center'
              });
            }
        })
        .catch( (error) =>{
          this.loadingModalArea = false;
          this.$toasted.error('Error', {
            duration: 1500,
            position: 'top-center'
          });
        });
      }
    },
    onResetArea(event){
      // event.preventDefault()
      this.nameArea = '';
      this.desArea = '';
      this.showTable_area = true;
      this.showRegister_area = false;
      // this.$nextTick(() => {
      //   this.showTable_area = true,
      //   this.showRegister_area = false
      // })
    },

    addType(){
      if(this.nameType == '' || this.nameType == null){
        this.$toasted.error('Ingrese todos los campos', {
          duration: 1500,
          position: 'top-center'
        });
      }
      else{
        this.loadingModalType = true;
        axios.post(config.hostname+'maestro/registrar_tipo_doc_exp',{
          'c_nomb_tipo_doc_exp':this.nameType, 
          "b_tipo_doc_exp":true
        })
        .then( (response) =>{
          if(response.data.status === 'success'){
            if(config.hosts == true) {
              axios.post(config.hostname_bd+'maestro/registrar_tipo_doc_exp',{
                'c_nomb_tipo_doc_exp':this.nameType, 
                "b_tipo_doc_exp":true
              }).then( (response) =>{
                if(response.data.status === 'success'){
                  this.loadingModalType = false;
                  this.loadTypeDoc();
                  this.nameType = '';
                  this.desType = '';
                  this.showTable_tipo = true,
                  this.showRegister_tipo = false
                  this.$toasted.success(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
                }else{
                  this.loadingModalType = false;
                  this.$toasted.error(response.data.message, {
                    duration: 1500,
                    position: 'top-center'
                  });
                }
              })
              .catch( (error) =>{
                this.loadingModalType = false;
                this.$toasted.error('Error', {
                  duration: 1500,
                  position: 'top-center'
                });
              })
            }else{
              this.loadingModalType = false;
              this.loadTypeDoc();
              this.nameType = '';
              this.desType = '';
              this.showTable_tipo = true,
              this.showRegister_tipo = false
              this.$toasted.success(response.data.message, {
                duration: 1500,
                position: 'top-center'
              });
            }
          }else{
            this.loadingModalType = false;
            this.$toasted.error(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
          }
        })
        .catch( (error) =>{
          this.loadingModalType = false;
          this.$toasted.error('Error', {
            duration: 1500,
            position: 'top-center'
          });
        });
      }
    },
    onResetType(event){
      // event.preventDefault()
      this.nameType = '';
      this.desType = '';
      this.showTable_tipo = true;
      this.showRegister_tipo = false;
      // this.$nextTick(() => {
      //   this.showTable_tipo = true,
      //   this.showRegister_tipo = false
      // })
    },
    openModalTypeDelete(id) {
      this.$root.$emit('bv::show::modal', 'modal-type-delete-'+id)
    },
    hideModalTypeDelete(id,row) {
      this.$root.$emit('bv::hide::modal', 'modal-type-delete-'+id);
    },
    deleteType(id){
      this.loadingModalType = true;
      axios.delete(config.hostname+'maestro/eliminar_eliminar_tipo_doc_exp', {
        data:{
          "id_tipo_doc_exp": id
        }
      })
      .then( (response) =>{
        if(response.data.status === 'success'){
          if(config.hosts == true){
            axios.delete(config.hostname_bd+'maestro/eliminar_eliminar_tipo_doc_exp', {
              data:{
                "id_tipo_doc_exp": id
              }
            }).then( (response) =>{
              if(response.data.status === 'success'){
                this.loadingModalType = false;
                this.$toasted.success(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.loadTypeDoc();
                this.hideModalTypeDelete(id);
              }else{
                this.loadingModalType = false;
                this.$toasted.error(response.data.message, {
                  duration: 1500,
                  position: 'top-center'
                });
                this.hideModalTypeDelete(id);
              }
            })
          }else{
            this.loadingModalType = false;
            this.$toasted.success(response.data.message, {
              duration: 1500,
              position: 'top-center'
            });
            this.loadTypeDoc();
            this.hideModalTypeDelete(id);
          }
        }
        else{
          this.loadingModalType = false;
          this.$toasted.error(response.data.message, {
          duration: 1500,
          position: 'top-center'
          });
          this.hideModalTypeDelete(id);
        }
      })
      .catch((error) =>{
        this.loadingModalType = false;
        this.$toasted.error('Error', {
          duration: 1500,
          position: 'top-center'
        });
        this.hideModalTypeDelete(id);
      });
    },
    loadTypeDoc(){
      axios.get(config.hostname+'maestro/obtener_tipo_doc_exp')
      .then( (response) =>{
        this.typeDoc = response.data.data;
        this.loadingTableTypeDoc = false;
        this.showTableTypeDoc = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadProfession(){
       axios.get(config.hostname+'maestro/obtener_profesion')
      .then( (response) =>{
        this.profession = response.data.data;
        this.loadingTableProfession = false;
        this.showTableProfession = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadArea(){
       axios.get(config.hostname+'maestro/obtener_area_trabajo')
      .then( (response) =>{
        this.area = response.data.data;
        this.loadingTableArea = false;
        this.showTableArea = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadPositions(){
      axios.get(config.hostname+'maestro/obtener_cargos')
      .then( (response) =>{
        this.position = response.data.data;
        this.loadingTablePosition = false;
        this.showTablePosition = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    loadContract(){
       axios.get(config.hostname+'maestro/obtener_modalidad_contratado')
      .then( (response) =>{
        this.contract = response.data.data;
        this.loadingTableContract = false;
        this.showTableContract = true;
      })
      .catch( (error) =>{
        console.log(error);
      });
    },
    onRegisterContrato(){
      // this.showTable_contrato = false;
      // this.showRegister_contrato = true
    },
    onRegisterPago(){
      // this.showTable_pago = false;
      // this.showRegister_pago = true
    },

    onSubmitPago(event) {
      // console.log('gg mrd')
      event.preventDefault()
      // alert(JSON.stringify(this.form))
      this.showTable_pago= true;
      this,this.showRegister_pago = false;
    },
 
    onResetContrato(event) {
      //event.preventDefault()
      this.form.nameDoc = ''
      this.form.codeDoc = ''
      this.form.timeDoc = null
      this.show = true,
      this.showTable_contrato = true,
      this.showRegister_contrato = false
      // this.$nextTick(() => {
      //   this.show = true,
      //   this.showTable_contrato = true,
      //   this.showRegister_contrato = false
      // })
    },
   
    onSubmitContrato(event) {
      event.preventDefault()
      // alert(JSON.stringify(this.form))
      this.showTable_contrato= true;
      this,this.showRegister_contrato = false;
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
  created(){
    let user = JSON.parse(window.localStorage.getItem('user'));
    if (user.id_area_trabajo !=8) {
     this.$router.push('/app/partes');
    }
    this.loadTypeDoc();
    this.loadArea();
    this.loadContract();
    this.loadProfession();
    this.loadPositions();
  }
};
</script>

<style src="./Maestro.scss" lang="scss" scoped />