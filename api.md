# API

Simply pass JavaScript code in the POST body to the API. The API returns `201 Created` with the image URL in the Location header.

```
POST https://syntax-api.foundry.sh/
const Demo = () => {
    console.log('Hello world!')
}
```