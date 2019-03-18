# SwiftLint Config

Contains the SwiftLint Configuration to use on all iOS projects

To use this in your project add this repo as a submodule at the root of the project. Then, where ever you run SwiftLint (probably in a Build Phase script), make sure to call run SwiftLint with the configuation contained in this repo. This will look something like...

```
swiftlint lint --config submodule-dir/.swiftlint.yml
```
