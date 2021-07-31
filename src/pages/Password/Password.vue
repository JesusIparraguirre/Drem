<template>
<div class="log-page">
        <h2 class="page-title">Cuenta <span class="fw-semi-bold"></span></h2>
    <b-row>
      <b-col>
        <Widget
          title="<h5>Actualizar <span class='fw-semi-bold'>Contraseña</span></h5>"
          customHeader
        >
        <br>
        <div class="data-loader" v-if="loadingData">
          <i class="la la-spinner la-spin"></i>
        </div>
        <b-row>
            <b-col cols="0" sm="2" md="3"></b-col>
            <b-col cols="12" sm="8" md="6">
            <div class="ml-5 mr-5 text-center" v-show="!loadingData">
            <form class="mt" @submit.prevent="updatePassword">
            <div class="form-group">
              <input class="form-control no-border" ref="email" required type="email" name="email" placeholder="Email" v-model="email" disabled/>
            </div>
            <div class="form-group">
              <input class="form-control" ref="email" required type="password" name="password" placeholder="Contraseña" v-model="password"/>
            </div>
            <div class="form-group">
              <input class="form-control" ref="new_password" required type="password" name="new_password" placeholder="Nueva contraseña" v-model="new_password"/>
            </div>
            <div class="text-right">
                <b-button type="submit" size="block" class="auth-btn mb-1 text-left" variant="inverse" v-if="!loading">Actualizar contraseña</b-button>
            </div>
            <div class="data-loader">
            <i class="la la-spinner la-spin" v-if="loading"></i>
          </div>
          </form>
          </div>
        </b-col>
        <b-col cols="0" sm="2" md="3"></b-col>
        </b-row>
        </Widget>
      </b-col>
    </b-row>
</div>
  
</template>
<script>
import Vue from 'vue';
import axios from 'axios';
import config from '../../config';
import Widget from '@/components/Widget/Widget';
import Sparklines from '../../components/Sparklines/Sparklines'

import VueTextareaAutosize from "vue-textarea-autosize";
Vue.use(VueTextareaAutosize);

export default {
  name: 'Password',
  components: {
    Widget,
    Sparklines,
  },
  data() {
    return { 
      loadingData : true,
      loading: false,
      email: '',
      password: '',
      new_password: '',
    };
  },
  methods: {

  },
  computed: {
    updatePassword(){
      this.loading = true,
      axios.put(config.hostname+'user/actualizar_contrasena', {
      c_usuario: this.email,
      c_contrasena_usuario: this.password,
      c_contrasena_usuario_nueva: this.new_password
      })
      .then( (response) =>{
        this.loading = false;
        this.email = '';
        this.password = '';
        if(response.data.status === 'success'){
          this.$toasted.success(response.data.message, {
            duration: 2000,
            position: 'top-center'
          });
          setTimeout( ()=> {
            this.$router.push('/app/partes');
          }, 500);
        }
        else{
          this.$toasted.error(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
        }
      })
      .catch( (error) =>{
        console.log(error);
      });
    }
  },
  created(){
    let data = JSON.parse(window.localStorage.getItem('user'));
    this.email = data.c_usuario;
    this.loadingData = false;
  }
};
</script>

<style src="./Password.scss" lang="scss" scoped />