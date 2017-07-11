# Package metadata

The metadata can be expressed in many media types: in the package tarball, it is as an "Erlang term file" that can be read with [`file:consult/1`][]. This specification describes the format used in the package tarball.

## Types

All types listed here are Erlang terms.

  + `string` - A UTF-8 encoded Erlang binary
  + `boolean` - A boolean (`true` or `false`)
  + `list(Inner)` - A list with an `Inner` type
  + `kvlist(Key => Value)` - A list with two element tuples where the first element is of type `Key` and the second `Value`

kvlists are normally generic in the sense that they can have any values for keys, but they can also allow only specific keys: `kvlist(...)` where the keys will be listed in relation to the type.

## Format

All keys are strings.

  + `name (string) (required)`

    Package name

  + `elixir (string) (optional)`

    [Version requirement][] on Elixir

  + `version (string) (required)`

    Release version, required to be in the [Semantic Version][] format

  + `app (string) (required)`

    OTP application name, usually the same name as the package but can differ

  + `description (string) (required)`

    Package description, recommended to be a single paragraph

  + `files (list(string)) (required)`

    Files in the package tarball contents

  + `licenses (list(string)) (required)`

    The package's licenses

  + `maintainers (list(string)) (optional)`

    The package's maintainers, can be a list of names and/or emails

  + `links (kvlist(string => string)) (optional)`

    Links related to the package where the key is the link name and the value is the URL

  + `requirements (kvlist(string => kvlist(...))) (required)`

    All dependencies of the package where the key is the dependent name,
    all keys below are required

    + `app (string)`

      OTP application name, usually the same name as the package

    + `optional (boolean)`

      If the package is required or not

    + `requirement (string)`

      [Version requirement][] on the dependent

    + `repository (string, optional)`

      Dependency repository

  + `build_tools (list(string)) (required)`

      Names of build tools that can build the package

  + `extra (kvlist(string => kvlist(...))) (optional)`

      Extra information about the package

### Optional dependencies

An optional dependency will only be used if a package higher up the dependency chain also depends on it (only if that the dependency is not defined as optional as well).

#### Example

The ecto package has an optional dependency on postgrex. If a project X depends on ecto and postgrex, both dependencies will be used and ecto's version requirement on postgrex has to be satisfied. But if project X only depends on ecto without listing postgrex as a dependency, the package can be ignored.

[`file:consult/1`]: http://www.erlang.org/doc/man/file.html#consult-1
[Semantic Version]: http://semver.org/
[Version requirement]: http://elixir-lang.org/docs/stable/elixir/Version.html
