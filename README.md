# Swift Styling Configuration

This repository contains the SwiftLint and SwiftFormat configuration files to use on all swift projects. It also contains a couple helpful git hooks to further aid in enforcing styling.

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

We understand not all of the defaults in this repository will be appropriate for every project. As such, you are able to override specific linting rules by providing your own `.swiftlint.yml` file at the root of your project while referencing these linting rules by default.

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

## Xcode Editor Extension

Install the following tools to help enforce formatting within your project.

```sh
brew install --cask swiftformat-for-xcode
brew upgrade --cask swiftformat-for-xcode
```

# Git hook

Run the following scripts to install a git hook to check for linting and formatting errors on push.

```sh
brew install pre-commit
pre-commit install --hook-type pre-commit --hook-type pre-push # creates the commit hook locally
```
