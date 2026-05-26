# Submissions API

Read your form submissions programmatically — including metadata like user IP address. This is a **read-only** REST API, separate from the form submission endpoint.

{% hint style="info" %}
The Submissions API is a **PRO feature**. Create and manage API keys from your [dashboard](https://app.web3forms.com/account/api-keys).
{% endhint %}

## Base URL

```
https://api.web3forms.com/v1
```

## Authentication

Every request must include a Bearer token in the `Authorization` header. Your API key looks like `w3f_live_…`.

```bash
curl https://api.web3forms.com/v1/forms \
  -H "Authorization: Bearer w3f_live_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

{% hint style="warning" %}
Your API key is shown **only once** when you create it. Store it somewhere safe. If you lose it, revoke it and create a new one.
{% endhint %}

### Managing keys

Go to **Dashboard → Account → API Keys**:

* **Create** a key — give it a label (e.g. "Production backend"). The full key is shown once.
* **Revoke** a key — takes effect immediately; any request using it returns `401`.
* You can hold up to **10 active keys** at a time.

A key is scoped to your account and can read submissions for any form you own.

## Rate limits

Requests are throttled at **20 requests/second** (burst 50) per account. Exceeding this returns `429` with a `Retry-After` header (in seconds).

***

## List forms

<mark style="color:green;">`GET`</mark> `https://api.web3forms.com/v1/forms`

Returns all forms you own.

#### Response

```json
{
  "data": [
    {
      "form_id": "0a1b2c3d-....",
      "form_name": "Contact Form",
      "created_at": "2026-01-15T10:30:00.000Z",
      "total_count": 142
    }
  ]
}
```

***

## List submissions

<mark style="color:green;">`GET`</mark> `https://api.web3forms.com/v1/submissions`

Returns submissions for a form, newest first.

#### Query Parameters

| Name                                       | Type    | Description                                  |
| ------------------------------------------ | ------- | -------------------------------------------- |
| form\_id<mark style="color:red;">\*</mark> | string  | The form to fetch submissions for.           |
| limit                                      | integer | Page size. Default `50`, min `1`, max `100`. |
| cursor                                     | string  | Pagination cursor from a previous response.  |

#### Example

```bash
curl "https://api.web3forms.com/v1/submissions?form_id=FORM_ID&limit=50" \
  -H "Authorization: Bearer w3f_live_…"
```

#### Response

```json
{
  "data": [
    {
      "id": "sub_a1b2c3d4e5f6",
      "form_id": "0a1b2c3d-....",
      "submitted_at": "2026-05-26T18:21:09.123Z",
      "ip_address": "203.0.113.42",
      "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) ...",
      "site_url": "https://example.com/contact",
      "fields": {
        "name": "Jane Doe",
        "email": "jane@example.com",
        "message": "Hello!"
      },
      "attachments": []
    }
  ],
  "has_more": true,
  "next_cursor": "eyJQSyI6Ii4uLiJ9"
}
```

### Pagination

When `has_more` is `true`, pass `next_cursor` back as the `cursor` parameter to fetch the next page. Repeat until `has_more` is `false`.

```bash
curl "https://api.web3forms.com/v1/submissions?form_id=FORM_ID&cursor=NEXT_CURSOR" \
  -H "Authorization: Bearer w3f_live_…"
```

***

## Get a submission

<mark style="color:green;">`GET`</mark> `https://api.web3forms.com/v1/submissions/{id}`

Returns a single submission by its `id`.

#### Example

```bash
curl "https://api.web3forms.com/v1/submissions/sub_a1b2c3d4e5f6" \
  -H "Authorization: Bearer w3f_live_…"
```

#### Response

```json
{
  "data": {
    "id": "sub_a1b2c3d4e5f6",
    "form_id": "0a1b2c3d-....",
    "submitted_at": "2026-05-26T18:21:09.123Z",
    "ip_address": "203.0.113.42",
    "user_agent": "Mozilla/5.0 ...",
    "site_url": "https://example.com/contact",
    "fields": { "name": "Jane Doe", "email": "jane@example.com" },
    "attachments": []
  }
}
```

***

## Errors

Errors return a non-2xx status and a JSON body:

```json
{
  "error": {
    "code": "unauthorized",
    "message": "Invalid API key"
  }
}
```

| Status | Code                  | Meaning                                           |
| ------ | --------------------- | ------------------------------------------------- |
| `400`  | `bad_request`         | Missing or invalid parameter (e.g. no `form_id`)  |
| `401`  | `unauthorized`        | Missing, malformed, or revoked key                |
| `403`  | `forbidden`           | Key not authorized for that form                  |
| `404`  | `not_found`           | Form or submission doesn't exist (or isn't yours) |
| `429`  | `rate_limit_exceeded` | Too many requests — retry after the header value  |
| `500`  | `server_error`        | Something went wrong on our end                   |
