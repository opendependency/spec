# OpenDependency Module Specification

A module is a description of an arbitrary artefact supposed to be used by other modules or to be consumable by an end-user.
All modules together provide an overview of what modules exist and how those modules are related to other modules in the dimension of type and version.

## Property Descriptions

- **`namespace`** *string*

  This REQUIRED property specifies the module namespace.

  The namespace groups one or more modules with each other.

  Constraints:
  - MUST be between 1 and 63 characters
  - MUST contain only lowercase alphanumeric characters, '-' or '.'
  - MUST start with an alphabetic character
  - MUST end with an alphanumeric character

- **`name`** *string*

  This REQUIRED property specifies the module name.

  The module name represents this module in a namespace.

  Constraints:
  - MUST be between 1 and 63 characters
  - MUST contain only lowercase alphanumeric characters, '-' or '.'
  - MUST start with an alphabetic character
  - MUST end with an alphanumeric character
  - MUST be unique within a namespace

- **`type`** *string*

  This REQUIRED property specifies the module type.

  The module type represents in the most cases the underlying technology.
  A list of [well-known module types](#well-known-module-types) specifies common used types.

  Constraints:
  - MUST be between 1 and 63 characters
  - MUST contain only lowercase alphanumeric characters, '-' or '.'
  - MUST start with an alphabetic character
  - MUST end with an alphanumeric character

- **`version`** *object*

  This REQUIRED property specifies the module version details.

  - **`name`** *string*

    This REQUIRED property specifies the actual module version.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphanumeric character
    - MUST end with an alphanumeric character
    - MUST be unique within module namespace, module name and module type

  - **`schema`** *string*

    This OPTIONAL property describes the used version schema.

    A list of [well-known module version schemas](#well-known-module-version-schemas) specifies common used schemas.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphabetic character
    - MUST end with an alphanumeric character

  - **`replaces`** *array of strings*

    This OPTIONAL property specifies an array of module versions replaced by this module version.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphanumeric character
    - MUST end with an alphanumeric character

- **`annotations`** *string-string map*

  This OPTIONAL property contains arbitrary metadata.

  Constraints:
  - both key and value MUST be strings
  - key MUST be between 1 and 63 characters
  - key MUST contain only lowercase alphanumeric characters, '-' or '.'
  - key MUST start with an alphabetic character
  - key MUST end with an alphanumeric character
  - keys MUST be unique within this map
  - keys SHOULD be prefixed to namespace
  - keys SHOULD be named using a reverse domain notation (e.g. `com.example.key`)
  - value MUST be present but MAY be an empty string
  - value MUST be 253 characters or less

- **`dependencies`** *array of objects*

  This OPTIONAL property specifies zero, one or more dependencies to other modules.

  - **`namespace`** *string*

    This REQUIRED property specifies the dependent module namespace.

    The namespace groups one or more modules with each other.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphabetic character
    - MUST end with an alphanumeric character

  - **`name`** *string*

    This REQUIRED property specifies the dependent module name.

    The module name represents this module in a namespace.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphabetic character
    - MUST end with an alphanumeric character
    - MUST be unique within a namespace

  - **`type`** *string*

    This REQUIRED property specifies the dependent module type.

    The module type represents in the most cases the underlying technology.
    A list of [well-known module types](#well-known-module-types) specifies common used types.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphabetic character
    - MUST end with an alphanumeric character§

  - **`version`** *string*

    This REQUIRED property specifies the dependent module version.

    Constraints:
    - MUST be between 1 and 63 characters
    - MUST contain only lowercase alphanumeric characters, '-' or '.'
    - MUST start with an alphanumeric character
    - MUST end with an alphanumeric character
    - MUST be unique within module namespace, module name and module type

  - **`direction`** *string*

    This OPTIONAL property specifies the dependency direction.

    If not defined, `UPSTREAM` is used.

    Constraints:
    - MUST be one of `UPSTREAM` or `DOWNSTREAM`

## Well-Known Module Types

| Name                                                               | Identifier                 |
|--------------------------------------------------------------------|----------------------------|
| [Go](https://golang.org)                                           | `go`                       |
| [Java](https://java.com)                                           | `java`                     |
| [Helm Chart](https://helm.sh)                                      | `sh.helm.chart`            |
| [Kubernetes Custom Resource Definition (CRD)](https://k8s.io)      | `io.k8s.crd`               |
| [OCI Image](https://opencontainers.org/)                           | `org.opencontainers.image` |
| [OpenAPI](https://openapis.org/)                                   | `org.openapis` |

## Well-Known Module Version Schemas

| Name                                                                     | Identifier      |
|--------------------------------------------------------------------------|-----------------|
| [Semantic Versioning 1.0.0](https://semver.org/lang/de/spec/v1.0.0.html) | `org.semver.v1` |
| [Semantic Versioning 2.0.0](https://semver.org/lang/de/spec/v2.0.0.html) | `org.semver.v2` |


## Example Modules

*Example showing a Helm Chart:*
```json
{
  "namespace": "com.example.shop",
  "name": "order",
  "type": "sh.helm.chart",
  "version": {
    "name": "v1.2.3",
    "schema": "org.semver.v2"
  },
  "dependencies": [
    {
      "namespace": "com.example.shop",
      "name": "order",
      "type": "org.opencontainers.image",
      "version": "v1.2.3"
    },
    {
      "namespace": "io.k8s.apps",
      "name": "deployment",
      "type": "io.k8s.crd",
      "version": "v1"
    },
    {
      "namespace": "io.k8s",
      "name": "service",
      "type": "io.k8s.crd",
      "version": "v1"
    },
    {
      "namespace": "io.k8s",
      "name": "serviceaccount",
      "type": "io.k8s.crd",
      "version": "v1"
    }
  ]
}
```

*Example showing a OCI Image module:*
```json
{
  "namespace": "com.example.shop",
  "name": "order",
  "type": "org.opencontainers.image",
  "version": {
    "name": "v1.2.3",
    "schema": "org.semver.v2"
  },
  "annotations": {
    "docker.version": "20.10.8"
  },
  "dependencies": [
    {
      "namespace": "com.example.shop",
      "name": "base",
      "type": "org.opencontainers.image",
      "version": "v3.0.0"
    }
  ]
}
```

*Example showing a Go module:*
```json
{
  "namespace": "com.example.shop",
  "name": "order",
  "type": "go",
  "version": {
    "name": "v1.2.3",
    "schema": "org.semver.v2"
  },
  "annotations": {
    "go.module": "github.com/example/order",
    "go.version": "1.17"
  },
  "dependencies": [
    {
      "namespace": "com.example.shop",
      "name": "payment",
      "type": "org.openapis",
      "version": "v2.0.0"
    }
  ]
}
```

*Example showing an OpenAPI module:*
```json
{
  "namespace": "com.example.shop",
  "name": "payment",
  "type": "org.openapis",
  "version": {
    "name": "v2.1.0",
    "schema": "org.semver.v2"
  }
}
```

*Example showing a Go module:*
```json
{
  "namespace": "com.example.shop",
  "name": "payment",
  "type": "go",
  "version": {
    "name": "v4.1.1",
    "schema": "org.semver.v2"
  },
  "annotations": {
    "go.module": "github.com/example/payment",
    "go.version": "1.17"
  },
  "dependencies": [
    {
      "namespace": "com.example.shop",
      "name": "payment",
      "type": "org.openapis",
      "version": "v2.1.0",
      "direction": "downstream"
    }
  ]
}
```