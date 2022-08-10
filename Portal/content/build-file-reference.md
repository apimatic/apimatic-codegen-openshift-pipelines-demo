APIMatic expects a file that ends with `APIMATIC-BUILD.json` in the root directory inside the ZIP archive you provide as an input to the [Build endpoint]($e/Build/Generate%20using%20File). This page is a complete reference for this Build file. If you are just getting starting, please checkout the [Generating API Portal guide](overview-generating-api-portal.md).

The Build file is broken down into four high level control properties:

- [`$schema`](#schema)
- [`buildFileVersion`](#buildfileversion)
- [`generatePortal`](#generateportal)
- [`generateVersionedPortal`](#generateversionedportal)

## `$schema`

This is an optional property that helps provide [autocomplete in IDEs](build-file.md#intellisense-with-vs-code) that support it. It should be set to the value shown the example below:

```json
{
  "$schema": "https://titan.apimatic.io/api/build/schema"
  ...
}
```

## `buildFileVersion`

This property represents the APIMatic Build file format version which must be exactly `"1"`.

```json
{
  "buildFileVersion": "1"
  ...
}
```

## `generatePortal`

This property holds the configuration for the Portal Generator. The data type of this property is an **object**.

_Note: If you want to generate a Versioned Portal, use the [`generateVersionedPortal`](#generateversionedportal) key instead._

The `generatePortal` object has the following list of properties:

- [`pageTitle` _(required)_](#generateportal-pagetitle)
- [`navTitle` _(required)_](#generateportal-navtitle)
- [`logoUrl`](#generateportal-logourl)
- [`logoLink`](#generateportal-logolink)
- [`logoAltText`](#generateportal-logoalttext)
- [`faviconUrl`](#generateportal-faviconurl)
- [`headIncludes`](#generateportal-headincludes)
- [`tailIncludes`](#generateportal-tailincludes)
- [`apiSpecPath`](#generateportal-apispecpath)
- [`apiSpecs`](#generateportal-apispecs)
- [`apiMergeSettings`](#generateportal-apimergesettings)
- [`filterByRole`](#generateportal-filterbyrole)
- [`contentFolder`](#generateportal-contentfolder)
- [`languageConfig` _(required)_](#generateportal-languageconfig)
- [`portalSettings`](#generateportal-portalsettings)
- [`useHostedPortalScript`](#generateportal-usehostedportalscript)
- [`debug`](#generateportal-debug)

## `generatePortal.pageTitle`

**Required**. The title of the portal (used in the &lt;title&gt; of the page). The property's data type is **string**.

```json
"generatePortal": {
  "pageTitle": "Acme Developer Portal"
}
```

## `generatePortal.navTitle`

**Required**. The title of the portal, used in the navigation bar on top. The property's data type is **string**.

```json
"generatePortal": {
  "navTitle": "Acme API"
}
```

## `generatePortal.logoUrl`

The URL for the brand logo, shown in the navigation bar. The property's data type is **string**.

```json
"generatePortal": {
  "logoUrl": "static/images/logo.png"
}
```

## `generatePortal.logoLink`

The target URL, when the brand logo is clicked. The property's data type is **string**.

Here is what a sample looks like:

```json
"generatePortal": {
  "logoLink": "https://www.acme.org/"
}
```

## `generatePortal.logoAltText`

The alternate text for the brand logo. The property's data type is **string**.

```json
"generatePortal": {
  "logoAltText": "Acme Logo"
}
```

## `generatePortal.faviconUrl`

The property stores the favicon URL. The property's data type is **string**. This property is `optional` and defaults to `/favicon.ico`.

Here is what a sample looks like:

```json
"generatePortal": {
  "faviconUrl": "https://www.acme.org/favicon.ico"
}
```

## `generatePortal.headIncludes`

HTML that is to be injected into the &lt;head&gt; of the portal page. The property's data type is **string**.

For example, you can use this to inject additional stylesheets or set additional meta tags in the &lt;head&gt;.

```json
"generatePortal": {
  "headIncludes": "<link href=\"https:\/\/fonts.googleapis.com\/css?family=Open+Sans:300,300i,400,400i,600,600i,700,700i,800,800i&display=swap\" rel=\"stylesheet\">"
}
```

## `generatePortal.tailIncludes`

HTML to be injected at the bottom of the page. The property's data type is **string**.

A common use for this property is to inject scripts into the page. You can use this to integrate Google Analytics, Zendesk widgets or other tools with your page by injecting their scripts.

```json
"generatePortal": {
  "tailIncludes": "<script id=\"ze-snippet\" src=\"https:\/\/static.zdassets.com\/ekr\/snippet.js?key=xxxxx-xxxx-xxxx-xxxx\"><\/<script>"
}
```

## `generatePortal.apiSpecPath`

The API specification path represents the path for the directory containing the API Specification file(s). This is an `optional` property and defaults to `./spec`. The data type of this property is a **string**.

Checkout the [API Specification Guide](api-spec.md) to learn how to pass an API Specification to the Portal Generator.

```json
"generatePortal": {
  "apiSpecPath": "./spec"
}
```

## `generatePortal.apiSpecs`

The API specification(s) represents the list of directories relative to the [`apiSpecPath`](#generateportal-apispecpath) containing API specs. The data type of this property is an **array**.

Checkout the guide on [Merging Multiple API Specifications](api-spec.md#merging-multiple-api-specifications) to learn how to generate an API Portal from multiple API specs.

```json
"generatePortal": {
  "apiSpecs": [
    "spec1",
    "spec2"
  ],
}
```

## `generatePortal.apiMergeSettings`

In case multiple API specs are provided via the [`apiSpecs`](#generateportal-apispecs), the `apiMergeSettings` can be used to configure how the multiple API specs should be merged.

The data type of this property is an **object** which can take any of the properties supported in the [Merge Settings object in the Metadata file](https://docs.apimatic.io/api-transformer/configuring-transformer/#merge-settings-object).

However, note that the property names must be camel-cased when used in the Build file. For example, `DeepCompareFieldTypes` should be used as `deepCompareFieldTypes`.

## `generatePortal.filterByRole`

Filter endpoints in the API by roles at the time of Portal Generation. The property's data type is an **array**. Each item in this array is a **string** representing a role ID.

Check out the guide on [Filtering API by Roles](filtering-api-by-roles.md).

```json
"generatePortal": {
  "filterByRole": ["pub"]
}
```

## `generatePortal.contentFolder`

Root directory for the Portal Generation step inputs such as the `spec` and `content` directories. Defaults to the root of the uploaded ZIP file.

Paths such as the [`apiSpecPath`](#generateportal-apispecpath) and [`apiSpecs`](#generateportal-apispecs) are considered relative to the `contentFolder`.

Here is what a sample looks like:

```json
"generatePortal": {
  "contentFolder": "some/path/portal-source"
}
```

## `generatePortal.languageConfig`

**Required**. Use to specify which languages to generate the SDKs and documentation for. This property is of type **object** with keys being a language name (listed below) and the value being a _Language Object_.

The following languages are supported:

- csharp
- java
- php
- python
- ruby
- typescript
- http

Each Language Object except `http` has the following properties:

- [packageRepository](#generateportal-languageconfig-language_name-packagerepository)
- [packageSettings](#generateportal-languageconfig-language_name-packagesettings)

Here's an example for generating a portal with HTTP and Java documentation and Java SDK included:

```json
"generatePortal": {
  "languageConfig": {
    "http": {},
    "java": {}
  }
}
```

## `generatePortal.languageConfig.<language_name>.packageRepository`

Package repository type for the published package. The data type of this property is a **string** having the following possible values.

- NuGet _(used for csharp)_
- Maven _(used for java)_
- Packagist _(used for php)_
- PyPI _(used for python)_
- RubyGems _(used for ruby)_
- Npm _(used for typescript)_

The `packageRepository` is used together with the `packageSettings` to tell the Portal Generator where the SDK package is published. When the `packageRepository` is set, the [`packageSettings`](#generateportal-languageconfig-language_name-packagesettings) must also be provided.

## `generatePortal.languageConfig.<language_name>.packageSettings`

Package settings for the published package. The data type of this property is an **object** having the following properties based on the package repository selected

- NuGet _(used for csharp)_
  - PackageName
  - VersionNumber
- Maven _(used for java)_
  - GroupId
  - PackageName
  - VersionNumber
- Packagist _(used for php)_
  - VendorName
  - PackageName
  - VersionNumber
- PyPI _(used for python)_
  - PackageName
  - VersionNumber
- RubyGems _(used for ruby)_
  - PackageName
  - VersionNumber
- Npm _(used for typescript)_
  - PackageName
  - VersionNumber

_**Note**: All the properties GroupId, PackageName and VersionNumber have a data type **string**._

Here is what a sample for [`languageConfig`](#generateportal-languageconfig) looks like:

```json
"generatePortal": {
  "languageConfig": {
    "http": {},
    "java": {
      "packageRepository": "Maven",
      "packageSettings": {
        "GroupId": "io.apimatic",
        "PackageName": "sample-sdk-artifact-id",
        "Version": "1.0.0"
      }
    }
  }
}
```

The `packageRepository` is used together with the `packageSettings` to tell the Portal Generator where the SDK package is published so that it can generate the correct documentation using the package information. When the `packageSettings` is set, the [`packageRepository`](#generateportal-languageconfig-language_name-packagerepository) must also be provided.

## `generatePortal.portalSettings`

You can configure the Portal's user interface using the `portalSettings` settings. The data type of this property is an **object** having the following properties:

- [`container`](#generateportal-portalsettings-container)
- [`portalStyle`](#generateportal-portalsettings-portalstyle)
- [`baseURL`](#generateportal-portalsettings-baseurl)
- [`enableExport`](#generateportal-portalsettings-enableexport)
- [`enableConsoleCalls`](#generateportal-portalsettings-enableconsolecalls)
- [`languageSettings`](#generateportal-portalsettings-languagesettings)
- [`initialPlatform`](#generateportal-portalsettings-initialplatform)
- [`themeOverrides`](#generateportal-portalsettings-themeoverrides)
- [`renameHttpToRest`](#generateportal-portalsettings-renamehttptorest)

## `generatePortal.portalSettings.container`

The ID for the HTML element inside which the portal should be rendered. The property's data type is a **string** and it defaults to `"apimatic-widget"`.

```json
"generatePortal": {
  "portalSettings": {
    "container": "acme-portal-container"
  }
}
```

## `generatePortal.portalSettings.portalStyle`

This is the Portal-embedding style. The value should be _"default"_. The property's data type is a **string**.

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "portalStyle": "default"
  }
}
```

## `generatePortal.portalSettings.baseUrl`

It is the base URL for the Portal. Base URL is the URL where the Portal will be hosted. The property's data type is a **string** and it defaults to `"./"`.

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "baseUrl": "https://www.acme.org/docs"
  }
}
```

## `generatePortal.portalSettings.enableExport`

Enable API export button which allows exporting the API specifications in different formats supported by APIMatic. The property's data type is **boolean** and it defaults to `false`.

_Note: This feature is not available at the moment but can be requested from APIMatic._

```json
"generatePortal": {
  "portalSettings": {
    "enableExport": "true"
  }
}
```

## `generatePortal.portalSettings.enableConsoleCalls`

Enable the API Code Playground to make API calls directly from the Portal. This allows the user to test out the API without having leave the portal. The property's data type is **boolean** and it defaults to `true`.

```json
"generatePortal": {
  "portalSettings": {
    "enableConsoleCalls": "true"
  }
}
```

## `generatePortal.portalSettings.languageSettings`

This is used to configure additional user-interface settings for each language's documentation.

This property's data type is an **object** with the key being a _Template Name_ (valid names are listed below) and the value being an **object**.

Note that if you set the `languageSettings`, you must add all the languages you have listed in the [`generatePortal.languageConfig`](#generateportal-languageconfig).

### Template Names

The Template Names are different from Language Names used in [`generatePortal.languageConfig`](#generateportal-languageconfig) for historic reasons. The allowed language names are:

- `http_curl_v1` (corresponds to `http`)
- `cs_net_standard_lib` (corresponds to `csharp`)
- `java_eclipse_jre_lib` (corresponds `java`)
- `php_generic_lib_v2` (corresponds `php`)
- `python_generic_lib` (corresponds `python`)
- `ruby_generic_lib` (corresponds `ruby`)
- `ts_generic_lib` (corresponds `typescript`)

## `generatePortal.portalSettings.languageSettings.<template_name>.sdkDownloadLink`

External link to the SDK archive in case you are hosting the SDK archive yourself. The property's data type is a **string** and it defaults to the location of the SDK generated by APIMatic during the Portal Generation.

```json
"generatePortal": {
  "portalSettings": {
    "languageSettings": {
      "java_eclipse_jre_lib": {
        "sdkDownloadLink": "https://www.acme.org/downloads/java_sdk.zip"
      }
    }
  }
}
```

## `generatePortal.portalSettings.languageSettings.<template_name>.disableSdkDownload`

Remove the SDK download button for a language from the Portal. The property's data type is **boolean** and it defaults to `false`.

```json
"generatePortal": {
  "portalSettings": {
    "languageSettings": {
      "java_eclipse_jre_lib": {
        "disableSdkDownload": true
      }
    }
  }
}
```

## `generatePortal.portalSettings.initialPlatform`

Language that should open when the portal is first loaded. The property's data type is a **string** and it can take one of the values from the [Template Names](#template-names). It defaults to the first language in the [`generatePortal.languageConfig`](#generateportal-languageconfig).

Note that the `initialPlatform` must correspond to one of the languages enabled for generation in the [`generatePortal.languageConfig`](#generateportal-languageconfig)

```json
"generatePortal": {
  "portalSettings": {
    "initialPlatform": "http_curl_v1"
  }
}
```

## `generatePortal.portalSettings.themeOverrides`

Theme overrides for customizing the API Portal's look-and-feel. The property's data type is an **object** having the following properties.

- [`themeType`](#generateportal-portalsettings-themeoverrides-themteype)
- [`palette`](#generateportal-portalsettings-themeoverrides-palette)
- [`fontSource`](#generateportal-portalsettings-themeoverrides-fontsource)
- [`cssStyles`](#generateportal-portalsettings-themeoverrides-cssstyles)

## `generatePortal.portalSettings.themeOverrides.themeType`

Theme type. Currently, only a `"cool"` theme is supported which is also the default value. We might add more themes in the future.

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "themeType": "cool",
    }
  }
}
```

## `generatePortal.portalSettings.themeOverrides.palette`

Options for customizing the colors used in the API Portal UI. The property's data type is an **object** having the following properties.

- [`primaryColor`](#generateportal-portalsettings-themeoverrides-palette-primarycolor)
- [`linkColor`](#generateportal-portalsettings-themeoverrides-palette-linkcolor)
- [`colors`](#generateportal-portalsettings-themeoverrides-palette-colors)

## `generatePortal.portalSettings.themeOverrides.palette.primaryColor`

The primary color for the API Portal. The property's data type is **string**. Default: `"#0c7ff2"`

It is represented in the image below with `P`.

## `generatePortal.portalSettings.themeOverrides.palette.linkColor`

Link color for the API Portal. The property's data type is **string**. Default: `"#00C7D4"`

It is represented in the image below with `L`.

## `generatePortal.portalSettings.themeOverrides.palette.colors`

Options for further customizing the API Portal theme colors. The property's data type is an **object** having the following properties.

| **Property Names** | **Purpose**                                                                          | **Default (theme: cool)** |
| ------------------ | ------------------------------------------------------------------------------------ | ------------------------- |
| `C000`             | Input background-color                                                               | `"#FFFFFF"`               |
| `C100`             | **Not Applicable - Not to be used**                                                  | `"#F9FAFE"`               |
| `C200`             | Background color                                                                     | `"#F5F7F8"`               |
| `C300`             | Border color                                                                         | `"#E7E9F2"`               |
| `C400`             | Input placeholder &amp; hover border                                                 | `"#CED2E2"`               |
| `C500`             | **Not Applicable - Not to be used**                                                  | `"#B0B5C9"`               |
| `C600`             | Side Menu Tittle Color                                                               | `"#858BA5"`               |
| `C700`             | Icons, data Types in API explorer                                                    | `"#606E8A"`               |
| `C800`             | Body Text Color                                                                      | `"#3E445D"`               |
| `C900`             | Language Selector Background, Headings Text color, Dark Code Sample background color | `"#282D44"`               |
| `C1000`            | **Not Applicable - Not to be used**                                                  | `"#1D2236"`               |

All the properties under `colors` have a data type **string**.

The following image points to where [`palette`](#generateportal-portalsettings-themeoverrides-palette) properties are reflected on the DX Portal.

![Colors Options in API Portal](static/images/colors-build_file-low_res_v2.png)

Here is what a sample for [`palette`](#generateportal-portalsettings-themeoverrides-palette) looks like:

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "palette": {
        "primaryColor": "#00bcec",
        "linkColor": "#00bcec",
        "color": {
          "C000": "#FFF",
          "C200": "#F9FA6E",
          "C300": "#D345R7",
          "C600": "#0000",
          "C700": "#3R67EF",
        }
      }
    }
  }
}
```

## `generatePortal.portalSettings.themeOverrides.fontSource`

List of font sources that will be loaded with the API Portal. The property's data type is an **array**.

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "fontSource": [
        "https://fonts.googleapis.com/css?family=Open+Sans"
      ]
    }
  }
}
```

## `generatePortal.portalSettings.themeOverrides.cssStyles`

Options for customizing the typography or text-styling in the API Portal. The property's data type is an **object** having the following properties.

- [`headings`](#generateportal-portalsettings-themeoverrides-cssstyles-headings)
- [`body`](#generateportal-portalsettings-themeoverrides-cssstyles-body)
- [`code`](#generateportal-portalsettings-themeoverrides-cssstyles-code)

## `generatePortal.portalSettings.themeOverrides.cssStyles.headings`

Styles for headings. The property's data type is an **object** having the following list of properties mention in the table.

| Property Name | Type                             | Purpose                                                                                                                                                   |
| ------------- | -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fontFamily`    | String                           | Font-family for text. This is applied to all heading elements unless overridden at the heading level. <br/><br/> **Example**: `"'Open Sans', sans-serif"` |
| `h1`            | [Font Style Object](#font-style) | Styles for Level 1 headings.                                                                                                                              |
| `h2`            | [Font Style Object](#font-style) | Styles for Level 2 headings.                                                                                                                              |
| `h3`            | [Font Style Object](#font-style) | Styles for Level 3 headings.                                                                                                                              |
| `h4`            | [Font Style Object](#font-style) | Styles for Level 4 headings.                                                                                                                              |
| `h5`            | [Font Style Object](#font-style) | Styles for Level 5 headings.                                                                                                                              |
| `h6`            | [Font Style Object](#font-style) | Styles for Level 6 headings.                                                                                                                              |

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "cssStyles": {
        "heading": {
          "fontFamily": "Rubik, sans-serif",
          "h1": {
            "fontFamily": "Rubik, sans-serif",
            "fontSize": "27px",
            "fontWeight": "500",
            "fontStyle": "normal",
            "lineHeight": "1.3"
          }
        }
      }
    }
  }
}
```

## `generatePortal.portalSettings.themeOverrides.cssStyles.body`

Styles for body and other text. The property's data type is an **object** having the following list of properties mention in the table.

| **Property Name** | **Type**                         | **Purpose**                                                                                                                                          |
| ----------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fontFamily`        | String                           | Font-family for text. This is applied to all heading elements unless overridden at the heading level.<br/><br/> **Example**: "Open Sans", sans-serif |
| `text1`             | [Font Style Object](#font-style) | Styles for Text 1                                                                                                                                    |
| `text2`             | [Font Style Object](#font-style) | Styles for Text 2                                                                                                                                    |
| `text3`             | [Font Style Object](#font-style) | Styles for Text 3                                                                                                                                    |

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "cssStyles": {
        "body": {
          "fontFamily": "Rubik, sans-serif",
          "text1": {
            "fontFamily": "Rubik, sans-serif",
            "fontSize": "15px",
            "fontWeight": "400",
            "fontStyle": "normal",
            "lineHeight": "1.75"
          }
        }
      }
    }
  }
}
```

## `generatePortal.portalSettings.themeOverrides.cssStyles.code`

Styles for code snippets or code blocks. The property's data type is an **object** having the following list of properties mention in the table.

| **Property Name** | **Type**                         | **Purpose**                                                                                                                                          |
| ----------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fontFamily`        | String                           | Font-family for text. This is applied to all heading elements unless overridden at the heading level.<br/><br/> **Example**: "Open Sans", sans-serif |
| `inlineCode`        | [Font Style Object](#font-style) | Styles for inline code snippets.                                                                                                                     |
| `blockCode`         | [Font Style Object](#font-style) | Styles for block code snippets.                                                                                                                      |

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "themeOverrides": {
      "cssStyles": {
        "code": {
          "fontFamily": "'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, Courier",
          "blockCode": {
            "fontFamily": "'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, Courier",
            "fontSize": "15px",
            "fontWeight": "400",
            "fontStyle": "normal",
            "lineHeight": "1.75"
          }
        }
      }
    }
  }
}
```

## Font Style

_**Note**: This is not a property to be used in the build file. Checkout [`cssStyles`](#generateportal-portalsettings-themeoverrides-cssstyles) for options on customizing the typography._

| **Property Name** | **Type** | **Purpose**                                                                                                                                               |
| ----------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fontFamily`      | String   | Font-family for text. This is applied to all heading elements unless overridden at the heading level. <br/><br/> **Example**: `"'Open Sans', sans-serif"` |
| `fontSize`        | String   | Font-size for the text. <br/><br/> **Example**: `"16px"`                                                                                                  |
| `fontWeight`      | String   | Font-weight for the text. <br/><br/> **Example**: `"400"`                                                                                                 |
| `fontStyle`       | String   | Font-style for the text. <br/><br/>**Example**: `"normal"`                                                                                                |
| `lineHeight`      | String   | Line-height for the text. <br/><br/>**Example**: `"1.5"`                                                                                                  |

## `generatePortal.portalSettings.renameHttpToRest`

Rename "HTTP" docs to "REST" docs.The property's data type is **boolean** and the default value is `false`.

Here is what a sample looks like:

```json
"generatePortal": {
  "portalSettings": {
    "renameHttpToRest": "true"
  }
}
```

## `generatePortal.useHostedPortalScript`

Use the Portal JavaScript script hosted by APIMatic. The property's data type is **boolean**. The default and recommended value is `true`.

When `false`, the Portal script will be copied into the output. Note that using this option does have its downsides and we generally discourage its usage:

1. APIMatic makes frequent releases to fix bugs and make improvements. When you bundle the runtime resources, you will not receive those fixes automatically. You will need to generate your Portal frequently to keep it updated.
2. The generated Portal has a larger size due to the script being bundled in it. This means Portal generation might take more time.
3. In case you want to report a Portal-related bug, we will require you to generate an updated Portal first. This is the only way for us to ensure that the bug is not already fixed in a release for the Portal runtime that might have been missed by you.

```json
"generatePortal": {
  "useHostedPortalScript": "true"
}
```

## `generatePortal.debug`

Setting to output information that can help you debug issues during Portal generation. The data type of this property is an **object** having the following properties:

- [`publishReport`](#generateportal-debug-publishreport)
- [`dumpImportedApi`](#generateportal-debug-dumpimportedapi)
- [`reportPath`](#generateportal-debug-reportpath)
- [`importedApiDumpPath`](#generateportal-debug-importedapidumppath)

Here is what a sample for [`debug`](#generateportal-debug) looks like:

```json
"generatePortal": {
  "debug": {
    "publishReport": "true",
    "dumpImportedApi": "true"
  }
}
```

## `generatePortal.debug.publishReport`

Enabling the property will create an HTML report in the output containing validation messages from the API specification(s) that were imported and any merge-related issues from the imported APIs. The property's data type is **boolean** and it defaults to `false`.

## `generatePortal.debug.dumpImportedApi`

Marking this property as `true` will put the imported API specification(s) in the APIMatic Format in the output.

This is useful when used with API Merging feature as you can debug what went wrong in merging. The property's data type is **boolean** and it defaults to `false`.

## `generatePortal.debug.reportPath`

This refers to the path where the report mentioned in [`publishReport`](#generateportal-debug-publishreport) will be published. The property's data type is **string** and the default value is `"./apimatic-debug/apimatic-report.html"`.

## `generatePortal.debug.importedApiDumpPath`

This refers to the path where the API dump mentioned [`dumpImportedApi`](#generateportal-debug-dumpimportedapi) will be published. The property's data type is **string** and the default value is `"./apimatic-debug/imported-api-dump.json"`.

## `generateVersionedPortal`

This property represents the configuration for the Versioned Portal Generator. This can not be used together with [`generatePortal`](#generateportal); only one of them must be used in a Build file at a time.

Check out our detailed guide on generating a [Versioned API Portal](versioning.md).

The data type of this property is an **object** having the following properties.

- [versionsPath](#generateversionedportal-versionspath)
- [useHostedPortalScript](#generateversionedportal-usehostedportalscript)
- [versions](#generateversionedportal-versions)

## `generateVersionedPortal.versionsPath`

The root directory for the Versioned Portal Generation steps inputs. Defaults to `"./versioned_docs"`. The property's data type is **string**.

Here is what a sample looks like:

```json
"generateVersionedPortal": {
  "versionsPath": "./versioned_docs"
}
```

## `generateVersionedPortal.useHostedPortalScript`

Use the portal script hosted by APIMatic. The property's data type is **boolean**. The default and recommended value is `true`.

When `false`, the portal script will be copied into the output. This is similar to [`useHostedPortalScript`](#generateportal-usehostedportalscript).

```json
"generateVersionedPortal": {
  "useHostedPortalScript": "true"
}
```

## `generateVersionedPortal.versions`

The version information of the Portal. The data type of this property is an **array of objects** having the following properties:

- [`label`](#generateversionedportal-versions-label)
- [`version`](#generateversionedportal-versions-version)

Here is what a sample for [`versions`](#generateversionedportal-versions) looks like:

```json
{
"generateVersionedPortal": {
  "versions": [
    {
      "label": "2.0 (current)",
      "version": "2.0"
    },
    {
      "label": "1.0",
      "version": "1.0"
    }
  ]
}
```

## `generateVersionedPortal.versions[*].label`

Label for the version. This will be shown in the version selector dropdown in the portal. The property's data type is **string**.

## `generateVersionedPortal.versions[*].version`

The version number for the portal. This string must be a valid URL slug. The property's data type is **string**.
