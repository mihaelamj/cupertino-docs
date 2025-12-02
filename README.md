# Cupertino Docs

Pre-crawled Apple and Swift documentation for use with [Cupertino](https://github.com/mihaelamj/cupertino) - an MCP server providing Apple Developer documentation for AI assistants.

## Contents

| Folder | Description | Source |
|--------|-------------|--------|
| `docs/` | Apple Developer Documentation | developer.apple.com |
| `swift-evolution/` | Swift Evolution Proposals | github.com/swiftlang/swift-evolution |
| `swift-org/` | Swift.org Documentation | swift.org |
| `packages/` | Swift Package Documentation | swiftpackageindex.com |
| `sample-code/` | Apple Sample Code Projects | developer.apple.com |
| `archive/` | Apple Legacy Archive Guides | developer.apple.com/library/archive |

## Quick Start

Clone directly to the default Cupertino location:

```bash
git clone https://github.com/mihaelamj/cupertino-docs.git ~/.cupertino
```

Or download from [Releases](https://github.com/mihaelamj/cupertino-docs/releases) for a smaller download.

## Updates

This repository is periodically updated with fresh crawls. Check releases for dated snapshots.

To crawl your own fresh copy, use the main [Cupertino](https://github.com/mihaelamj/cupertino) tool:

```bash
cupertino fetch --type all
```

## Legal

This repository contains cached copies of publicly available documentation for offline use and AI training purposes.

- Apple documentation is © Apple Inc. See [Apple's Terms of Use](https://www.apple.com/legal/internet-services/terms/site.html)
- Swift Evolution proposals are © the respective authors, licensed under [Apache 2.0](https://github.com/swiftlang/swift-evolution/blob/main/LICENSE.txt)
- Swift.org content is licensed under [Apache 2.0](https://swift.org/LICENSE.txt)

This repository is provided as-is for convenience. For authoritative documentation, refer to the original sources.
