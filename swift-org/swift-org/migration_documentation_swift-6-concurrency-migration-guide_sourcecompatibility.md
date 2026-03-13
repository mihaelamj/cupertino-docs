---
source: https://www.swift.org/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/
crawled: 2025-11-15T21:59:55Z
---

# Source Compatibility

# Source Compatibility

See an overview of potential source compatibility issues.Swift 6 includes a number of evolution proposals that could potentially affect source compatibility. These are all opt-in for the Swift 5 language mode.

Note

For the previous release’s Migration Guide, see [Migrating to Swift 5](https://www.swift.org/migration-guide-swift5/).

## [Handling Future Enum Cases](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Handling-Future-Enum-Cases)

[SE-0192](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0192-non-exhaustive-enums.md): `NonfrozenEnumExhaustivity`

Lack of a required `@unknown default` has changed from a warning to an error.

## [Concise magic file names](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Concise-magic-file-names)

[SE-0274](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md): `ConciseMagicFile`

The special expression `#file` has changed to a human-readable string containing the filename and module name.

## [Forward-scan matching for trailing closures](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Forward-scan-matching-for-trailing-closures)

[SE-0286](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md): `ForwardTrailingClosures`

Could affect code involving multiple, defaulted closure parameters.

## [Incremental migration to concurrency checking](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Incremental-migration-to-concurrency-checking)

[SE-0337](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0337-support-incremental-migration-to-concurrency-checking.md): `StrictConcurrency`

Will introduce errors for any code that risks data races.

Note

This feature implicitly also enables [`IsolatedDefaultValues`](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Isolated-default-value-expressions), [`GlobalConcurrency`](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Strict-concurrency-for-global-variables), and [`RegionBasedIsolation`](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Region-based-Isolation).

## [Implicitly Opened Existentials](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Implicitly-Opened-Existentials)

[SE-0352](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md): `ImplicitOpenExistentials`

Could affect overload resolution for functions that involve both existentials and generic types.

## [Regex Literals](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Regex-Literals)

[SE-0354](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md): `BareSlashRegexLiterals`

Could impact the parsing of code that was previously using a bare slash.

## [Deprecate @UIApplicationMain and @NSApplicationMain](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Deprecate-UIApplicationMain-and-NSApplicationMain)

[SE-0383](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0383-deprecate-uiapplicationmain-and-nsapplicationmain.md): `DeprecateApplicationMain`

Will introduce an error for any code that has not migrated to using `@main`.

## [Importing Forward Declared Objective-C Interfaces and Protocols](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Importing-Forward-Declared-Objective-C-Interfaces-and-Protocols)

[SE-0384](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0384-importing-forward-declared-objc-interfaces-and-protocols.md): `ImportObjcForwardDeclarations`

Will expose previously-invisible types that could conflict with existing sources.

## [Remove Actor Isolation Inference caused by Property Wrappers](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Remove-Actor-Isolation-Inference-caused-by-Property-Wrappers)

[SE-0401](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0401-remove-property-wrapper-isolation.md): `DisableOutwardActorInference`

Could change the inferred isolation of a type and its members.

## [Isolated default value expressions](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Isolated-default-value-expressions)

[SE-0411](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0411-isolated-default-values.md): `IsolatedDefaultValues`

Will introduce errors for code that risks data races.

## [Strict concurrency for global variables](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Strict-concurrency-for-global-variables)

[SE-0412](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0412-strict-concurrency-for-global-variables.md): `GlobalConcurrency`

Will introduce errors for code that risks data races.

## [Region based Isolation](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Region-based-Isolation)

[SE-0414](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0414-region-based-isolation.md): `RegionBasedIsolation`

Increases the constraints of the `Actor.assumeIsolated` function.

## [Inferring `Sendable` for methods and key path literals](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Inferring-Sendable-for-methods-and-key-path-literals)

[SE-0418](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0418-inferring-sendable-for-methods.md): `InferSendableFromCaptures`

Could affect overload resolution for functions that differ only by sendability.

## [Dynamic actor isolation enforcement from non-strict-concurrency contexts](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Dynamic-actor-isolation-enforcement-from-non-strict-concurrency-contexts)

[SE-0423](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0423-dynamic-actor-isolation.md): `DynamicActorIsolation`

Introduces new assertions that could affect existing code if the runtime isolation does not match expectations.

## [Usability of global-actor-isolated types](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Usability-of-global-actor-isolated-types)

[SE-0434](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0434-global-actor-isolated-types-usability.md): `GlobalActorIsolatedTypesUsability`

Could affect type inference and overload resolution for functions that are globally-isolated but not `@Sendable`.

- [ Source Compatibility ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#app-top)

- [ Handling Future Enum Cases ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Handling-Future-Enum-Cases)

- [ Concise magic file names ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Concise-magic-file-names)

- [ Forward-scan matching for trailing closures ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Forward-scan-matching-for-trailing-closures)

- [ Incremental migration to concurrency checking ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Incremental-migration-to-concurrency-checking)

- [ Implicitly Opened Existentials ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Implicitly-Opened-Existentials)

- [ Regex Literals ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Regex-Literals)

- [ Deprecate @UIApplicationMain and @NSApplicationMain ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Deprecate-UIApplicationMain-and-NSApplicationMain)

- [ Importing Forward Declared Objective-C Interfaces and Protocols ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Importing-Forward-Declared-Objective-C-Interfaces-and-Protocols)

- [ Remove Actor Isolation Inference caused by Property Wrappers ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Remove-Actor-Isolation-Inference-caused-by-Property-Wrappers)

- [ Isolated default value expressions ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Isolated-default-value-expressions)

- [ Strict concurrency for global variables ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Strict-concurrency-for-global-variables)

- [ Region based Isolation ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Region-based-Isolation)

- [ Inferring `Sendable` for methods and key path literals ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Inferring-Sendable-for-methods-and-key-path-literals)

- [ Dynamic actor isolation enforcement from non-strict-concurrency contexts ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Dynamic-actor-isolation-enforcement-from-non-strict-concurrency-contexts)

- [ Usability of global-actor-isolated types ](/migration/documentation/swift-6-concurrency-migration-guide/sourcecompatibility/#Usability-of-global-actor-isolated-types)