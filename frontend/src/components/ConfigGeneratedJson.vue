<template>
  <v-container>
    <div>
      <v-textarea
        auto-grow
        readonly
        label="WebADE Application Configuration"
        v-model="appConfigAsString"
        class="jsonText"
      ></v-textarea>
    </div>
    <p class="text-right">
      <v-tooltip bottom>
        <template v-slot:activator="{ on }">
          <v-btn text icon color="primary" v-on="on" @click="downloadFile">
            <v-icon>cloud_download</v-icon>
          </v-btn>
        </template>
        <span>Download application configuration</span>
      </v-tooltip>

      <v-tooltip bottom>
        <template v-slot:activator="{ on }">
          <v-btn
            text
            icon
            color="primary"
            v-clipboard:copy="appConfigAsString"
            v-clipboard:success="clipboardSuccessHandler"
            v-clipboard:error="clipboardErrorHandler"
            v-on="on"
          >
            <v-icon>file_copy</v-icon>
          </v-btn>
        </template>
        <span>Copy application configuration to clipboard</span>
      </v-tooltip>
    </p>

    <v-snackbar v-model="snackbar.on" right top :timeout="6000" :color="snackbar.color">
      {{snackbar.text}}
      <v-btn color="white" text @click="snackbar.on = false">
        <v-icon>close</v-icon>
      </v-btn>
    </v-snackbar>
  </v-container>
</template>

<script>
import Vue from 'vue';
import { mapGetters } from 'vuex';
import VueClipboard from 'vue-clipboard2';

VueClipboard.config.autoSetContainer = true;
Vue.use(VueClipboard);

export default {
  data() {
    return {
      snackbar: {
        on: false,
        text: 'test',
        color: 'info'
      }
    };
  },
  computed: {
    ...mapGetters('configForm', ['appConfigAsString'])
  },
  methods: {
    clipboardSuccessHandler() {
      this.snackbar.on = true;
      this.snackbar.text = 'Application Configuration copied to clipboard';
      this.snackbar.color = 'info';
    },
    clipboardErrorHandler() {
      this.snackbar.on = true;
      this.snackbar.text = 'Error attempting to copy to clipboard';
      this.snackbar.color = 'error';
    },
    downloadFile() {
      var element = document.createElement('a');
      element.setAttribute(
        'href',
        'data:text/plain;charset=utf-8,' +
          encodeURIComponent(this.appConfigAsString)
      );
      element.setAttribute('download', 'applicationConfig.json');
      element.style.display = 'none';
      element.classList.add('hiddenDownloadTextElement');
      document.body.appendChild(element);
      element.click();
      document.body.removeChild(element);
    }
  }
};
</script>

<style>
.jsonBtn {
  cursor: pointer;
  margin-left: 20px;
}
</style>
