<template>
  <div class="view trip-detail">
    <div class="map-content">
      <template v-if="loaded">
        <GMap :direction="direction"></GMap>
      </template>
    </div>
    <div class="map-content-space">
    </div>
    <div class="trip-content">
      <h3 class="page-header">{{ trip.name }}</h3>
      <template v-if="loaded">
        <LocationList :locations="locations"></LocationList>
      </template>
      <b-button
        class="btn-lg btn-block btn-info"
        @click="gotoMyTrip()">
        Go to My Trip
      </b-button>
    </div>
  </div>
</template>

<script>
import {getUser, db} from '@/utils/FirebaseApp'
import Map from '@/components/Map'
import MyButton from '@/components/MyButton'
import LocationList from '@/components/LocationList'

export default {
  name: 'TripDetail',
  props: ['trip_id'],
  components: {
    "GMap": Map,
    MyButton,
    LocationList
  },
  data: () => ({
    trip: {},
    direction: [],
    locations: {},
    loaded: false
  }),
  methods: {
    gotoMyTrip () {
      this.$router.push('/my-trip')
    }
  },
  firebase () {
    return {
      trip: {
        source: db.ref('trips/' + getUser().uid + '/' + this.trip_id),
        asObject: true,
        readyCallback: () => {
          this.loaded = true
          this.direction = Object.keys(this.trip.locations)
            .map(key => this.trip.locations[key])
          this.locations = Object.keys(this.trip.locations)
            .map(key => {
              const item = this.trip.locations[key]
              const place_id = key
              return {
                id: place_id,
                locationName: String(item.name),
                rating: item.rating,
                order: item.order,
                type: 'simple',
                photoReference: item.thumbnail,
                thumbnail: item.thumbnail,
                maxWidth: 100,
                maxHeight: 100
              }
            })
        }
      }
    }
  }
}
</script>

<style>
.trip-detail {
  height: 100%;
}
.trip-content {
  padding-bottom: 10px;
}
.map-content {
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 80%;
  z-index: 2
}
.map-content-space {
  position: relative;
  width: auto;
  height: 80%;
  z-index: 1
}
</style>
