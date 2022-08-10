You can add your own custom guides and product documentation to the API Portal by providing Markdown files. You can also provide a custom table-of-content to arrange your documentation for easier navigation.

*Note that providing custom content is optional; if you want to skip this, you must remove the `content` directory from the input.*

To provide custom content:

* You must add your Markdown files to the `content` directory.
* You must provide a table-of-content as the `content\toc.yml` file.
* Markdown files must be written using Github-flavored Markdown and end with the `.md` extension.
* HTML tags are not supported in the Markdown files at the moment.
* Markdown files can be arranged arbitrarily in sub-directories for your own convenience.
* APIMatic always creates the navigation hierarchy for the content using the `content\toc.yml` file.
* The Markdown files' filename and path does not matter when building the navigation hierarchy.

### Table of Content

You **must** provide a table-of-content using the `content\toc.yml` YAML file when you provide custom content. APIMatic will use this to create the navigation hierarchy for the content.

Here is what a sample `toc.yml` file looks like:

```yml
toc:
  - group: Getting Started
    items:
      - page: Introduction
        file: introduction.md
      - generate: How to Get Started
        from: getting-started
      - page: Architecture
        file: architecture_overview.md
      - page: Security
        file: security.md
      - page: Session Management
        file: session_management.md
      - page: Error Handling
        file: error_handling.md
  - group: Guides
    dir: guides
  - generate: API Endpoints
    from: endpoints
  - generate: Models
    from: models
```

* `group` creates a new navigation group with the given `name` and `items`. `items` in a `group` can be a page, generated section or another group.
* `page` adds a Markdown file as a page to the group. The `file` is a Markdown file path relative to the `toc.yml`'s path.
* `group` can include a `toc.yml` placed in another directory at `dir`. This can be used to compose `toc.yml` files together.
* `generate` adds generated content to the documentation.
	* `from: getting-started` adds a getting started page for using the REST API or the SDK.
	* `from: endpoints` adds a group containing the API reference.
	* `from: models` adds a group containing the schema reference for the API.

## Next

Checkout how to [link to pages, headings and auto-generated content from your Markdown pages](links.md#linking-in-custom-content).
