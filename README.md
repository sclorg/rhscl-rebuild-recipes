# Rebuild recipes for Software Collections

The rebuild recipe is a YAML file to manage a collection that will include *steps to build the collection from scratch*
We will aim to prepare the recipe for every collection.
This will also help everybody else (users/customers) trying to do the same, so it may be part of the documentation later as well.

The recipes could be also available at https://www.softwarecollections.org/en/docs/ in the future.

## How to create the recipe file

### Deciding the recipe file name

Create your recipe file as `RECIPE_NAME.yml` to manage a list of collection.
For example a recipe file `python.yml` can manage a collection `python27` and `rh-python34`.

### File syntax

Below is the file syntax.

```
COLLECTION_ID_A:
  name: COLLECTION_NAME
  requires:
    - COLLECTION_ID_1
    - COLLECTION_ID_2
    ...
    - COLLECTION_ID_N
  packages:
    - PACKAGE_1
    - PACKAGE_2:
      macros:
        MACRO_NAME_1: MACRO_VALUE_1
        MACRO_NAME_2: MACRO_VALUE_2
        ...
        MACRO_NAME_N: MACRO_VALUE_N
      replaced_macros:
        MACRO_NAME_1: MACRO_VALUE_1
        MACRO_NAME_2: MACRO_VALUE_2
        ...
        MACRO_NAME_N: MACRO_VALUE_N
      cmd: COMMAND,
      cmd:
        - COMMAND_1
        - COMMAND_2
        ...
        - COMMAND_N
      dist: DIST_RE
      platforms:
        - PLATFORM_1
        - PLATFORM_2
        ...
        - PLATFORM_N
      patch: PATCH_CONTENT
    - PACKAGE_3
    ...
    - PACKAGE_N

COLLECTION_ID_B:
  ...
```

### Element and Status

| Element | Decscription | Kind | Required? | Supported? |
| ------- | ------------ | ---- | --------- | ---------- |
| `COLLECTION_ID` | Unique string to describe a collection. It is recommended to use SCL name if possible. | Scalar | Yes | Yes |
| `COLLECTION_ID/name`          | Collection name. just for completeness | Scalar | No | Yes |
| `COLLECTION_ID/requires` | Sequences of collections that the collection depends on | Sequences | No | No |
| `COLLECTION_ID/packages` | Sequences of SRPM belonging to particular collection by build order. | Sequences of Union (Scalar or Mappings) | Yes | Yes |
| `COLLECTION_ID/packages/PACKAGE/macros` | Mappings of macros to break circular dependencies. Insert those to top of the RPM spec file such as `rpmbuild --define` | Mappings | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/replaced_macros` | Mappings of macros to break circular dependencies. Those replaces the value of macro that has already defined in the RPM spec file. | Mappings | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/cmd` | Not recommended. But if `macros` and `replaced_macros` are not enough for your requirement, you can use it. For example `sed -i 'something' foo.spec` to edit RPM spec file. | Union (Scalar or Sequences) | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/dist` | Distribution platforms that are considered to build. The format is a regular expression. A `platforms` element was changed to this element. Use `dist` instead of `platforms` element. | Scalar | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/patch` | Deprecated, due to that it can be replaced as `macros`, `replaced_macros`, or `cmd` element. | Scalar | No | No |
| `COLLECTION_ID/packages/PACKAGE/platforms` | Deprecated, due to that it can be replaced as `dist` element. Set of platforms that are considered to build. | Sequences | No | No |

**Caution**: "Supported?" column means whether the element is supported by the tool to parse the recipe file and build automatically: [RPM List Builder](https://github.com/sclorg/rpm-list-builder).
  If the element is Supported?: No, the status is only proposed. It can be chnaged in the future.

### Example

Here is [an example](example.yml) of the recipe.
