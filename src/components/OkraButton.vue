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
    color: {
      type: String,
      required: false
    },
    limit: {
      type: String,
      required: false
    },
    corporate: {
      type: Boolean,
      required: false
    },
    options: {
      type: Object,
      default: function() {
        return {};
      }
    },
    connectMessage: {
      type: String,
      required: false
    },
    redirect_url: {
      type: String,
      required: false
    },
    logo: {
      type: String,
      required: false
    },
    widget_success: {
      type: String,
      required: false
    },
    currency: {
      type: String,
      required: false
    },
    exp: {
      type: String,
      required: false
    },
    success_title: {
      type: String,
      required: false
    },
    success_message: {
      type: String,
      required: false
    },
    guarantors: {
      type: Object,
      default: function() {
        return {
          status: false,
          message: "Please add your gaurantor",
          number: "2"
        };
      }
    },
    filter: {
      type: Object,
      default: function() {
        return {};
      }
    },
    clientName: {
      type: String,
      required: true
    },
    token: {
      type: String,
      required: true
    },
    products: {
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
      const link = window.document.createElement("link");
      window.document.head.appendChild(link);
      link.setAttribute("rel", "stylesheet");
      link.setAttribute("type", "text/css");
      link.setAttribute("href", "https://cdn.okra.ng/okra.css");

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
        token: this.token,
        callback_url: this.callback_url,
        source: "vue",
        color: this.color,
        limit: this.limit,
        options: this.options,
        connectMessage: this.connectMessage,
        redirect_url: this.redirect_url,
        logo: this.logo,
        widget_success: this.widget_success,
        currency: this.currency,
        exp: this.exp,
        success_title: this.success_title,
        success_message: this.success_message,
        guarantors: this.guarantors,
        filter: this.filter,
        corporate: this.corporate,
        products: this.products,
        onSuccess: json => {
          this.success(json);
        },
        onClose: json => {
          this.close(json);
        }
      };
      this.scriptLoaded &&
        this.scriptLoaded.then(() => {
          var client = new window.okra.create();
          client.open(options);
        });
    }
  }
};
</script>