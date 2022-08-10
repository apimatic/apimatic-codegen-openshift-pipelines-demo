APIMatic expects one or more API specifications in the `spec` directory inside the ZIP archive you provide as an input to the [Build endpoint]($e/Build/Generate%20using%20File).

* The API specification must be in one of the [formats supported by APIMatic](https://www.apimatic.io/transformer/#supported-formats).
* The API specification format is detected automatically by APIMatic.
* The API specification can be provided as multiple files if that is supported by the API specification format you are using.
* Multiple API specifications are supported but *this requires additional handling (see next section)*.

The API specification must be valid for the APIMatic Code Generator to work.

## Merging Multiple API Specifications

APIMatic can import multiple API specifications and merge them together to create a unified API Portal and SDK for each language.

To merge multiple API specifications together, you must:

1. Put each API specification (whether it is a single file or composed of multiple files) in a separate directory in the `spec` directory. Checkout this sample directory structure with two API specifications:

```txt
APIMATIC-BUILD.json
spec\
  spec1\
    openapi.json
  spec2\
    openapi.json
  ...
content\
static\
```

2. Update your `APIMATIC-BUILD.json` file with the list of specification directories in the `spec` directory. The *order of this list is important to the merging strategy APIMatic applies*. Checkout this sample Build file, specially the `apiSpecs` directive:

```json
{
  "$schema": "https://titan.apimatic.io/api/build/schema",
  "buildFileVersion": "1",
  "generatePortal": {
    "apiSpecs": [
      "spec1",
      "spec2"
    ],
    "languageConfig": {
      "http": {}
    }
  }
}
```

### Merging Strategy

By default, APIMatic merges APIs using the *Take Left* merge strategy. When two API specifications are being merged, APIMatic will try to keep elements such as endpoints and schemas from both the APIs. If there is a conflict, the value from the first API will take precedence. This is why the order of the API specifications in the `apiSpecs` list matters.

## Next

Checkout how to [add custom content to your portal](custom-content.md).

Checkout how to [link to custom and auto-generated content from your Markdown pages](links.md#linking-in-api-definitions).
