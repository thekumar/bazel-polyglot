# Sample Bazel WORKSPACE with support for multiple languages.

This is a sample repository to help you get started with setting up your polyglot bazel monorepo workspace.
I have taken an opinionated approach to how the repo is structured. It is optimized for quick iterative development (achieved via fast incremental builds) and high reliability (fast and lightweight unit tests, which encourages developers to write and run more tests).

* `tools` - *Build Toolchains reside here. (Bazel rule defs, utilities, etc.)*
* `third_party` - *All 3rd party dependencies, including node_modules, external java libraries, go packages, etc. reside here.*
* `src` - *Top level classification by language.*
    * `ts` - Typescript (and Javascript)
      - `src` - Source packages belong here. - *Second level by server (services, server libraries) vs. client (client libs, commands, apps, etc.) vs. common. But, this is flexible and you can choose whatever structure is apt for you.*
        - `server`
          - `path/to/package/api` - *API package with interfaces.* **Depends only on other APIs and Protobuf interfaces**
          - `path/to/package/impl` - *Implementation of API interfaces.* **Depends on APIs and Impls**
          - `path/to/package/mocks` - *Mock implementations for tests* **Depends on this and other APIs**
          - `path/to/package/test` - *Unit tests* **Depends on APIs & Mocks, and only on this package's Impl**
        - `client`
        - `common`
      - `tst` - **Integration test** packages belong here. - *Ideally would follow similar structure to src.*
        - `server/path/to/package/` - *Integration tests for the same source package*
    * `go` - Golang
      - `src`
      - `tst`
    * `java` - Java
      - `src`
      - `tst`
    * `proto` - Protobuf

It is important to note how the dependencies are set up.
* API packages can only depend on API packages, and are limited to only defining lightweight interfaces.
* Impl packages can depend on anything, and thus tend to be the most resource intensive when built.
* Mocks provide a lightweight dummy implementation that can be swapped for in tests.
* Unit tests should only depend on APIs and Mocks in addition to the implementation package they are testing. This way, running unit tests becomes a fast and lightweight task.
