APIMatic allows you to generate a complete *API Documentation Portal* from your API specification. You can also add product documentation and guides to the API Portal by providing custom content in the form of Markdown files. The generated API Portal contains:

1. Comprehensive API reference
2. SDK-specific documentation
3. Product documentation or guides provided by you

## Generating an API Documentation Portal

* You must ZIP the input files (described below) for the portal generation into a ZIP archive. Only ZIP archives are supported right now.
* Call the [Build endpoint]($e/Build/Generate%20using%20File) with the ZIP file as the input. Checkout the endpoint documentation on the exact API call needed.
* The generation make take a few minutes; the generated portal is returned as a ZIP file.

## Inputs for Portal Generation

To generate an API Portal using APIMatic, you need to provide the following as the input to APIMatic:

1. [Build file](build-file.md): Describes the settings for portal generation.
2. [API specification(s)](api-spec.md): File(s) describing your API(s) in a standard format.
3. [Custom Content](custom-content.md) _(optional)_: Custom content for the portal in the form of Markdown files.
4. Static files _(optional)_: Static files such as images that might be referenced in your content.

## Directory Structure

APIMatic expects these files to be provided as a ZIP archive, with the following directory structure:

```txt
APIMATIC-BUILD.json
spec\
  openapi.json
content\
  guide1.md
  guide2.md
  toc.yml
  custom-section/
    section-overview.md
    toc.yml
static\
  images\
    img1.png
    img2.jpg
  another-static-file.txt
```

* APIMatic expects a file with name ending with `APIMATIC-BUILD.json` file in the root directory.
* The `spec` folder must contain one API specification. This can be in any of the API specification formats support by APIMatic. If you have multiple API specifications, checkout the [API specification](api-spec.md#merging-multiple-api-specifications) topic for additional handling required for this.
* `content` and `static` directories can be skipped if you do not have custom content or static files.

## Next

Checkout how to [create a build file](build-file.md) for portal generation.