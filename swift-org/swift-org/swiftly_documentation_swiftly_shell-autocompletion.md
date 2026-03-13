---
source: https://www.swift.org/swiftly/documentation/swiftly/shell-autocompletion/
crawled: 2025-11-15T21:58:39Z
---

# Add Shell Auto-completions | Documentation

- [ Swiftly ](/swiftly/documentation/swiftlydocs)

- [ Add Shell Auto-completions ](/swiftly/documentation/swiftly/shell-autocompletion/)

- [ Add Shell Auto-completions ](/swiftly/documentation/swiftly/shell-autocompletion/)

Article# Add Shell Auto-completions

Generate shell auto-completions for Swiftly.## [Overview](/swiftly/documentation/swiftly/shell-autocompletion/#overview)

Swiftly can generate shell auto-completion scripts for your shell to automatically complete subcommands, arguments, options and flags. It does this using the [swift-argument-parser](https://apple.github.io/swift-argument-parser/documentation/argumentparser/installingcompletionscripts/), which has support for Bash, Z shell, and Fish.

You can ask swiftly to generate the script using the hidden `--generate-completion-script` flag with the type of shell like this:



```
swiftly --generate-completion-script <shell>

```

If you have [oh-my-zsh](https://ohmyz.sh/) installed then this command will install the swiftly completions into the default directory:



```
swiftly --generate-completion-script zsh > ~/.oh-my-zsh/completions/_swiftly

```

Otherwise, you’ll need to add a path for completion scripts to your function path, and turn on completion script autoloading. First, add these lines to ~/.zshrc:



```
fpath=(~/.zsh/completion $fpath)
autoload -U compinit
compinit

```

Next, create the completion directory and add the swiftly completions to it:



```
mkdir -p ~/.zsh/completion && swiftly --generate-completion-script zsh > ~/.zsh/completions/swiftly

```

If you have [bash-completion](https://github.com/scop/bash-completion) installed then this command will install the swiftly completions into the default directory:



```
swiftly --generate-completion-script bash > swiftly.bash
# copy swiftly.bash to /usr/local/etc/bash_completion.d

```

Without bash-completion create a directory for the completion and generate the script in there:



```
mkdir -p ~/.bash_completions && swiftly --generate-completion-script bash > ~/.bash_completions/swiftly.bash

```

Add a line to source that script in your `~/.bash_profile` or `~/.bashrc` file:



```
source ~/.bash_completions/swiftly.bash

```

Generate the completion script to any path in the environment variable `$fish_completion_path`. Typically this command will generate the script in the right place:



```
swiftly --generate-completion-script fish > ~/.config/fish/completions/swiftly.fish

```

Once you have installed the completions you can type out a swiftly command, and press a special key (e.g. tab) and the shell will show you the available subcommand, argument, or options to make it easier to assemble a working command-line.

## [See Also](/swiftly/documentation/swiftly/shell-autocompletion/#see-also)

### [Guides](/swiftly/documentation/swiftly/shell-autocompletion/#Guides)

[Install Swift Toolchains](/swiftly/documentation/swiftly/install-toolchains)Install swift toolchains with Swiftly.[Use Swift Toolchains](/swiftly/documentation/swiftly/use-toolchains)Using installed swift toolchains.[Uninstall Swift Toolchains](/swiftly/documentation/swiftly/uninstall-toolchains)Uninstall Swift toolchains.[Update Swift Toolchain](/swiftly/documentation/swiftly/update-toolchain)Update swift toolchains.[Install Swiftly Automatically](/swiftly/documentation/swiftly/automated-install)Automatically install swiftly and Swift toolchains.
- [ Add Shell Auto-completions ](/swiftly/documentation/swiftly/shell-autocompletion/#app-top)

- [ Overview ](/swiftly/documentation/swiftly/shell-autocompletion/#overview)

- [ See Also ](/swiftly/documentation/swiftly/shell-autocompletion/#see-also)