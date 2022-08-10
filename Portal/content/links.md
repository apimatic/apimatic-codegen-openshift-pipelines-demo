You can use Markdown links to link to auto-generated and [custom content](custom-content.md) in the portal. To maintain portability of links across SDK documentations, you must use the permalinks as defined by APIMatic.

## Shortcut: Getting Links From the API Portal

There is a shortcut to getting the permalink of a page or a heading from the portal: if you double click on a heading, the portal will log the link to the heading on the browser's developer console (usually accessed by pressing F-12 key).

![Copy link from API Portal](static/images/copy-link-from-portal.gif)

If the permalink for a heading is not logged to the developer console, linking to it is not supported.

## Linking in Custom Content

You can link from one Markdown page in your [custom content](custom-content.md) to another page or a heading inside the page.

You can use *relative links* to refer to other Markdown pages. A relative link is a link that is resolved relative to the current file:

```md
[link text](my-directory/markdown-file-name.md)
```

You can also link to headings in a page using the *heading ID*. Each heading is automatically assigned a GitHub-style heading ID which you can reference using a URL fragment:

```md
[link text](my-directory/markdown-file-name.md#heading-in-other-page)
```

You can also reference to a heading within the same page like this:

```md
[link text](#heading-in-this-page)
```

In your Markdown files, you can also use the `page:` style of links as described in [Linking in API Definitions](#linking-in-api-definitions). However, the relative-linking style is preferred in Markdown files because it also works when you are navigating your Markdown files on GitHub in case you are hosting the source code on GitHub.

To link to auto-generated content like endpoint and model pages, checkout [Linking to Auto-Generated Content](#linking-to-auto-generated-content).

## Linking in API Definitions

You can link to the Markdown pages in your [custom content](custom-content.md) and to auto-generated content using APIMatic's permalinks in the description fields of your API definition.

You must link to your Markdown pages using a `page:` link. Page links are **always** resolved relative to the `content` directory and do not have a file extension. For example, this will link to the page `content/directory-in-content/sub-directory/markdown-file-name.md`.

```md
[link text](page:directory-in-content/sub-directory/markdown-file-name)
```

You can also link to a heading inside a Markdown page like this using a GitHub-style heading ID:

```md
[link text](page:directory-in-content/sub-directory/markdown-file-name#heading-in-markdown-page)
```

Note that other style of links described in [Linking in Markdown Pages](#linking-in-custom-content) do **not** work in API definition files.

To link to auto-generated content like endpoint and model pages, checkout [Linking to Auto-Generated Content](#linking-to-auto-generated-content).

## Linking to Auto-Generated Content

 The table below describes how links to various auto-generated content are built:

| Reference Type | Format |
| --- | --- |
| Introduction | $h/__intro |
| How to install | $h/__how_to_install |
| How to build | $h/__how_to_build |
| How to use | $h/__how_to_use |
| How to test | $h/__how_to_test |
| Initializing | $h/__initializing |
| Environments | $h/__environments |
| Authorize | $h/__authorize |
| API errors | $h/__api_errors |
| Endpoint Group | $e/**{Endpoint Group Name}** |
| Endpoint | $e/**{Endpoint Group Name}**/**{Endpoint Name}** |
| Custom-type | $m/**{Model Name}** |

So, to reference an endpoint named `Get users` in a group called `User`, the link in markdown syntax would be:

```md
[put your link text here]($e/User/Get%20users)
```

Please note that

* The snippet `{ ... }` indicates that you must write the URL-encoded form of the title or the name you are referring to, without the curly brackets. To be specific, we're using `System.Net.WebUtility.UrlEncode()` method from .NET framework for URL-encoding. If you find it difficult to write this link, checkout the [shortcut to getting links from the API Portal](#shortcut-getting-links-from-the-api-portal)
* The links are case-sensitive.
