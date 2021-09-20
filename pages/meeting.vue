<template>
  <div class="meeting">
    <div class="container-zoom">
      <client-only>
        <ZoomFrame :nickname="nickname" :meeting-id="meetingId" />
      </client-only>
    </div>
  </div>
</template>

<script>
import ZoomFrame from '@/components/ZoomFrame.vue'
export default {
  name: 'Meeting',
  components: {
    ZoomFrame
  },
  data () {
    return {
      nickname: '',
      meetingId: ''
    }
  },
  created () {
    this.nickname = this.$route.query.nickname
    this.meetingId = this.$route.query.meetingId
  }
}
</script>

<style>
.container-zoom {
  width: 70%;
  height: 100%;
}
</style>
