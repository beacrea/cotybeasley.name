<template>
  <div id="app" v-bind:class="activeBg">
    <span id="overlay" v-bind:style="{ opacity: blurAbout }"></span>
    <transition name="fade" mode="out-in" v-on:after-enter="afterEnter" appear>
      <router-view v-on:introStatus="introBlurred" v-on:pageChange="changeBG" v-on:hideNav="hideNav" v-on:showNav="showNav" />
    </transition>
    <transition name="peek" mode="out-in" v-on:after-enter="afterEnter" appear>
      <Navigation v-show="nav" v-on:pageChange="changeBG" v-on:introStatus="introBlurred"></Navigation>
    </transition>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import Navigation from './components/Navigation'

Vue.use(VueResource)

export default {
  components: {Navigation},
  name: 'App',
  data () {
    return {
      activeBg: 'pg-' + this.$route.name,
      blurAbout: 0,
      nav: true
    }
  },
  methods: {
    changeBG: function (e) {
      this.activeBg = 'pg-' + this.$route.name
    },
    hideNav: function () {
      this.nav = false
    },
    showNav: function () {
      this.nav = true
    },
    introBlurred: function (trigger) {
      this.blurAbout = trigger
    },
    afterEnter: function (el, done) {
      console.log('Page changed to: ' + this.$route.name)
    }
  }
}
</script>

<style lang="scss">
// Imports
@import "globalStyles/global";

#app {
  border: none;
  display: flex;
  flex-direction: column;
  height: 100%;
  background: #0B0E1F no-repeat top center;
  background-size: cover;
  color: $color_light;
  transition-duration: 1s;
  position: relative;
  z-index: 0;
  > div {
    flex: 1 1 auto;
    z-index: 2;
    -webkit-transform:translateZ(0px);
  }
  &.pg-intro {
    background-image: url('/static/img/bg-about-sm.jpg');
  }
  &.pg-intro-blurred {
    background-image: url('/static/img/bg-about-sm-blurred.jpg');
  }
  &.pg-stats {
    background-image: url('/static/img/bg-stats.jpg');
  }
  &.pg-contact {
    background-image: url('/static/img/bg-contact.jpg');
  }
  &.pg-work, &.pg-workdetail {
    background-image: url('/static/img/bg-work-light.jpg');
  }
}

// Overlay
#overlay {
  position: absolute;
  display: block;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  opacity: 0;
  transition-duration: 2s;
  z-index: 0 !important;
  background-image: url('/static/img/bg-about-sm-blurred.jpg');
}

// Transitions
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s
}

.fade-enter, .fade-leave-active {
  opacity: 0
}
.peek-enter-active, .peek-leave-active {
  transition: all 0.5s
}
.peek-enter, .peek-leave-active {
  transition: all 0.15s;
  opacity: 0;
  transform: translate(0,64px);
}
</style>
