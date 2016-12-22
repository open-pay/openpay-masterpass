# openpay-masterpass
JavaScript plugin which help integrate Masterpass in your web app.

## Add js dependence
Use this Masterpass js for TEST in SANDBOX environment

```html
<script type="text/javascript" src="https://sandbox.static.masterpass.com/dyn/js/switch/integration/MasterPass.client.js"></script>
```
Use this Masterpass js for PRODUTION environment

```html
<script type="text/javascript" src="https://static.masterpass.com/dyn/js/switch/integration/MasterPass.client.js"></script>
```
Add Openpay js

```html
<script type='text/javascript' src="https://openpay.s3.amazonaws.com/openpay.v1.min.js"></script>
```
Add openpay-masterpass.js

```html
<script type="text/javascript" src="${contextPath}/js/openpay/masterpass/openpay-masterpass.js"></script>
```
## Set Openpay api keys

```js
  OpenPay.setId('mYourMerchantId');
  OpenPay.setApiKey('pk_yourPublicKey');
  OpenPay.setSandboxMode(true);// set false to go production
```
## Configure event for Masterpass button

```html
<div class="MasterPassBtnExample">
			<a href="#"> <img src="https://www.mastercard.com/mc_us/wallet/img/en/US/mcpp_wllt_btn_chk_180x042px.png"></a>
</div>
```
```js
OpenpayMasterpass.configureButton(".MasterPassBtnExample a", {
      originUrl : 'https://myWebApp/demo/masterpass/main',
      callbackUrl : 'https://myWebApp/demo/masterpass/main',
      enableShippingAddress : true,
      successCallback : yourSuccessConfigureButtonCallback, // See an example below...
      failureCallback : yourFailureCallback,
      cancelCallback : yourCancelCallback,
      shoppingCart : {//Shoping cart
        currency: "MXN",  //Currency
        subtotal: "123.00",  //Total Amount
        items: [ //Optionally an arry of items
          {
            description : "Pen 3d", //Some description
            quantity : 2,
            value : "100.00", //Amount
            imageUrl : "https://cdn.pixabay.com/photo/2012/04/13/17/57/pen-33077_960_720.png" //Some image url
          },
          {
            description : "Pencil del futuro",
            quantity : 1,
            value : "23.00",
            imageUrl : "http://i50.twenga.com/suministros/lapiz/lapices-staedtler-noris-hb-tp_3981652568322654320f.jpg"
          }
        ]
      }
});
```

##  Your successConfigurationButtonCallback
```js
var successConfigureButtonCallback = function(response) {
  OpenpayMasterpass.getCheckoutData(
    response, //this is required
    yourSuccessCheckoutCallback, //You must implement this. See an example below...
    yourFailureCheckoutCallback  //And this too!
  );
};
```

##  Your successConfigurationButtonCallback
```js
  var successGetCheckoutCallback = function(response) {
  var data = response.data;
  console.log('DATA:');
  console.log(data);
  $("#tokenCard").val(data.id);//this is your openpay Token card to use in the charge request for Openpay

  var card = data.card;
  console.log(data.card);
  $("#holderName").text(card.holder_name);
  $("#cardNumber").text(card.card_number);
  $("#expirationDate").text(card.expiration_month + '/' +card.expiration_year);

  var customer = data.customer;
  $("#customerName").text(customer.name);
  $("#customerEmail").text(customer.email);
  $("#customerLastName").text(customer.last_name);
  $("#customerPhone").text(customer.phone_number);

  var sa = data.shipping_address; 
  if (sa) {
    $("#postalCode").text(sa.postal_code);
    $("#country").text(sa.country_code);
    $("#saPhone").text(sa.phone_number);
    $("#recipient").text(sa.recipient);
    $("#line1").text(sa.line1);
    $("#line2").text(sa.line2);
    $("#line3").text(sa.line3);
    $("#state").text(sa.state);
    $("#city").text(sa.city);
  }
};
```
