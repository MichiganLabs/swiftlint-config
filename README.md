# SwiftLint Config

Contains the SwiftLint Configuration to use on all iOS projects

To use this in your project add this repo as a submodule at the root of the project.

```sh
git submodule add git@code.michiganlabs.com:MichiganLabs/swiftlint-config.git
```

Then, where ever you run SwiftLint (probably in a Build Phase script), make sure to call run SwiftLint with the configuation contained in this repo.

```sh
swiftlint lint --config submodule-dir/.swiftlint.yml
```

See below for an example Build Phase.

```ruby
if which swiftlint >/dev/null; then
    echo "swiftlint version:"
    swiftlint version

    if [ "${CONFIGURATION}" == "Local" ]; then
        swiftlint lint --config swiftlint-config/.swiftlint.yml
    fi
else
    echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```