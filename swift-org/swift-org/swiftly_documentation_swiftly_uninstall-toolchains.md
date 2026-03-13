---
source: https://www.swift.org/swiftly/documentation/swiftly/uninstall-toolchains/
crawled: 2025-11-15T21:58:45Z
---

# Uninstall Swift Toolchains | Documentation

- [ Swiftly ](/swiftly/documentation/swiftlydocs)

- [ Uninstall Swift Toolchains ](/swiftly/documentation/swiftly/uninstall-toolchains/)

- [ Uninstall Swift Toolchains ](/swiftly/documentation/swiftly/uninstall-toolchains/)

Article# Uninstall Swift Toolchains

Uninstall Swift toolchains.## [Overview](/swiftly/documentation/swiftly/uninstall-toolchains/#overview)

After installing several toolchains the list of the available toolchains to use becomes too large. Each toolchain also occupies substantial storage space. It’s good to be able to cleanup toolchains when they aren’t needed anymore. This guide will cover how to uninstall your toolchains assuming that you have installed swiftly and used it to install them.

If you have a released version that you want to uninstall then give the exact three digit version (major, minor and patch):



```
$ swiftly uninstall 5.6.1

```

When you’re done working with every patch of a minor swift release you can remove them all by omiting the patch version.



```
$ swiftly uninstall 5.6

```

Snapshots can be removed individually using the version (or main) and the date.



```
$ swiftly uninstall main-snapshot-2022-08-30
$ swiftly uninstall 5.7-snapshot-2022-08-30

```

It can be time consuming to remove all of the snapshots that you have installed. You can remove all of the snapshots on a version, or main with one command.



```
$ swiftly uninstall main-snapshot
$ swiftly uninstall 5.7-snapshot

```

You can see what toolchahins remain with the list subcommand like this:



```
$ swiftly list


Installed release toolchains
----------------------------
Swift 5.10.1 (in use)


Installed snapshot toolchains
-----------------------------

```

Here you have seen how you can uninstall toolchains in different ways using swiftly to help manage your development environment.

## [See Also](/swiftly/documentation/swiftly/uninstall-toolchains/#see-also)

### [Guides](/swiftly/documentation/swiftly/uninstall-toolchains/#Guides)

[Install Swift Toolchains](/swiftly/documentation/swiftly/install-toolchains)Install swift toolchains with Swiftly.[Use Swift Toolchains](/swiftly/documentation/swiftly/use-toolchains)Using installed swift toolchains.[Add Shell Auto-completions](/swiftly/documentation/swiftly/shell-autocompletion)Generate shell auto-completions for Swiftly.[Update Swift Toolchain](/swiftly/documentation/swiftly/update-toolchain)Update swift toolchains.[Install Swiftly Automatically](/swiftly/documentation/swiftly/automated-install)Automatically install swiftly and Swift toolchains.
- [ Uninstall Swift Toolchains ](/swiftly/documentation/swiftly/uninstall-toolchains/#app-top)

- [ Overview ](/swiftly/documentation/swiftly/uninstall-toolchains/#overview)

- [ See Also ](/swiftly/documentation/swiftly/uninstall-toolchains/#see-also)