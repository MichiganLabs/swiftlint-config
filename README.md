# Swift Styling Configuration

This repository contains the SwiftLint and SwiftFormat configuration files to use on all Swift projects. It also contains a couple helpful git hooks to further aid in enforcing styling.

To use MichiganLab's default rules in your project, add this repo as a submodule at the root of the project.

```sh
git submodule add https://github.com/MichiganLabs/swiftlint-config.git
```

To pull the latest changes of this repository, call the following two commands.

```sh
git submodule sync --recursive
git submodule foreach git pull
```

# SwiftLint Configuration
Make sure to install SwiftLint on your machine. Although you can install Swiftlint globally on your machine using Homebrew, the recommended approach is to install Swiftlint via Mint. Click [here](https://github.com/realm/SwiftLint) for more information on how to install Swiftlint.


```sh
swiftlint lint --config submodule-dir/.swiftlint.yml
```

## Xcode Build Phase
See below for an example Build Phase.

<details>
<summary>Swiftlint Installed Globally</summary>

```sh
set -e

if ! which swiftlint >/dev/null; then
    echo "error: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
    exit 1
fi

echo "swiftlint version:"
swiftlint version

# Uses the root .swiftlint.yml file that extends the practice specific file.
swiftlint lint --config swiftlint-config/.swiftlint.yml
```
</details>

<details>
<summary>Swiftlint Installed via Mint with Homebrew</summary>

```sh
export PATH="$PATH:/opt/homebrew/bin"

echo "swiftlint version:"
mint run realm/swiftlint swiftlint "--version"

# Uses the root .swiftlint.yml file that extends the practice specific file.
mint run realm/swiftlint swiftlint "--config" swiftlint-config/.swiftlint.yml
```
</details>


## Update CI
Finally, you'll need to update your CI configuration to pull the submodule when it goes to checksout your project.

<details>
<summary>Gitlab</summary>

```yml
before_script:
    - git submodule sync --recursive
    - git submodule foreach git pull
```

</details>

## SwiftLint Overrides

We understand not all of the defaults in this configuration file will be appropriate for every project. As such, you are able to override specific linting rules by providing your own `.swiftlint.yml` file at the root of your project while referencing these linting rules by default.

*NOTE*: You will need to update your `swiftlint` command to use your new configuration file.

<details>
<summary>Example</summary>

```yml
# Overrides from the parent configuration. Points to the linting defaults.
parent_config: swiftlint-config/.swiftlint.yml

indentation: 2

excluded:
  - test_derived_data/*

```

</details>

Just be sure to also update the paths in your [Build Phase](#xcode-build-phase) to your file located at the root of your project.


# SwiftFormat Configuration

Make sure to install SwiftLint on your machine. Although you can install SwiftFormat globally on your machine using Homebrew, the recommended approach is to install SwiftFormat via Mint. Click [here](https://github.com/nicklockwood/SwiftFormat) for more information on how to install SwiftFormat.

Once you've installed SwiftFormat, you'll want to specify the version of Swift you are using for your project by adding a `.swift-version` file to the root of your project. This is a text file that should contain the minimum Swift version supported by your project, and is a standard already used by other tools.

## Configuration

It is recommended to use the `.swiftformat` file located in the `swift-styleing` submodule when using `swiftformat`.

```sh
swiftformat --config swift-styling/.swiftformat .
```

Setting the configuration in this way will _ONLY_ use the rules specified in this file.

However, we understand not all of the defaults in this configuration file will be appropriate for every project. As such, you are able to override specific formatting rules with the following approaches.

### Option 1: Extending Defaults

**Step 1:** Remove the `--config` argument when calling the `swiftformat` command. SwiftFormat will (by default), automatically look for and apply rules based on `.swiftformat` files in found in subdirectories of the call site.

**Step 2:** Create a symbolic link at the root of your repository that points to the default swift formatting rules.

```sh
ln -s swift-styling/.swiftformat .swiftformat
```

**Step 3:** Add your own `.swiftformat` files at subdirectories within your repository. These addtional `.swiftformat` files will add to and override any rules set by the symlink file.

### Option 2: Brute Force Rules

Although this option is not as ideal as option 1, you can simply copy all of the default rules from the `.swiftformat` file located within the `swift-styling` directory and create a new `.swiftformat` file at the root of your repository. It will then be your responsibility to update this rule set as the defaults are updated.

## Xcode Editor Extension

Install the following tools to help enforce formatting while developing in Xcode.

```sh
brew install --cask swiftformat-for-xcode
brew upgrade --cask swiftformat-for-xcode
```

After you've installed the "SwiftFormat for Xcode" application, launch the application and follow the instructions to finish the installation.

# Git hook

Run the following scripts to install a git hook to check for linting and formatting errors on push.

```sh
brew install pre-commit
pre-commit install --config swiftlint-config/.pre-commit-config.yaml --hook-type pre-push
```
