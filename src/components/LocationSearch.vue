<template lang="html">
  <div class="form-group components location-search">
    <input type="text"
      ref="gmap-autocomplete"
      class="form-text form-control form-control-lg"
      :placeholder="placeholder">
  </div>
</template>

<script>
import * as VueGoogleMaps from 'vue2-google-maps'

export default {
  name: 'LocationSearch',
  props: ['placeholder'],
  data: () => ({
    location: {}
  }),
  mounted () {
    const autocomplete = new google.maps.places.Autocomplete(
      this.$refs['gmap-autocomplete'],
      {types: ['geocode']}
    )

    autocomplete.setComponentRestrictions({'country': ['th']});

    autocomplete.addListener('place_changed', () => {
      const thumbSize = {maxWidth: 200, maxHeight: 200}
      const place = autocomplete.getPlace()

      this.$emit('update:location', {
        id: place.place_id,
        name: place.name,
        order: 1,
        rating: place.rating ? place.rating : 0,
        thumbnail: place.photos ? place.photos.pop().getUrl(thumbSize) : '',
        position: {
          lat: place.geometry.location.lat(),
          lng: place.geometry.location.lng()
        }
      })
    })
  }
}
</script>

<style lang="css">
</style>
