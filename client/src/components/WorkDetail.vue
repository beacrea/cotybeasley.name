<template>
  <div id="workdetail">
    <div v-if="latestData.updated === 0">
      Loading...
    </div>
    <div v-if="latestData.updated === 1">
      shii
    </div>
    <div v-if="latestData.updated === 2">
      <h1>Response from db:</h1>
      <div class="company" v-for="company in latestData.companies" :key="company.id">
        {{company.fields.name}}
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
        companies: {}
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
  },
  methods: {
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
  }
</style>
