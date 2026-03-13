---
source: https://www.swift.org/documentation/docc/redirected
crawled: 2025-11-15T21:50:14Z
---

# Redirected

Directive# Redirected

A directive that specifies a previous URL for the page where the directive appears. Swift-DocC 5.5+ 

```
@Redirected(from: URL)
```

## [ Parameters ](/documentation/docc/redirected#parameters)

`from`The URL that redirects to the page associated with the directive. **(required)**

## [Overview](/documentation/docc/redirected#overview)

If the page has moved more than once you can add multiple `Redirected` directives, each specifying one previous URL. For example:



```
@Redirected(from: "old/path/to/this/page")
@Redirected(from: "another/old/path/to/this/page")

```

Note

Starting with version 6.0, the `Redirected` directive is supported both top-level and as a member of a [`Metadata`](/documentation/docc/metadata) directive. In earlier versions, the `Redirected` directive is only supported as a top-level directive.

### [Setting up Redirects](/documentation/docc/redirected#Setting-up-Redirects)

If you host your documentation on a web server, you can set a HTTP “301 Moved Permanently” redirect for each `Redirected` value to avoid breaking existing links to your content.

To find each page’s Redirected values, pass the `--emit-digest` flag to DocC. This flag configures DocC to write additional metadata files to the output directory. One of these files, `linkable-entities.json`, contains summarized information about all pages and on-page landmarks that you can link to from outside the DocC documentation. Each of these “entities” has a `"path"`—which represents the current relative path of that page—and an optional list of `"redirects"`—which represent all the `Redirected` values for page as they were authored in the markup. You can write either relative redirect values or absolute redirect values in the markup depending on what information you need when setting up HTTP “301 Moved Permanently” redirects on your web server.

## [See Also](/documentation/docc/redirected#see-also)

### [Configuring Documentation Behavior](/documentation/docc/redirected#Configuring-Documentation-Behavior)

[`@Options(scope: Scope)`](/documentation/docc/options)A directive to adjust Swift-DocC’s default behaviors when rendering a page.[`@Metadata`](/documentation/docc/metadata)Use metadata directives to instruct DocC how to build certain documentation files.[`@TechnologyRoot`](/documentation/docc/technologyroot)Configures an article to become a top-level page.
- [ Redirected ](/documentation/docc/redirected#app-top)

- [ Parameters ](/documentation/docc/redirected#parameters)

- [ Overview ](/documentation/docc/redirected#overview)

- [ Setting up Redirects ](/documentation/docc/redirected#Setting-up-Redirects)

- [ See Also ](/documentation/docc/redirected#see-also)