<template>
  <div class="container is-info is-outlined">
    <form id="fileupload">
      <a class="button is-primary is-outlined">
        Add Files
        <input class="input is-transparent" type="file" placeholder="File" name="files[]" />
      </a>
    </form>
  </div>
</template>

<script>
import Vue from 'vue'
import 'blueimp-file-upload'
import VueBlu from 'vue-blu'
import 'vue-blu/dist/css/vue-blu.min.css'
Vue.use(VueBlu)

export default {
  name: 'fileupload',
  data () {
    return {
        msg: ''
    }
  }
}
</script>
