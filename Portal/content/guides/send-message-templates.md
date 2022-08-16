# Send Message Templates


WhatsApp message templates are specific message formats that businesses use to send out notifications or customer care messages to people that have opted in to notifications. Messages can include appointment reminders, shipping information, issue resolution or payment updates.

Before sending a message template, you need to create one. See [Create Message Templates for Your WhatsApp Business Account](https://www.facebook.com/business/help/2055875911147364?id=2129163877102343) for more information. If your account is not verified yet, you can use one of our pre-approved templates.

Currently, you can send the following template types:

* Text-based message templates
* Media-based message templates
* Interactive message templates

All API calls mentioned in this guide must be authenticated with an access token. Developers can authenticate their API calls with the access token generated in `App Dashboard > WhatsApp > Getting Started`. Business Solution Providers (BSPs) must authenticate themselves with an access token with the `whatsapp_business_messaging` permission.

## Text-Based Message Templates

To send a text-based message template, make a `POST` call to [/messages]($e/Messages/SendMessage) and attach a message object with type=template. Then, add a template object.

Sample request:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "template",
  "template": {
    "name": "TEMPLATE_NAME",
    "language": {
      "code": "LANGUAGE_AND_LOCALE_CODE"
    },
    "components": [
      {
        "type": "body",
        "parameters": [
          {
            "type": "text",
            "text": "text-string"
          },
          {
            "type": "currency",
            "currency": {
              "fallback_value": "VALUE",
              "code": "USD",
              "amount_1000": NUMBER
            }
          },
          {
            "type": "date_time",
            "date_time": {
              "fallback_value": "DATE"
            }
          }
        ]
      }
    ]
  }
}'
```

A successful response includes an object with an identifier prefixed with wamid. Use the ID listed after wamid to track your message status.
```
{
  "messaging_product": "whatsapp",
  "contacts": [{
      "input": "PHONE_NUMBER",
      "wa_id": "WHATSAPP_ID",
    }]
  "messages": [{
      "id": "wamid.ID",
    }]
}
```

## Media-Based Message Templates

To send a media-based message template, make a `POST` call to  [/messages]($e/Messages/SendMessage) and attach a message object with type=template. Then, add a template object.

Sample request:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "template",
  "template": {
    "name": "TEMPLATE_NAME",
    "language": {
      "code": "LANGUAGE_AND_LOCALE_CODE"
    },
    "components": [
      {
        "type": "header",
        "parameters": [
          {
            "type": "image",
            "image": {
              "link": "https://URL"
            }
          }
        ]
      },
      {
        "type": "body",
        "parameters": [
          {
            "type": "text",
            "text": "TEXT-STRING"
          },
          {
            "type": "currency",
            "currency": {
              "fallback_value": "VALUE",
              "code": "USD",
              "amount_1000": NUMBER
            }
          },
          {
            "type": "date_time",
            "date_time": {
              "fallback_value": "MONTH DAY, YEAR"
            }
          }
        ]
      }
    ]
  }
}'
```

A successful response includes an object with an identifier prefixed with wamid. Use the ID listed after wamid to track your message status.

```
{
  "messaging_product": "whatsapp",
  "contacts": [{
      "input": "PHONE_NUMBER",
      "wa_id": "WHATSAPP_ID",
    }]
  "messages": [{
      "id": "wamid.ID",
    }]
}

```

## Interactive Message Templates

Interactive message templates expand the content you can send recipients beyond the standard message template and media messages template types to include interactive buttons using the components object. There are two types of predefined buttons:

Call-to-Action — Allows your customer to call a phone number and visit a website.
Quick Reply — Allows your customer to return a simple text message.
These buttons can be attached to text messages or media messages. Once your interactive message templates have been created and approved, you can use them in notification messages as well as customer service/care messages.

To send an interactive message template, make a `POST` call to  [/messages]($e/Messages/SendMessage) and attach a message object with type=template. Then, add a template object with your chosen button.

Sample request:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "template",
  "template": {
    "name": "TEMPLATE_NAME",
    "language": {
      "code": "LANGUAGE_AND_LOCALE_CODE"
    },
    "components": [
      {
        "type": "header",
        "parameters": [
          {
            "type": "image",
            "image": {
              "link": "http(s)://URL"
            }
          }
        ]
      },
      {
        "type": "body",
        "parameters": [
          {
            "type": "text",
            "text": "TEXT_STRING"
          },
          {
            "type": "currency",
            "currency": {
              "fallback_value": "VALUE",
              "code": "USD",
              "amount_1000": NUMBER
            }
          },
          {
            "type": "date_time",
            "date_time": {
              "fallback_value": "MONTH DAY, YEAR"
            }
          }
        ]
      },
      {
        "type": "button",
        "sub_type": "quick_reply",
        "index": "0",
        "parameters": [
          {
            "type": "payload",
            "payload": "PAYLOAD"
          }
        ]
      },
      {
        "type": "button",
        "sub_type": "quick_reply",
        "index": "1",
        "parameters": [
          {
            "type": "payload",
            "payload": "PAYLOAD"
          }
        ]
      }
    ]
  }
}'
```

A successful response includes an object with an identifier prefixed with wamid. Use the ID listed after wamid to track your message status.
```
{
  "messaging_product": "whatsapp",
  "contacts": [{
      "input": "PHONE_NUMBER",
      "wa_id": "WHATSAPP_ID",
    }]
  "messages": [{
      "id": "wamid.ID",
    }]
}
```