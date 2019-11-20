<template>
  <div id="okra-enabled">
    <button @click="openOkraWidget">{{ text }}</button>
  </div>
</template>
<script>
export default {
  props: {
    text: {
      type: String,
      default: "Open Okra"
    },
    env: {
      type: String,
      required: true
    },
    clientName: {
      type: String,
      required: true
    },
    token : {
      type: String,
      required: true
    },
    products : {
      type: Array,
      required: true
    },
    okra_key: {
      type: String,
      required: true
    },
    callback_url: {
      type: String,
      required: true
    },
    close: {
      type: Function,
      required: true,
      default: function() {}
    },
    success: {
      type: Function,
      required: true,
      default: function() {}
    },
    user: {
      type: Object,
      default: function() {
        return {};
      }
    }
  },
  data() {
    return {
      count: 0
    };
  },
  created() {
    this.scriptLoaded = new Promise(resolve => {
      this.loadScript(() => {
        resolve();
      });
    });
  },
  methods: {
    loadScript(callback) {

      const link = window.document.createElement('link');
      window.document.head.appendChild(link);
      link.setAttribute('rel', 'stylesheet');
      link.setAttribute('type', 'text/css');
      link.setAttribute('href', 'https://cdn.okra.ng/okra.css');

      const script = document.createElement("script");
      script.src = "https://cdn.okra.ng/okra.min.js";
      document.getElementsByTagName("head")[0].appendChild(script);
      if (script.readyState) {
        // IE
        script.onreadystatechange = () => {
          if (
            script.readyState === "loaded" ||
            script.readyState === "complete"
          ) {
            script.onreadystatechange = null;
            callback();
          }
        };
      } else {
        script.onload = () => {
          callback();
        };
      }
    },

    openOkraWidget() {
      const options = {
            env: this.env,
            clientName: this.clientName,
            key: this.okra_key,
            token : this.token,
            callback_url: this.callback_url, 
            user: {
              fullname:  this.user.fullname,
              email: this.user.email,
              bvn: this.user.bvn
            },
            products: this.products,
            onSuccess: () => {
              this.success();
            },
            onClose: () => {
              this.close();
            }
          }
      this.scriptLoaded &&
        this.scriptLoaded.then(() => {
          var client = new window.okra.create();
          client.open(options);
        });
    }
  }
};
</script>