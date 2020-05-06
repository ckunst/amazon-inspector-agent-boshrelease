# Amazon Inspector Bosh release

* Clone this repository and execute:

  `bosh init-release`


* The config directory is created with the following contents:
  ```
  ├ config
  │   ├ blobs.yml
  │   └ final.yml
  ├ jobs
  ├ packages
  └ src
  ```

* Copy Amazon Inspector Agent to `src` directory, example:
  ```
  ├ src
  │   ├  install
  ```

* Register blobs or a new version using the following command:
  ```
  bosh add-blob src/install install
  ```
This populates the `blobs.yml` with blob filename, filename size, and SHA

* Update `packages > amazon-inspector-agent > spec`, to include the blob
  ```
  files:
    - inspector
  ```

* Update the file under `config > final.yml` to include the `blobstore_path` variable:

  ```
  blobstore:
    provider: local
    options:
      blobstore_path: /Users/ckunst/workspace/hsbc_aws/amazon-inspector-boshrelease/blobs

  name: amazon-inspector-boshrelease
  ```

* Create the Release
  ```
  bosh create-release --final --force --tarball=amazon-inspector-boshrelease.tgz
  ```

* Update the file under `runtime-config > amazon-inspector.yml` file and update the version number, from the output of the previous command
  ```
  version: 1
  ```

* Upload the bosh Release
  ```
  bosh upload-release amazon-inspector-boshrelease.tgz
  ```

* Update the runtime config
  ```
  bosh update-runtime-config --name=amazon-inspector runtime-config/amazon-inspector.yml
  ```

* Trigger `Apply Changes` from Ops Manager
