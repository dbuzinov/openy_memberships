<template>
  <section class="app-container">
    <div class="container">
      <div class="">
        <div class="">
          <h1 class="title">
            Membership Builder
          </h1>
        </div>
      </div>
      <div class="description">  
        <div class="description-text">
          Select your preferred YMCA branch.
        </div>
        <div class="text-align-right">
          <a class="view-loactions" href="/locations">View Locations <ViewLocationIcon /></a>
        </div>
      </div>
        

      <div>
        <loading :active.sync="isLoading"></loading>
        <locations :locations="locations" />
      </div>
    </div>
    <div class="navigation" v-if="$store.state.location">
      <div class="container">
        <button class="btn btn-next" @click="$emit('go-next')">Next</button>
      </div>
    </div>
  </section>
</template>

<script>
import Locations from '@/components/Locations';
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';

import ViewLocationIcon from "@/assets/launch.svg?inline";

// import { LMap, LTileLayer, LMarker } from 'vue2-leaflet';
// import 'leaflet/dist/leaflet.css';
// import { Icon } from 'leaflet';

// delete Icon.Default.prototype._getIconUrl;
// Icon.Default.mergeOptions({
//   iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
//   iconUrl: require('leaflet/dist/images/marker-icon.png'),
//   shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
// });

export default {
  mounted() {
    this.isLoading = true;
    window.jQuery.ajax({
      url: '/jsonapi/node/branch',
      dataType: 'json'
    }).then((data)=>{
      this.isLoading = false
      this.locations = Object.keys(data.data).map(key => {
        let attributes = data.data[key].attributes;
        return {
          name: attributes.title,
          address: attributes.field_location_address.locality + ', ' + attributes.field_location_address.administrative_area,
          value: attributes.drupal_internal__nid
        }
      })
    }).catch(() => {
      this.isLoading = false
    })
  },
  methods: {
    next() {
      this.$router.push({
          path: '/memberships/summary'
      })
    }
  },
  components: {
    Locations,
    Loading,
    ViewLocationIcon
  },
  data () {
    return {
      isLoading: false,
      tab: null,
      zip: null,
      items: [
        { tab: 'zip'},
        { tab: 'manual'},
      ],
      locations: []
    }
  }
}
</script>

<style lang="scss">
  a.view-loactions {
    display: inline-flex;
    align-items: center;
    svg {
      margin: 5px;
    }
    font: Bold 14px/21px Verdana;
    letter-spacing: 0;
    color: #0060AF;
  }
</style>
<style lang="scss" scoped>
.description {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>

