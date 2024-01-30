# Stable Feature Checklist

<!--
  Copyright 2024, Bitwise IO, Inc.
  Copyright 2018-2021 Cargill Incorporated
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->

New Splinter features are usually added to the code as experimental.
Experimental features are often incomplete (needing more implementation,
documentation, testing, etc.). The new feature is defined in Cargo.toml and
added to the list of experimental features. The name of the feature should be
short and descriptive. For more information on the mechanics of features, see
[Rust features](https://doc.rust-lang.org/cargo/reference/manifest.html#the-features-section).

`Cargo.toml` excerpt with an experimental feature:

``` toml
[features]
default = []

stable = []

experimental = ["new-feature"]

new-feature = []
```

Once a feature has completed the following checklist, the feature may be moved
from experimental into stable. Following are the required steps and tasks that
must be complete before an experimental feature can be moved into stable:

* New APIs must be "stable". APIs include library code, CLIs, REST APIs, etc.
    * The public API must be finalized--any future changes will need to remain
      backward compatible, following semver.
    * All internal implementation details must not be exposed and must be
      private.
    * The public API must not contain references to any of Splinter's external
      dependencies. As other dependencies change (and potentially the API of the
      dependencies), they should not break our stable APIs. The only way to
      accomplish this is to not expose the use of those dependencies on public
      API surfaces.
* A stable feature cannot depend on an experimental feature. Thus lower-level
  features must be stabilized before features built upon them.
* All maintainers should perform a final code review, to get the widest amount
  of feedback possible prior to the feature becoming stable.
* Unit tests should exist that fully exercise the feature's public APIs. The
  tests should be defined and constructed to assist us in determining when we
  may be accidentally changing the public API's behavior (thus requiring a
  consideration of backward compatibility and semver). Ultimately the
  necessary level of testing should be considered on a per-feature basis by
  the maintainers, as part of the final review.  
* New APIs should be fully documented.
    * If the feature modifies a CLI, the CLI documentation must reflect that
      change.
    * If the feature modifies a REST API, the associated REST API  documentation
      (the openapi.yaml file) must be updated.
    * If the feature exposes a new library API, that API must have complete doc
      comments. (Usually in the form of Rust doc.)
    * Features should be documented at a feature level in the Splinter Docs
      repo or/and have complete rust API doc comments.
* New configuration settings or options have been added to the
  [example splinterd config file](https://github.com/splintercommunity/splinter/blob/main/splinterd/packaging/splinterd.toml.example)
  (where applicable).
* Any new cli commands or subcommands have been added to the Splinter CLI
  [bash completions file](https://github.com/splintercommunity/splinter/blob/main/cli/packaging/ubuntu/completions/splinter)
  (where applicable).


When all of the above items have been completed and merged, a final PR should be
created that moves the features from experimental into stable. There should be
no other changes included in this PR.

`Cargo.toml` excerpt with a stable feature:

``` toml
[features]
default = []

stable = ["new-feature"]

experimental = []

new-feature = []
```

In some cases, the feature will also be added to default or removed from
the Cargo.toml if the feature was an update to an existing feature.
