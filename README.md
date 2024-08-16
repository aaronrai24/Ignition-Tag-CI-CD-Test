# ign-tag-ci-cd

___

## Overview

- This repository is for simulating the root tag not getting imported when using the `ign-tag-ci-cd` module to export and import tags. In this replication a tag structure like this is used:

```json
- Root
  - Child1(Folder)
    - Grandchild1(Tag)
  - Child2(Tag)
```

- Child2 is will be skipped during the import process. 

## Steps to import

1. Clone this repository.
2. Import the tags into the gateway using the following command:

  ```bash
  ./scripts/tag-import.sh -p default -c d -d /tags https://ign-tag-ci-cd.localtest.me   
  ```

## Steps to export

1. To export run the following command:

  ```bash
  ./scripts/tag-export.sh -p default -r True -d -f /tags https://ign-tag-ci-cd.localtest.me   
  ```
