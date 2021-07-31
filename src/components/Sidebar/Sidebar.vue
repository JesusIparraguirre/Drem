<template>
  <div class="sidebar-wrapper">
    <nav
        :class="{sidebar: true, sidebarStatic, sidebarOpened}"
        @mouseenter="sidebarMouseEnter"
        @mouseleave="sidebarMouseLeave"
    >
      <header class="logo">
        <!-- <router-link to="/app/dashboard"><span class="primary-word" style="font-size: 15px">Drem</span> <span class="secondary-word"> Piura</span></router-link> -->
        <a><span class="secondary-word">DREM PIURA</span></a>
      </header>
      <h5 class="navTitle first">
        Areas
      </h5>
      <ul class="nav">
        <NavLink
            :activeItem="activeItem"
            header="Mesa de partes"
            link="/app/partes"
            iconName="flaticon-archive"
            index="partes"
            isHeader
        />
        <NavLink
            :activeItem="activeItem"
            header="Dirección"
            link="/app/direccion"
            iconName="flaticon-users"
            index="direccion"
            isHeader
        />
        <NavLink
            :activeItem="activeItem"
            header="Hidrocarburo"
            link="/app/hidrocarburo"
            iconName="flaticon-cloud"
            index="hidrocarburo"
            isHeader
        />
        <NavLink
            :activeItem="activeItem"
            header="Mineria"
            link="/app/mineria"
            iconName="flaticon-diamond"
            index="mineria"
            isHeader
        />
        <NavLink
            :activeItem="activeItem"
            header="Energia"
            link="/app/energia"
            iconName="flaticon-star"
            index="energia"
            isHeader
        />
        <NavLink
            :activeItem="activeItem"
            header="Legal"
            link="/app/legal"
            iconName="flaticon-document"
            index="legal"
            isHeader
        />
        <!-- <NavLink
            :activeItem="activeItem"
            header="Administracion"
            link="/app/administracion"
            iconName="flaticon-home"
            index="administracion"
            isHeader
        /> -->
        <NavLink v-if="user.id_area_trabajo == 8"
            :activeItem="activeItem"
            header="Historial"
            link="/app/historial"
            iconName="flaticon-trash"
            index="historial"
            isHeader
        />
        <NavLink v-if="user.id_area_trabajo == 8"
            :activeItem="activeItem"
            header="Reporte"
            link="/app/reporte"
            iconName="flaticon-database"
            index="reporte"
            isHeader
        />

        <NavLink v-if="user.id_area_trabajo == 8"
            :activeItem="activeItem"
            header="Log"
            link="/app/log"
            iconName="flaticon-flag"
            index="log"
            isHeader
        />
        
        <NavLink v-if="user.id_area_trabajo == 8"
            :activeItem="activeItem"
            header="Soporte"
            link="/app/soporte"
            iconName="flaticon-settings"
            index="soporte"
            :childrenLinks="[
              { header: 'Usuarios', link: '/app/soporte/usuario' },
              { header: 'Personal', link: '/app/soporte/personal' },
              { header: 'Maestros', link: '/app/soporte/maestro' },
            ]"
        />

        <!-- <NavLink
            :activeItem="activeItem"
            header="Log"
            link="/app/log"
            iconName="flaticon-network"
            index="log"
            isHeader
        /> -->

        <!-- <NavLink
            :activeItem="activeItem"
            header="Dashboard"
            link="/app/dashboard"
            iconName="flaticon-home"
            index="dashboard"
            isHeader
        /> -->
        <!-- <NavLink
            :activeItem="activeItem"
            header="Typography"
            link="/app/typography"
            iconName="flaticon-list"
            index="typography"
            isHeader
        /> -->
        <!-- <NavLink
            :activeItem="activeItem"
            header="Tables Basic"
            link="/app/tables"
            iconName="flaticon-equal-1"
            index="tables"
            isHeader
        /> -->
        <!-- <NavLink
            :activeItem="activeItem"
            header="Notifications"
            link="/app/notifications"
            iconName="flaticon-bell"
            index="notifications"
            isHeader
        /> -->
        <!-- <NavLink
            :activeItem="activeItem"
            header="Soporte"
            link="/app/components"
            iconName="flaticon-network"
            index="components"
            :childrenLinks="[
              { header: 'Maestros', link: '/app/components/charts' },
              { header: 'Usuarios', link: '/app/components/icons' },
              //{ header: 'Maps', link: '/app/components/maps' },
            ]"
        /> -->
      </ul>
      
     <!-- <h5 class="navTitle">
        LABELS
      </h5>
      <ul class="sidebarLabels">
        <li>
          <a href="#">
            <i class="fa fa-circle text-danger"/>
            <span class="labelName">Core</span>
          </a>
        </li>
        <li>
          <a href="#">
            <i class="fa fa-circle text-primary"/>
            <span class="labelName">UI Elements</span>
          </a>
        </li>
        <li>
          <a href="#">
            <i class="fa fa-circle text-success"/>
            <span class="labelName">Forms</span>
          </a>
        </li>
      </ul>-->
      <!--<h5 class="navTitle">
        PROJECTS
      </h5>
      <div class="sidebarAlerts">
        <b-alert
            v-for="alert in alerts"
            :key="alert.id"
            class="sidebarAlert" variant="transparent"
            show dismissible
        >
          <span>{{alert.title}}</span><br/>
          <b-progress class="sidebarProgress progress-xs mt-1"
                      :variant="alert.color" :value="alert.value" :max="100"/>
          <small>{{alert.footer}}</small>
        </b-alert>
      </div>-->
    </nav>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import isScreen from '@/core/screenHelper';
import NavLink from './NavLink/NavLink';

export default {
  name: 'Sidebar',
  components: { NavLink },
  data() {
    return {
      user:{},
      alerts: [
        {
          id: 0,
          title: 'Sales Report',
          value: 15,
          footer: 'Calculating x-axis bias... 65%',
          color: 'danger',
        },
        {
          id: 1,
          title: 'Personal Responsibility',
          value: 20,
          footer: 'Provide required notes',
          color: 'primary',
        },
      ],
    };
  },
  methods: {
    ...mapActions('layout', ['changeSidebarActive', 'switchSidebar']),
    setActiveByRoute() {
      const paths = this.$route.fullPath.split('/');
      paths.pop();
      this.changeSidebarActive(paths.join('/'));
    },
    sidebarMouseEnter() {
      if (!this.sidebarStatic && (isScreen('lg') || isScreen('xl'))) {
        this.switchSidebar(false);
        this.setActiveByRoute();
      }
    },
    sidebarMouseLeave() {
      if (!this.sidebarStatic && (isScreen('lg') || isScreen('xl'))) {
        this.switchSidebar(true);
        this.changeSidebarActive(null);
      }
    },
  },
  created() {
    this.setActiveByRoute();
    this.user = JSON.parse(window.localStorage.getItem('user'));
  },
  computed: {
    ...mapState('layout', {
      sidebarStatic: state => state.sidebarStatic,
      sidebarOpened: state => !state.sidebarClose,
      activeItem: state => state.sidebarActiveElement,
    }),
  },
};
</script>

<!-- Sidebar styles should be scoped -->
<style src="./Sidebar.scss" lang="scss" scoped/>
