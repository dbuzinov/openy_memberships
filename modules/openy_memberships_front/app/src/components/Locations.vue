<template>
  <div class="locations">
    
      <location @input="updateLocation" :value="loc" :val="location.value" :key="key" v-for="(location, key) in locations" :name="location.name" :address="location.address" class="location-wrapper" />
  </div>
</template>
<script>
import Location from './Location';
import { mapState } from 'vuex';

export default {
  props: ['locations'],
  computed: mapState({
    loc: state => state.location,
  }),
  components: {
    Location
  },
  data() {
    return {
      value: null
    }
  },
  methods: {
    updateLocation(e) {
      this.value = e;
      this.$store.commit('setLocation', this.value);
    }
  }
}
</script>

<style lang="scss" scoped>
.locations {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  margin-left: -10px;
  margin-right: -10px;
}
.location-wrapper {
  min-width: 33.3%;
  padding: 10px;
  @media screen and (max-width: 650px) {
    min-width: 50%;
  }
  @media screen and (max-width: 400px) {
    min-width: 100%;
  }
}
</style>