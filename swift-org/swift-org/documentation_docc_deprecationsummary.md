---
source: https://www.swift.org/documentation/docc/deprecationsummary
crawled: 2025-11-15T21:57:48Z
---

# DeprecationSummary

Directive# DeprecationSummary

A directive that specifies a custom deprecation summary message to an already deprecated symbol. Swift-DocC 5.5+ 

```
@DeprecationSummary {
    ...
}
```

## [Overview](/documentation/docc/deprecationsummary#overview)

Many in-source deprecation annotations—such as the `@available` attribute in Swift—allow you to specify a plain text deprecation message. You can use the `@DeprecationSummary` directive to override that deprecation message with one or more paragraphs of formatted documentation markup. For example,



```
@DeprecationSummary {
    This method is unsafe because it could potentially cause buffer overruns.
    Use ``getBytes(_:length:)`` or ``getBytes(_:range:)`` instead.
}

```

You can use the `@DeprecationSummary` directive top-level in articles, documentation extension files, or documentation comments.

Earlier versions

Before Swift-DocC 6.1, `@DeprecationSummary` was not supported in documentation comments.

Tip

 If you are writing a custom deprecation summary message for an API or documentation page that isn’t already deprecated, you should also deprecate it—using in-source annotations when possible or [`Available`](/documentation/docc/available) directives when in-source annotations aren’t available—so that the reader knows the version when you deprecated that API or documentation page.

## [See Also](/documentation/docc/deprecationsummary#see-also)

### [Customizing the Availability Information of a Page](/documentation/docc/deprecationsummary#Customizing-the-Availability-Information-of-a-Page)

[`@Available(_ platform: Platform, introduced: SemanticVersion, deprecated: SemanticVersion)`](/documentation/docc/available)A directive that specifies when a documentation page is available for a given platform.
- [ DeprecationSummary ](/documentation/docc/deprecationsummary#app-top)

- [ Overview ](/documentation/docc/deprecationsummary#overview)

- [ See Also ](/documentation/docc/deprecationsummary#see-also)