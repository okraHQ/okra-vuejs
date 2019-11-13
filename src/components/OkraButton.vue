<template>
  <div>
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
      link.setAttribute('href', 'https://cdn.okra.ng/okra.min.css');

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
      this.scriptLoaded &&
        this.scriptLoaded.then(() => {
          var client = new window.okra.create({
            env: this.env,
            clientName: this.clientName,
            key: this.okra_key,
            callback_url: this.callback_url,
            user: {
              fullname:  this.user.fullname,
              email: this.user.email,
              bvn: this.user.bvn
            },
            products: ["auth", "transactions", "balance", "income", "identity"],
            onSuccess: () => {
              this.success();
            },
            onClose: () => {
              this.close();
            }
          });
          client.open();
        });
    }
  }
};
</script>