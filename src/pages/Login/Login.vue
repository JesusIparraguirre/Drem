<template>
  <div class="auth-page">
    <b-container>
      <h5 v-if="!showRegister" class="auth-logo">
        <i class="fa fa-circle text-primary"></i>
        Iniciar sesión
        <i class="fa fa-circle text-danger"></i>
      </h5>
      <h5 v-if="showRegister" class="auth-logo">
        <i class="fa fa-circle text-primary"></i>
        Recuperar contraseña
        <i class="fa fa-circle text-danger"></i>
      </h5>
      <Widget v-if="!showRegister" class="widget-auth mx-auto" title="<h3 class='mt-0'>Inicie sesión en Drem - Piura</h3>" customHeader>
        <p class="widget-auth-info">
            Use su Email para iniciar sesión.
        </p>
        <form class="mt" @submit.prevent="login">
          <b-alert class="alert-sm" variant="danger" :show="!!errorMessage">
            {{errorMessage}}
          </b-alert>
          <div class="form-group">
            <input class="form-control no-border" ref="email" required type="email" name="email" placeholder="Email" v-model="email"/>
          </div>
          <div class="form-group">
            <input class="form-control no-border" ref="password" required type="password" name="password" placeholder="contraseña" v-model="password"/>
          </div>
          <b-button type="submit" size="sm" class="auth-btn mb-1" variant="inverse" v-if="!loading">Iniciar sesión</b-button>
          <a href="#" class="d-block text-right" @click="showRegister = true" v-if="!loading">Recuperar contraseña</a>
        </form>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loading"></i>
        </div>
      </Widget>

      <Widget v-if="showRegister" class="widget-auth mx-auto" title="<h3 class='mt-0'>Recuperar contraseña</h3>" customHeader>
        <p class="widget-auth-info">
            <br>
            Use su Email para recuperar su contraseña.
        </p>
        <form class="mt" @submit.prevent="rememberPassword">
          <b-alert class="alert-sm" variant="danger" :show="!!errorMessage">
            {{errorMessage}}
          </b-alert>
          <div class="form-group">
            <input class="form-control no-border" ref="email" required type="email" name="email" placeholder="Email" v-model="email"/>
          </div>
          <b-button type="submit" size="sm" class="auth-btn mb-1" variant="inverse" v-if="!loading">Recuperar contraseña</b-button>
          <a href="#" class="d-block text-left" @click="showRegister = false" v-if="!loading">Volver</a>
          <!-- <router-link class="d-block text-right" to="login" @click="showRegister = false">Recuperar contraseña</router-link> -->
          <!--<p class="widget-auth-info">or sign in with</p>-->
          <!--<div class="social-buttons">
            <b-button variant="primary" class="social-button mb-2">
              <i class="social-icon social-google"></i>
              <p class="social-text">GOOGLE</p>
            </b-button>
            <b-button variant="success" class="social-button">
              <i class="social-icon social-microsoft"></i>
              <p class="social-text">MICROSOFT</p>
            </b-button>
          </div>-->
        </form>
        <div class="data-loader">
          <i class="la la-spinner la-spin" v-if="loading"></i>
        </div>
        <!--<p class="widget-auth-info">
          Don't have an account? Sign up now!
        </p>
        <router-link class="d-block text-center" to="login">Create an Account</router-link>-->
      </Widget>
    </b-container>
    <footer class="auth-footer">
      2021 &reg; Registrado por <a href="http://drem.regionpiura.gob.pe/">Dream-Piura</a>
    </footer>
  </div>
</template>

<script>
import Widget from '@/components/Widget/Widget';
import config from '../../config';
export default {
  name: 'LoginPage',
  components: { Widget },
  data() {
    return {
      ip:config.ip,
      email: '',
      password: '',
      loading:false,
      errorMessage: null,
      showRegister: false,
    };
  },
  methods: {
    recuperar(){
      this.showRegister = true;
    },
    login() {
      this.loading = true,
      axios.post(config.hostname+'user/signin', {
      c_usuario: this.email,
      c_contrasena_usuario: this.password
      })
      .then( (response) =>{
        this.loading = false;
        if(response.data.message.substring(0,10) === 'Bienvenido'){
          this.$toasted.success(response.data.message, {
            duration: 1500,
            position: 'top-center'
          });
          setTimeout( ()=> {
              this.$router.push('/app/partes');
              window.localStorage.setItem('authenticated', true);
              window.localStorage.setItem('user',JSON.stringify(response.data.usuario[0]));
              window.localStorage.setItem('token',response.data.token);
          }, 1000);
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
    },
    rememberPassword(){
      this.loading = true,
      axios.post(config.hostname+'user/restablecer_contrasena', {
      c_usuario: this.email,
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
              this.showRegister = false;
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
  created() {
    if (window.localStorage.getItem('authenticated') === 'true') {
      this.$router.push('/app/main/analytics');
    }
  },
};
</script>
<style src="./Loader.scss" lang="scss"/>