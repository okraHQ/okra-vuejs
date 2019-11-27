# okra-vuejs
> This is an vue module that abstracts the complexity of using okra with vue.

## USAGE

### 1. Install the module
```sh
npm install --save vue-okra
```

### 2. Import the module
In your `app.vue` or any module where the component would be used like so:

```vue
<script>
import OkraButton from "./components/OkraButton.vue";

export default {
  name: "app",
  components: {
    OkraButton
  },
  methods: {
    success: function() {
      window.console.log("okra success");
    },
    close: function() {
      window.console.log("okra closed");
    }
  }
};
</script>
```

### 3. Implement in your project

* **VueOkraComponent**: Renders a button which when clicked loads okra Inline
  ```html
    <OkraButton
      text="Open Okra"
      token="5d8a35224d8113507c7521ac"
      env="sandbox"
      clientName="Chikala"
      okra_key="c81f3e05-7a5c-5727-8d33-1113a3c7a5e4"
      callback_url="www.google.com"
      :close="close"
      :success="success"
      :user="{fullname: 'USER_FULL_NAME', email: 'USER_EMAIL', bvn: 'USER_BVN'}"
      :products="['auth', 'transactions', 'balance', 'income', 'identity']"
    />
  ```

And then in your `component.ts`
```vue
  <script>
import OkraButton from "./components/OkraButton.vue";

export default {
  name: "app",
  components: {
    OkraButton
  },
  methods: {
    success: function() {
      window.console.log("okra success");
    },
    close: function() {
      window.console.log("okra closed");
    }
  }
};
</script>
```


## OkraOptions

|Name                   | Type           | Required            | Default Value       | Description         |
|-----------------------|----------------|---------------------|---------------------|---------------------|
|  `callback_url `      | `string`       | true                |  undefined          | This is your webhook to which okra sends the clients data to.
|  `key `               | `String`       | true                |  undefined          | Your public key from Okra.
|  `token `             | `String`       | true                |  undefined          | Your client token on the [setting page of the okra dashboard](https://dashboard.okra.ng/settings/keys)
|  `products`           | `ArrayList<Enums.Product>`| true     |  undefined          | The Okra products you want to use with the widget. list of products include: 'auth', 'transactions', 'balance', 'income', 'identity'
|  `env`                | `String`       | true                |  https uses `production` and http uses `sandbox-production` |  `production` or `production-sandbox`
|  `clientName`         | `String`       | false               |  `This client`      | Name of the customer using the widget on the application
|  `user`               | `object`       | false               |  undefined          | This contains some information of the user using the okra widget {fullname: 'USER_FULL_NAME',email: 'USER_EMAIL', bvn: 'USER_BVN'}



> For more information checkout [okra's documentation](https://docs.okra.ng)

## Contributing

Please feel free to fork this package and contribute by submitting a pull request to enhance the functionalities.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
