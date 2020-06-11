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
import OkraButton from "vue-okra/src/components/OkraButton";

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
    env="production"
    client-name="Chikala"
    okra-key="c81f3e05-7a5c-5727-8d33-1113a3c7a5e4"
    callback-url="www.google.com"
    color="#b0c77f"
    limit="24"
    :corporate="false"
    connect-message=""
    redirect-url=""
    logo=""
    widget-success=""
    currency=""
    exp=""
    success-title=""
    success-message=""
    :guarantors="{'status': false, 'message': 'Please add your gaurantor','number': '2'}"
    :filter= "{'industry_type':'all','banks':['ecobank-nigeria','fidelity-bank','first-bank-of-nigeria','first-city-monument-bank','guaranty-trust-bank','heritage-bank','polaris-bank','stanbic-ibtc-bank','standard-chartered-bank','sterling-bank','union-bank-of-nigeria','united-bank-for-africa','wema-bank','unity-bank','alat','access-bank']}"
    :close="close"
    :success="success"
    :options="{fullname: 'USER_FULL_NAME', email: 'USER_EMAIL', bvn: 'USER_BVN'}"
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
|  `callback-url `      | `string`       | true                |  undefined          | This is your webhook to which okra sends the clients data to.
|  `key `               | `String`       | true                |  undefined          | Your public key from Okra.
|  `token `             | `String`       | true                |  undefined          | Your client token on the [setting page of the okra dashboard](https://dashboard.okra.ng/settings/keys)
|  `products`           | `Array`| true     |  `['Auth']`          | The Okra products you want to use with the widget. list of products include: 'auth', 'transactions', 'balance', 'income', 'identity'
|  `env`                | `String`       | true                |  https uses `production` and http uses `sandbox-production` |  `production` or `production-sandbox`
|  `client-name`         | `String`       | false               |  `This client`      | Name of the customer using the widget on the application
|  `logo `              | `String(URL)`  | false               | Okra's Logo         | 
|  `color`              | `HEX   `       | false               | #3AB795             | Theme on the widget 
|  `limit`              | `Number`       | false               | 24                  | Statement length
|  `filter`             | `Object`       | false               |                     | Filter for widget
|  `corporate`          | `Boolen`       | false               | `false`             | Corporate or Individual account
|  `connect-message`    | `String`       | false               |                     | Instruction to connnect account
|  `guarantors          | `Object`       | false               |                     | 
|  `widget-success`     | `String`       | false               |                     | Widget Success Message
|  `widget-failed`      | `String`       | false               |                     | Widget Failed Message
|  `currency`           | `String`       | false               | NGN                 | Wallet to bill
|  `exp`                | `Date`         | false               | Won't expire        | Expirary date of widget
|  `options`            | `Object`       | false               |                     | You can pass a object custom values eg id
|  `success`            | `Function`     | false               |                     | Action to perform after widget is successful
|  `close`              | `Function`     | false               |                     | Action to perform if widget is closed


## Data Dictionary

### Auth
Field                        | Required | Description
-----------------------------|----------|---------------------------------------------------------------
**id**<br>`ObjectID`         | **Yes**  | Unique Auth ID (Unique Okra Identifier)
**validated**<br>`Boolean`   | **Yes**  | Customer authentication status
**bank**<br>`ObjectID`       | **Yes**  | Unique Bank ID (Unique Okra Identifier)
**customer**<br>`ObjectID`   | **Yes**  | Unique Customer ID (Unique Okra Identifier)
**record**<br>`ObjectID`     | **Yes**  | Unique Record ID (Unique Okra Identifier)
**owner**<br>`ObjectID`      | **Yes**  | Unique Company ID (Unique Okra Identifier) (Your Client Token)
**created_at**<br>`Object`   | **Yes**  | Date Auth was fetched
**last_updated**<br>`Object` | **Yes**  | Last Date Auth was fetched

### Balance
Field                              | Required | Description
-----------------------------------|----------|--------------------------------------------------------------------------------------
**id**<br>`ObjectID`               | **Yes**  | Unique Balance ID (Unique Okra Identifier)
**available_balance**<br>`Integer` | **Yes**  | Amount of available funds in account
**ledger_balance**<br>`Integer`    | **Yes**  | Closing balance of account
**currency**<br>`String`           | **Yes**  | The currency of the account
**connected**<br>`Boolean`         | **Yes**  | Customer connection status (Did they choose to connect this account to you)
**env**<br>`String`                | **Yes**  | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**bank**<br>`ObjectID`             | **Yes**  | Unique Bank ID (Unique Okra Identifier)
**accounts**<br>`ObjectID`         | **Yes**  | Unique Account ID (Unique Okra Identifier)
**customer**<br>`ObjectID`         | **Yes**  | Unique Customer ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID`  | **Yes**  | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object`         | **Yes**  | Date Balance was fetched
**last_updated**<br>`Object`       | **Yes**  | Last Date Balance was fetched

### Identity
Field                                 | Required | Description
--------------------------------------|----------|--------------------------------------------------------------------------------------
**id**<br>`ObjectID`                  | **Yes**  | Unique Identifier ID (Unique Okra Identifier)
**firstname**<br>`String`             | **Yes**  | Customer First Name
**middlename**<br>`String`            | **Yes**  | Customer Middle Name
**lastname**<br>`String`              | **Yes**  | Customer Last Name
**next_of_kins**<br>`Identity Object` | **Yes**  | Customer Next of Kins
**dob**<br>`Date`                     | **Yes**  | Customer Date of Birth
**verified**<br>`String`              | **Yes**  | BVN Validation status
**score**<br>`String`                 | **Yes**  | Unique Okra Score
**dti**<br>`String`                   | **Yes**  | Customer Debt to Income Score
**fullname**<br>`String`              | **Yes**  | Customer Fullname
**company_name**<br>`String`          | **Yes    | Company Name if Corporate Identity
**nin**<br>`String`                   | **Yes**  | Customer NIN Number
**national_id**<br>`String`           | **Yes**  | Customer National ID Number
**drivers_lisence**<br>`String`       | **Yes**  | Customer Driver's License Number
**nimc**<br>`String`                  | **Yes**  | Customer National Identity Management Commission (NIMC) Number
**voters_id**<br>`String`             | **Yes**  | Customer Voter's ID Number
**rc_number**<br>`String`             | **Yes**  | Company's Registered Company Number if Corporate Identity
**phone**<br>`Array of String`        | **Yes**  | Customer Phone Number
**last_login**<br>`String`            | **Yes**  | Customer Last Login via Okra
**email**<br>`Array of String`        | **Yes**  | Customer Email address
**address**<br>`Array of String`      | **Yes**  | Customer
**mothers_maiden**<br>`String`        | **Yes**  | Customer Mother's Maiden Name
**photo_ids**<br>`Array of Object`    | **Yes**  | Customer's photo ID
**env**<br>`String`                   | **Yes**  | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**bank**<br>`ObjectID`                | **Yes**  | Unique Bank ID (Unique Okra Identifier)
**accounts**<br>`ObjectID`            | **Yes**  | Unique Account ID (Unique Okra Identifier)
**customer**<br>`ObjectID`            | **Yes**  | Unique Customer ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID`     | **Yes**  | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object`            | **Yes**  | Date Balance was fetched
**last_updated**<br>`Object`          | **Yes**  | Last Date Balance was fetched

### Transaction
Field                                    | Required | Description
-----------------------------------------|----------|--------------------------------------------------------------------------------------
**id**<br>`ObjectID`                     | **Yes**  | Unique Transaction ID (Unique Okra Identifier)
**debit**<br>`Integer`                   | **No**   | Amount deducted from account
**credit**<br>`Integer`                  | **No**   | Amount credited to account
**trans_date**<br>`Date`                 | **Yes**  | Date transaction occurred
**cleared_date**<br>`Date`               | **Yes**  | Date transaction cleared at bank
**unformatted_trans_date**<br>`String`   | **Yes**  | Date transaction occurred (from bank)
**unformatted_cleared_date**<br>`String` | **Yes**  | Date transaction cleared (from bank)
**branch**<br>`String`                   | **No**   | Branch transactions occurred
**ref**<br>`String`                      | **No**   | Bank reference ID (from bank)
**env**<br>`String`                      | **Yes**  | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**code**<br>`String`                     | **No**   | Bank Code (from bank)
**benefactor**<br>`ObjectID`             | **No**   | Customer ID of sender (within Okra)
**code**<br>`String`                     | **No**   | Bank Code (from bank)
**notes**<br>`Object`                    | **Yes**  | Breakdown of Narrative from bank
**bank**<br>`ObjectID`                   | **Yes**  | Unique Bank ID (Unique Okra Identifier)
**account**<br>`ObjectID`                | **Yes**  | Unique Account ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID`        | **Yes**  | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object`               | **Yes**  | Date transactions was fetched
**last_updated**<br>`Object`             | **Yes**  | Last Date transactions was fetched

### Notes Data Dictionary
Field                                | Required | Description
-------------------------------------|----------|------------------------------------------------------------------------------------------
**desc**<br>`String`                 | **Yes**  | Narrative / Description of transaction (combination of bank and user entered information)
**topics**<br>`Array of String`      | **Yes**  | Topics within the desc
**places**<br>`Array of String`      | **Yes**  | Places mentioned within the desc
**people**<br>`Array of String`      | **Yes**  | People mentioned within the desc
**actions**<br>`Array of String`     | **Yes**  | Actions mentioned within the desc
**subject**<br>`Array of String`     | **Yes**  | Subject of the desc
**preposition**<br>`Array of String` | **Yes**  | Prepositions within desc to understand intent


> For more information checkout [okra's documentation](https://docs.okra.ng)

## Contributing

Please feel free to fork this package and contribute by submitting a pull request to enhance the functionalities.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
