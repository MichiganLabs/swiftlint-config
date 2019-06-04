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

```sh
set -e

if ! which swiftlint >/dev/null; then
    echo "error: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
    exit 1
fi

echo "swiftlint version:"
swiftlint version

swiftlint lint --config swiftlint-config/.swiftlint.yml
```

Finally, you'll need to update your `.gitlab-ci.yml` file to pull the submodule when it checksout your project. To do this, add the following two lines to your `before_script` section.

```yml
before_script:
    - git submodule sync --recursive
    - git submodule foreach git pull
```

Likewise, you'll need to run these commands locally on your machine when you want to pull the most recent changes from the submodule.

## Ignored folders
The following folders are automatically ignored because they typically contain source code that we did not create and do not want to modify:

* `Carthage`
* `Vendor`
* `Pods`