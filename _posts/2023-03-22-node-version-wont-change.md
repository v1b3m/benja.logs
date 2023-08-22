---
title: "Node version won't change"
categories:
  - dev
tags:
  - node
  - nvm
  - fnm
---

I had an issue where the node version just refused to change for some bizarre reason. After hours of struggle, I had an insight, "what if I have two clashing node version managers?"

Turns out I was correct, a few days/weeks back I installed `fnm` as it was a requirement for some project I was hacking on. Well, turns out `fnm` took precedence over `nvm` and the node version being used was the one set via `fnm`

The commands are a lot simillar though, so for now, I'll just use `fnm`. I prefer `nvm` though.

### `nvm` cheatsheet

Here's a basic cheatsheet for `nvm` (Node Version Manager), which is a popular tool for managing multiple versions of Node.js. Please note that `nvm` commands and usage may evolve over time, so always refer to the official documentation for the most up-to-date information.

**Installation:**

```sh
# Install nvm using the install script
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

**Usage:**

- **Install a Specific Node.js Version:**

  ```sh
  nvm install <version>
  ```

- **Use a Specific Node.js Version:**

  ```sh
  nvm use <version>
  ```

- **Use a Specific Node.js Version for the Current Directory:**

  ```sh
  nvm use <version> --local
  ```

- **Set a Default Node.js Version:**

  ```sh
  nvm alias default <version>
  ```

- **List Installed Node.js Versions:**

  ```sh
  nvm ls
  ```

- **List Remote Node.js Versions:**

  ```sh
  nvm ls-remote
  ```

- **Run a Node.js Command with a Specific Version:**

  ```sh
  nvm exec <version> <command>
  ```

- **Uninstall a Node.js Version:**

  ```sh
  nvm uninstall <version>
  ```

- **Help and Documentation:**

  ```sh
  nvm --help
  ```

**Examples:**

- Install Node.js version 14:

  ```sh
  nvm install 14
  ```

- Use Node.js version 16:

  ```sh
  nvm use 16
  ```

- Set Node.js version 12 as the default:

  ```sh
  nvm alias default 12
  ```

- List installed Node.js versions:

  ```sh
  nvm ls
  ```

Remember that `nvm` provides powerful version management capabilities, allowing you to easily switch between different Node.js versions. Properly configuring your shell and using `nvm` with your projects will help you work efficiently with different Node.js environments.

## `fnm` cheatsheet

Here's a basic cheatsheet for `fnm` (Fast Node Manager), which is a version manager for Node.js. It allows you to easily manage and switch between different versions of Node.js. Keep in mind that `fnm` commands and usage may change over time, so it's always a good idea to refer to the official documentation for the most up-to-date information.

**Installation:**

```sh
# Install fnm using the install script (Linux/macOS)
curl -fsSL https://github.com/Schniz/fnm/raw/master/.ci/install.sh | bash

# Install fnm using Homebrew (macOS)
brew install Schniz/tap/fnm
```

**Usage:**

- **Install a Specific Node.js Version:**

  ```sh
  fnm install <version>
  ```

- **Use a Specific Node.js Version:**

  ```sh
  fnm use <version>
  ```

- **Use a Specific Node.js Version for the Current Directory:**

  ```sh
  fnm local <version>
  ```

- **Set a Default Node.js Version:**

  ```sh
  fnm default <version>
  ```

- **List Installed Node.js Versions:**

  ```sh
  fnm ls
  ```

- **List Remote Node.js Versions:**

  ```sh
  fnm ls-remote
  ```

- **Run a Node.js Command with a Specific Version:**

  ```sh
  fnm exec <version> -- <command>
  ```

- **Uninstall a Node.js Version:**

  ```sh
  fnm uninstall <version>
  ```

- **Help and Documentation:**

  ```sh
  fnm help
  ```

**Examples:**

- Install Node.js version 14:

  ```sh
  fnm install 14
  ```

- Use Node.js version 16:

  ```sh
  fnm use 16
  ```

- Set Node.js version 12 as the default:

  ```sh
  fnm default 12
  ```

- List installed Node.js versions:
  
  ```sh
  fnm ls
  ```

Remember that `fnm` aims to be fast and efficient, providing seamless version management. It's important to ensure that your shell is properly configured to use `fnm` and that you follow best practices for managing Node.js versions in your projects.