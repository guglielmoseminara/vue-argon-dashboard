<template>
    <li class="nav-item" v-if="link.path">
        <router-link
                :to="link.path"
                @click.native="linkClick"
                class="nav-link"
                :target="link.target"
                :href="link.path">
            <template>
                <i :class="link.icon"></i>
                <span class="nav-link-text">{{ link.name }}</span>
            </template>
        </router-link>
    </li>
    <li class="nav-item" v-else @click="toggleCollapse">
        <a data-toggle="collapse" class="sidebar-menu-item nav-link" :aria-expanded=collapsed >
          <i :class="link.icon"></i>
          <span class="nav-link-text">{{ link.name }}</span>
        </a>
        <div v-if="$slots.default && $slots.default.length > 0" @click="$event.stopPropagation()" class="collapse" v-bind:class="{'show': collapsed}" style="animation-fill-mode: both; animation-timing-function: ease-out;">
          <slot></slot>
        </div>
    </li>
</template>
<script>
  export default {
    name: 'sidebar-item',
    props: {
      link: {
        type: Object,
        default: () => {
          return {
            name: '',
            path: '',
            children: []
          };
        },
        description:
          'Sidebar link. Can contain name, path, icon and other attributes. See examples for more info'
      }
    },
    inject: {
      autoClose: {
        default: true
      }
    },
    mounted() {
      console.log(this.$slots);
    },
    data() {
      return {
        children: [],
        collapsed: false
      };
    },
    methods: {
      linkClick() {
        if (
          this.autoClose &&
          this.$sidebar &&
          this.$sidebar.showSidebar === true
        ) {
          this.$sidebar.displaySidebar(false);
        }
      },
      toggleCollapse() {
        this.collapsed = !this.collapsed;
        console.log(this.collapsed);
      }
    }
  };
</script>