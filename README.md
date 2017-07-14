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
  requires: [ COLLECTION_ID_a, COLLECTION_ID_b, .. ]
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
      platforms: [PLATFORM_1, PLATFORM2, .. ]
      patch: PATCH_CONTENT
    - PACKAGE_3
    ...
    - PACKAGE_N

COLLECTION_ID_B:
  ...
```

### Element and Status

| Element | Decscription | Required? | Supported? |
| ------- | ------------ | --------- | ---------- |
| `COLLECTION_ID` | Unique string to describe a collection. It is recommended to use SCL name if possible. | Yes | Yes |
| `COLLECTION_ID/name`          | Collection name. just for completeness | No | Yes |
| `COLLECTION_ID/requires` | List of collections that it depends on | No | No |
| `COLLECTION_ID/packages` | List of SRPM belonging to particular collection by build order. | Yes | Yes |
| `COLLECTION_ID/packages/PACKAGE/macros` | List of macros to break circular dependencies. Insert those to top of the RPM spec file such as `rpmbuild --define` | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/replaced_macros` | List of macros to break circular dependencies. Those replaces the value of macro that has already defined in the RPM spec file. | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/cmd` | Not recommended. But if `macros` and `replaced_macros` are not enough for your requirement, you can use it. For example `sed -i 'something' foo.spec` to edit RPM spec file. You can use both array and string element as the value. | No | Yes |
| `COLLECTION_ID/packages/PACKAGE/platforms` | Set of platforms that are considered to build. | No | No |
| `COLLECTION_ID/packages/PACKAGE/patch` | Deprecated, due to that it can be replaced as `macros`, `replaced_macros`, or `cmd` element. | No | No |

**Caution**: "Supported?" column means whether the element is supported by the tool to parse the recipe file and build automatically: [RPM List Builder](https://github.com/sclorg/rpm-list-builder).
  If the element is Supported?: No, the status is only proposed. It can be chnaged in the future.

### Example

Here is [an example](example.yml) of the recipe.
