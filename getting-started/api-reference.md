# API Reference

## Form Submission using Access Key

<mark style="color:green;">`POST`</mark> `https://api.web3forms.com/submit`

This endpoint allows you to submit form submissions. The following are the reserved names that will trigger form functions. You may use any other names in your forms as you need and it will be forwarded to your email as-is.&#x20;

{% hint style="info" %}
It is recommend that you use the API client/browser side, not server side.&#x20;

Server side usage requires paid plan + server IP whitelisting.&#x20;
{% endhint %}

#### Request Body

| Name                                          | Type    | Description                                                                                                                                                         |
| --------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| access\_key<mark style="color:red;">\*</mark> | string  | This is where you should pass your Access Key. It is required to send the form to your email address.                                                               |
| email                                         | string  | User Email. This will be used to set reply to address. So its easy to follow-up.                                                                                    |
| subject                                       | string  | Email Subject. It can be submitted by user or prefilled using `hidden` attribute.                                                                                   |
| ccemail                                       | string  | **PRO feature:** Add your co-workers to your email notification.                                                                                                    |
| replyto                                       | string  | Reply to Email. If you don't want to use `email` as replyto, you can assign a custom email here.                                                                    |
| redirect                                      | string  | <p>URL. You can use a custom URL to redirect to a page when the form submits successfully.<br><code>NOTE: Only recommended when using without JavaScript</code></p> |
| botcheck                                      | boolean | Hidden. To prevent Spam Submissions. Make sure its hidden by adding `display:none;`                                                                                 |
| attachment                                    | file    | **PRO feature:** Send a file.                                                                                                                                       |
| webhook                                       | string  | **PRO feature:** Hidden. Trigger a webhook when form is submitted.                                                                                                  |

## Form submission using Form ID

<mark style="color:green;">`POST`</mark> `https://api.web3forms.com/submit/YOUR_FORM_ID`

{% hint style="info" %}
Form ID and Access key is same UUID. Not a different one.&#x20;
{% endhint %}

Use Access key as form ID in the POST URL directly if your usage did not allow you to add a hidden `access_key` field inside `<form>`&#x20;

No hidden access\_key field is need to add if using this method.&#x20;

#### Request Body

`[any]: [any]`

Any fields are accepted.&#x20;

## Response Codes

#### `200` Success

```javascript
{
   "success":true,
   "body":{
      "data":{
        [USER SUBMITTED DATA]
      },
      "message":"Email sent successfully!"
   }
}
```

#### `303` Success Redirect

Redirects to `https://api.web3forms.com/submit/success` endpoint by default.&#x20;

or custom redirect page set by user.&#x20;

#### `400` Client Error

```javascript
{
   "success":false,
   "body":{
      "data":{
        [USER SUBMITTED DATA]
      },
      "message":"Error Description"
   }
}
```

#### `429` Ratelimit

```javascript
 {
   "success": false,
   "message": "Too may requests. Please try later!"
   }
}
```

#### `500` Server Error

```javascript
{
  "statusCode": 500,
  "error": "Something went wrong on server."
}
```



