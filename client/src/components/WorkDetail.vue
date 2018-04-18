<template>
  <div id="workdetail">
    <router-link :to="{ name: 'work'}" v-on:click.native="changeBg(); showNav();">Back</router-link>
    <div v-if="latestData.updated === 0">
      Loading...
    </div>
    <div v-if="latestData.updated === 1">
      shii
    </div>
    <div v-if="latestData.updated === 2">
      <h1>Response from db:</h1>
      <div class="company" v-for="company in latestData.companies" v-if="company.fields.id === $route.params.company" :key="company.id">
        <h2>{{company.fields.name}}</h2>
        <p>{{company.fields.location}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data () {
    return {
      msg: 'This is a message',
      latestData: {
        updated: 0,
        status: '',
        companies: {},
        count: 0
      }
    }
  },
  created: function () {
    this.$http.get('http://api.coty.design/companies').then(response => {
      this.latestData.updated = 2
      this.latestData.companies = response.data.records
    }, response => {
      this.latestData.updated = 1
      this.latestData.status = 'Error fetching data from database.'
      console.log(this.latestData.status)
    })
    this.$emit('hideNav')
  },
  methods: {
    changeBg: function () {
      this.$emit('pageChange')
      this.$emit('introStatus', 0)
    },
    showNav: function () {
      this.$emit('showNav')
    }
  }
}
</script>

<style scoped lang="scss">
  // Imports
  @import "../globalStyles/global";

  #workdetail {
    overflow: scroll;
    font-size: 1.6rem;
    color: $color_dark;
    padding: $padding_side;
    z-index: 4;
    * {
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }
  }
  h1 {
    margin-top: 0;
    font-size: $h2_size;
  }
  .company {
    margin-bottom: 1.6rem;
    background: rgba($color_blue-base, 0.9);
    color: white;
    padding: 1.6rem;
    border-radius: 8px;
    h2 {
      line-height: 1;
      margin: 0 0 0.8rem;
    }
    p {
      margin: 0;
      opacity: 0.8;
    }
  }
</style>
