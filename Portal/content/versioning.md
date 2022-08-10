You can version your documentation and API specification and generate a unified API Portal for it. The generated API Portal will have a version switcher that will allow the user to switch between the different versions.

![Versioning in API Portal](static/images/version-selector-portal.png)

**Recommendation:** *Before you start using this feature, consider that it can make it difficult for contributors to contribute to the documentation. Most of the time, you don't need versioning as it will increase build time and introduce complexity to your codebase. Versioning is best suited for API portals with high-traffic and rapid changes to documentation between versions. If your documentation or API specification rarely changes, it is not recommended to add versioning to your documentation.*

## Directory Structure

The directory structure for versioning extends the existing structure described in the [Portal Generation Overview](overview-generating-api-portal.md).

```shell
APIMATIC-BUILD.json         # contains version details
versioned_docs
  version-2.0               # documentation for version 2.0
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
  version-1.0               # documentation for version 1.0
    APIMATIC-BUILD.json
    spec\
      openapi.json
    ## Rest of the documentation files for version 1.0 here...
```

For each version of the documentation, there is a directory inside the `versioned_docs` directory. Each version's directory follows the same structure as described in the [overview](overview-generating-api-portal.md) and **must** contain its own `APIMATIC-BUILD.json` file.

There is also an additional APIMatic Build file in the root directory that contains the versioning information.

## Describing Versions

The `APIMATIC-BUILD.json` file in the root directory defines what versions are available. This is what the build file should look like:

```json
{
  "$schema": "https://titan.apimatic.io/api/build/schema",
  "buildFileVersion": "1",
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
}
```

Note that:

* The `APIMATIC-BUILD.json` file must describe the versioning information using the `generateVersionedPortal` object.
* Each version **must** have a `label` and a `version` number.
* The `version` number is used to determine the path for the documentation. For example, the documentation for version `2.0` should be in the `versioned_docs/version-2.0` directory.
* The label is a user-friendly readable name that is shown to the user through the version selector in the API Portal.

## Migrating Existing Documentation to Versioning

If your documentation is not yet versioned, you can start versioning by reorganizing your documentation according to the aforementioned directory structure. To migrate non-versioned docs to a versioned structure:

* Move your current documentation and the existing `APIMATIC-BUILD.json` file into the `versioned_docs/version-1.0` directory (assuming it was version 1.0).
* Add a new `APIMATIC-BUILD.json` in the root directory and add the versioning info using the `generateVersionedPortal` object (as explained in the previous heading).
* Ensure that any new versions are added to the `generateVersionedPortal` and have a corresponding directory in the `versioned_docs` directory.
* It is good practice to set up your webserver to redirect users from the old pages to the new pages in the documentation site.

## Recommended Practices

### Version Documentation Only When Needed

Should you make new documentation for version 1.0.1 after 1.0.0? You probably shouldn't. Versions 1.0.0 and 1.0.1 will probably have only minor changes and creating a whole new version of documentation for this will only result in creating unnecessary duplicated files.

### Redirect Users to Latest Documentation

It will help users if they are automatically directed to the latest version of your documentation when they visit your documentation site via a link on your marketing page, email or other website pages. To do this, you should set up your webserver to redirect users from `https://docs.company.com/v/latest` via a [302 redirect](https://httpstatuses.com/302) to the latest version of your documentation.

If you create a new version of your documentation, just update the redirect rule to point to the new version of the documentation.
