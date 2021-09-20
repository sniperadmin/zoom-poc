<template>
  <div class="iframe-container">
    <meta charset="utf-8">
    <link type="text/css" rel="stylesheet" href="https://dmogdx0jrul3u.cloudfront.net/1.3.7/css/bootstrap.css">
    <link
      type="text/css"
      rel="stylesheet"
      href="https://dmogdx0jrul3u.cloudfront.net/1.3.7/css/react-select.css"
    >

    <meta name="format-detection" content="telephone=no">
  </div>
</template>
<script>

export default {
  name: 'ZoomFrame',
  props: {
    nickname: String,
    meetingId: String
  },
  data () {
    return {
      src: '',
      meetConfig: {},
      signature: {}
    }
  },
  created () {
    if (process.client) {
      const { ZoomMtg } = process.client ? require('zoomus-jssdk') : {}

      console.log('checkSystemRequirements')
      console.log(JSON.stringify(ZoomMtg.checkSystemRequirements()))
      // CDN version default
      ZoomMtg.setZoomJSLib('https://dmogdx0jrul3u.cloudfront.net/1.3.7/lib', '/av')
      ZoomMtg.preLoadWasm()
      ZoomMtg.prepareJssdk()
      const API_KEY = process.env.API_KEY
      const API_SECRET = process.env.API_SECRET

      // Meeting config object
      this.meetConfig = {
        apiKey: API_KEY,
        apiSecret: API_SECRET,
        meetingNumber: this.meetingId,
        userName: this.nickname,
        passWord: '',
        leaveUrl: 'https://zoom.us',
        role: 0
      }
      // Generate Signature function
      this.signature = ZoomMtg.generateSignature({
        meetingNumber: this.meetConfig.meetingNumber,
        apiKey: this.meetConfig.apiKey,
        apiSecret: this.meetConfig.apiSecret,
        role: this.meetConfig.role,
        success (res) {
          // eslint-disable-next-line
          console.log("success signature: " + res.result);
        }
      })
      // join function
      ZoomMtg.init({
        leaveUrl: 'http://www.zoom.us',
        isSupportAV: true,
        success: () => {
          ZoomMtg.join({
            meetingNumber: this.meetConfig.meetingNumber,
            userName: this.meetConfig.userName,
            signature: this.signature,
            apiKey: this.meetConfig.apiKey,
            userEmail: 'email@gmail.com',
            passWord: this.meetConfig.passWord,
            success (res) {
              // eslint-disable-next-line
              console.log("join meeting success");
            },
            error (res) {
              // eslint-disable-next-line
              console.log(res);
            }
          })
        },
        error (res) {
          // eslint-disable-next-line
          console.log(res);
        }
      })
    }
  },
  mounted () {}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
