# Syntax

The Syntax API runs on a Go server on a Docker instance. It shells out to a couple of tools to transform JavaScript code into beautiful syntax-highlighted images. An HTML interface for the API is running at [https://syntax.foundry.sh/](https://syntax.foundry.sh/)

# How It Works

First, the Go server shells out to the Atom project [Highlights](https://github.com/atom/highlights) to create HTML with syntax class names for styling. This makes this project compatible with Atom themes written in CSS. It actually uses the [Atom One](https://github.com/atom/one-dark-syntax) dark theme for styling. Some JavaScript is added to the HTML file to report back the dimensions of the rendered code.

Since there's a narrow window between when the document has finished rendering and when the browser captures the screenshot and exits, the fonts are loaded with the JavaScript [FontFace API](https://developer.mozilla.org/en-US/docs/Web/API/FontFaceSet) so that a promise to be attached to `document.fonts.ready`. This function captures the rendered dimensions at the precise time the layout settles.

```js
document.fonts.ready.then(function () {
    var w = document.querySelector('.editor').offsetWidth
    var h = document.querySelector('.editor').offsetHeight
    window.dump('Size: ' + w + 'x' + h + '\n')
})
```

This HTML file is loaded in [Firefox headless](https://developer.mozilla.org/en-US/Firefox/Headless_mode) and a screenshot is rendered. The dimensions are captured using [window.dump\(\)](https://developer.mozilla.org/en-US/docs/Web/API/Window/dump) to output from JavaScript to stdout. The image is cropped to those dimensions and uploaded to Amazon S3.

Running Firefox headless on Docker was surprisingly painless once a few key profile settings are set to enable remote debugging. To get you up and running, a Dockerfile and accompanying files are on [Github](https://github.com/nathancahill/docker-firefox-headless).

# API

Simply send JavaScript source code as the POST body to the API. The API returns `201 Created` with the image URL in the Location header.

Request

```
POST https://syntax-api.foundry.sh/
const Demo = () => {
    console.log('Hello world!')
}
```

Response

```
HTTP/1.1 201 Created
Location: https://s3-us-west-2.amazonaws.com/syntax-foundry/2f59fb5f517133bb50902f4221d5dcbd.png
```



