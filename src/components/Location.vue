<template>
  <div class="card location" @click="gotoPlaceDetail(location.id)">
    <img v-if="location.thumbnail" :src="location.thumbnail" class="card-img">
    <div class="card-block location-info">
      <h3 class="card-title">
        {{ location.locationName }}
      </h3>
      <star-rating
        :rating="location.rating"
        :read-only="true"
        :star-size="15"
        :increment="0.5">
      </star-rating>
    </div>
  </div>
</template>

<script>
import { getPlaceImage } from '../utils/ApiUtil'
import StarRating from 'vue-star-rating'
import Icon from 'vue-icon'

export default {
  components: {
    StarRating,
    Icon
  },
  props: ['location'],
  data () {
    return {
      isAdd: false,
      isDelete: false
    }
  },
  methods: {
    gotoPlaceDetail (place_id) {
      this.$router.push('/location-detail/' + place_id)
    },
    getImage: function (photoReference, maxWidth, maxHeight) {
      return getPlaceImage(photoReference, maxWidth, maxHeight)
    },
    addLocation: function (id) {
      this.$emit('addId', this.id)
    },
    deleteLocation: function (id) {
      this.$emit('deleteId', this.id)
    }
  }
}
</script>

<style>
.location-photo {
  float: left;
  padding: 5px;
  position: relative;
}
.add {
  float: left;
  display: none;
}
.addActive {
  display: block;
}
.delete {
  position: inherit;
  display: none;
}
.deleteActive {
  display: block;
}
.location-name {}
.star-rating {}

.location:hover {
  background-color: #f4f6f6;
}

.card { position: relative; margin-bottom: 10px; border: 0; box-shadow: 0 0 .5em rgba(0,0,0,.3); min-height: 190px; background-color: rgba(0,0,0,.5) }
.rating-text { display: none; }
.card-img { width: 100%; height: auto; display: block }
.location-info { position: absolute; left: 0; bottom: 0; right: 0; padding: 10px; }
</style>
