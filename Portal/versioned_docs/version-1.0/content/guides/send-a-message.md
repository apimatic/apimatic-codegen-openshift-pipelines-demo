# Send a Message

To send a message, make a POST call to the [/messages]($e/Messages/SendMessage) endpoint and attach a message object with the type of message you would like to send.

Currently, you can send the following message types:

* [Text messages]($m/Audio) 
* Media  Messages including [Video]($m/Video), [Audio ]($m/Audio) and [Image]($m/Image)
* [Location messages]($m/Location)
* [Contact messages]($m/Contact)
* [Interactive messages]($m/Interactive)


All API calls mentioned in this guide must be authenticated with an access token. Developers can authenticate their API calls with the access token generated in `App Dashboard > WhatsApp > Getting Started`.
Business Solution Providers (BSPs) must authenticate themselves with an access token with the `whatsapp_business_messaging` permission.



## Text Message

To send a text message, make a `POST` call to the  [/messages]($e/Messages/SendMessage) endpoint and attach a message object with `type=text`. Then, add a text object.

```
curl -X POST \
  --url 'https://graph.facebook.com/v13.0/Phone-Number-ID6/messages' \
  -H 'Authorization: Bearer AccessToken'\
  -H 'Accept: application/json'\
  -H 'Content-Type: application/json' \
  --data-raw '{
  "messaging_product": "whatsapp",
  "to": "{{Recipient-Phone-Number}}",
  "type": "template",
  "text": {
    "body": "Hello world",
    "preview_url": false
  }
}'
```

## Media Messages

To send a media message, make a `POST `call to  [/messages]($e/Messages/SendMessage) and attach a message object with `type=image`, document, audio, image, video, or sticker. Then, add a corresponding media object.

Sample request using image with link:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "image",
  "image": {
    "id" : "MEDIA_OBJECT_ID"
  }
}'

```

Sample request using media ID:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "image",
  "image": {
    "id" : "MEDIA_OBJECT_ID"
  }
}'

````

A successful response includes an object with an identifier prefixed with wamid. If you are using a link to send the media, please check the callback events delivered to your Webhook server whether the media has been downloaded successfully.

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
````

## Location Messages
To send location messages, make a `POST` call to  [/messages]($e/Messages/SendMessage) and attach a message object with type=location. Then, add a location object.

Sample request:
```
curl -X  POST \
 'https://graph.facebook.com/V13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "to": "PHONE_NUMBER",
  "type": "location",
  "location": {
    "longitude": LONG_NUMBER,
    "latitude": LAT_NUMBER,
    "name": LOCATION_NAME,
    "address": LOCATION_ADDRESS
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

## Contact Messages

To send contact messages, make a `POST` call to  [/messages]($e/Messages/SendMessage) and attach a message object with type=contact. Then, add a contacts object.

Sample request:
```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "to": "PHONE_NUMBER",
  "type": "contacts",
  "contacts": [{
      "addresses": [{
          "street": "STREET",
          "city": "CITY",
          "state": "STATE",
          "zip": "ZIP",
          "country": "COUNTRY",
          "country_code": "COUNTRY_CODE",
          "type": "HOME"
        },
        {
          "street": "STREET",
          "city": "CITY",
          "state": "STATE",
          "zip": "ZIP",
          "country": "COUNTRY",
          "country_code": "COUNTRY_CODE",
          "type": "WORK"
        }],
      "birthday": "YEAR_MONTH_DAY",
      "emails": [{
          "email": "EMAIL",
          "type": "WORK"
        },
        {
          "email": "EMAIL",
          "type": "HOME"
        }],
      "name": {
        "formatted_name": "NAME",
        "first_name": "FIRST_NAME",
        "last_name": "LAST_NAME",
        "middle_name": "MIDDLE_NAME",
        "suffix": "SUFFIX",
        "prefix": "PREFIX"
      },
      "org": {
        "company": "COMPANY",
        "department": "DEPARTMENT",
        "title": "TITLE"
      },
      "phones": [{
          "phone": "PHONE_NUMBER",
          "type": "HOME"
        },
        {
          "phone": "PHONE_NUMBER",
          "type": "WORK",
          "wa_id": "PHONE_OR_WA_ID"
        }],
      "urls": [{
          "url": "URL",
          "type": "WORK"
        },
        {
          "url": "URL",
          "type": "HOME"
        }]
    }]
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

## Interactive Messages

Interactive messages include List Messages and Reply Buttons. To send interactive messages, make a `POST` call to  [/messages]($e/Messages/SendMessage) and attach a message object with type=interactive. Then, add an interactive object.

List Messages sample request:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "interactive",
  "interactive": {
    "type": "list",
    "header": {
      "type": "text",
      "text": "HEADER_TEXT"
    },
    "body": {
      "text": "BODY_TEXT"
    },
    "footer": {
      "text": "FOOTER_TEXT"
    },
    "action": {
      "button": "BUTTON_TEXT",
      "sections": [
        {
          "title": "SECTION_1_TITLE",
          "rows": [
            {
              "id": "SECTION_1_ROW_1_ID",
              "title": "SECTION_1_ROW_1_TITLE",
              "description": "SECTION_1_ROW_1_DESCRIPTION"
            },
            {
              "id": "SECTION_1_ROW_2_ID",
              "title": "SECTION_1_ROW_2_TITLE",
              "description": "SECTION_1_ROW_2_DESCRIPTION"
            }
          ]
        },
        {
          "title": "SECTION_2_TITLE",
          "rows": [
            {
              "id": "SECTION_2_ROW_1_ID",
              "title": "SECTION_2_ROW_1_TITLE",
              "description": "SECTION_2_ROW_1_DESCRIPTION"
            },
            {
              "id": "SECTION_2_ROW_2_ID",
              "title": "SECTION_2_ROW_2_TITLE",
              "description": "SECTION_2_ROW_2_DESCRIPTION"
            }
          ]
        }
      ]
    }
  }
}'

```

## Reply Button sample request:

```
curl -X  POST \
 'https://graph.facebook.com/v13.0/FROM_PHONE_NUMBER_ID/messages' \
 -H 'Authorization: Bearer ACCESS_TOKEN' \
 -d '{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "PHONE_NUMBER",
  "type": "interactive",
  "interactive": {
    "type": "button",
    "body": {
      "text": "BUTTON_TEXT"
    },
    "action": {
      "buttons": [
        {
          "type": "reply",
          "reply": {
            "id": "UNIQUE_BUTTON_ID_1",
            "title": "BUTTON_TITLE_1"
          }
        },
        {
          "type": "reply",
          "reply": {
            "id": "UNIQUE_BUTTON_ID_2",
            "title": "BUTTON_TITLE_2"
          }
        }
      ]
    }
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
