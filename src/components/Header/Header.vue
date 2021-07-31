<template>
  <b-navbar class="header d-print-none app-header">
    <b-nav>
      <b-nav-item>
        <a class="d-md-down-none px-2" href="#" @click="toggleSidebarMethod" id="barsTooltip">
          <i class='fi flaticon-menu' />
        </a>
        <a class="fs-lg d-lg-none" href="#" @click="switchSidebarMethod">
          <i class='fi flaticon-menu' />
        </a>
      </b-nav-item>
      <!--<b-nav-item class="d-md-down-none">
        <a href="#" class="px-2">
          <i class='fi flaticon-flip' />
        </a>
      </b-nav-item>-->
     <!--<b-nav-item class="d-md-down-none">
        <a href="#" class="px-2">
          <i class='fi flaticon-close' />
        </a>
      </b-nav-item>-->
    </b-nav>
    <!-- <b-nav>
      <b-form class="d-sm-down-none ml-5" inline>
        <b-form-group>
          <b-input-group class="input-group-no-border">
            <template v-slot:prepend>
              <b-input-group-text><i class='fi flaticon-search-2'/></b-input-group-text>
            </template>
            <b-form-input id="search-input" placeholder="Buscar..." />
          </b-input-group>
        </b-form-group>
      </b-form>
    </b-nav> -->
    <a class="navbarBrand d-md-none">
      <i class="fa fa-circle text-danger" />
      &nbsp;
      DremPiura
      &nbsp;
      <i class="fa fa-circle text-primary" />
    </a>
    <b-nav class="ml-auto">
      <b-nav-item-dropdown
        class="notificationsMenu d-sm-down-none mr-2"
        menu-class="notificationsWrapper py-0 animate__animated animate__animated-fast animate__fadeIn"
        right>
        <template slot="button-content">
          <span class="avatar rounded-circle thumb-sm float-left mr-2">
            <img class="rounded-circle" src="../../assets/people/user.png" alt="..." />
          </span>
          <span class="px-2">{{c_nombres_personal}}</span>
          <!--<span class="ml-1 mr-2 circle text-white fw-bold avatar-badge">9</span>
          <i class='fi flaticon-arrow-down px-2' />-->
        </template>
        <!--<Notifications />-->
      </b-nav-item-dropdown>
      <b-nav-item-dropdown id="v-step-2" class="settingsDropdown d-sm-down-none" no-caret right>
        <template slot="button-content">
          <i class='fi flaticon-settings-10 px-2' />
        </template>
        <b-dropdown-item-button @click="profile">
          <i class="fi flaticon-power-1 px-3 mr-3" /> Mi cuenta
        </b-dropdown-item-button>
        <b-dropdown-item-button @click="password">
          <i class="fi flaticon-power-1 px-3 mr-3" /> Actualizar<br>   contraseña
        </b-dropdown-item-button>
        <!-- <b-dropdown-item><i class='fi flaticon-person px-3 mr-3' @click="perfil"/> Mi cuenta</b-dropdown-item> -->
        <b-dropdown-divider />
        <!--<b-dropdown-item><i class='fi flaticon-calendar-9 px-3 mr-3' />Calendar</b-dropdown-item>-->
        <!--<b-dropdown-item><i class='fi flaticon-email px-3 mr-3' />
          Inbox &nbsp;&nbsp;<b-badge variant="inverse" pill class="animate__animated animate__bounceIn" style="padding: 6px 9px;">9</b-badge>
        </b-dropdown-item>-->
        <!--<b-dropdown-divider />-->
        <b-dropdown-item-button @click="logout">
          <i class="fi flaticon-power-1 px-3 mr-3" /> Salir
        </b-dropdown-item-button>
      </b-nav-item-dropdown>
    </b-nav>
  </b-navbar>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import Notifications from '@/components/Notifications/Notifications';

export default {
  name: 'Header',
  components: { Notifications },
  data() {
    return {
      user: {
        c_nombres_personal:''
      }
    };
  },
  computed: {
    ...mapState('layout', ['sidebarClose', 'sidebarStatic']),
  },
  methods: {
    ...mapActions('layout', ['toggleSidebar', 'switchSidebar', 'changeSidebarActive']),
    switchSidebarMethod() {
      if (!this.sidebarClose) {
        this.switchSidebar(true);
        this.changeSidebarActive(null);
      } else {
        this.switchSidebar(false);
        const paths = this.$route.fullPath.split('/');
        paths.pop();
        this.changeSidebarActive(paths.join('/'));
      }
    },
    toggleSidebarMethod() {
      if (this.sidebarStatic) {
        this.toggleSidebar();
        this.changeSidebarActive(null);
      } else {
        this.toggleSidebar();
        const paths = this.$route.fullPath.split('/');
        paths.pop();
        this.changeSidebarActive(paths.join('/'));
      }
    },
    logout() {
      window.localStorage.setItem('authenticated', false);
      window.localStorage.removeItem('user');
      window.localStorage.removeItem('token');
      this.$router.push('/login');
    },
    profile(){
      this.$router.push('/app/account');
    },
    password(){
      this.$router.push('/app/password');
    }
  },
  created() {
    let data = JSON.parse(window.localStorage.getItem('user'));
    this.c_nombres_personal = data.c_nombres_personal;
  }
};
</script>

<style src="./Header.scss" lang="scss"></style>
