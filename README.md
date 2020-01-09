# Introduction

This repository contains source code for custom builders. You can use these images as build steps for
[Google Cloud Build](https://cloud.google.com/cloud-build/docs/).

These are not official Google products.

## How to use a custom builder

Google Cloud Build executes a build as a series of build steps. Each build step is run in a Docker container. See
the [Cloud Build documentation](https://cloud.google.com/cloud-build/docs/overview) for more details
about builds and build steps.

### Before you begin

1.  Select or create a [Google Cloud project](https://console.cloud.google.com/cloud-resource-manager).
2.  Enable [billing for your project](https://support.google.com/cloud/answer/6293499#enable-billing).
3.  Enable [the Cloud Build API](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com).
4.  Install and initialize [the Cloud SDK](https://cloud.google.com/sdk/docs/).

### Build the build step from source

To use a customer builder as a build step, you need to download the source code from this
repo and build the image.

The example below shows how to download and build the image for the `cdxgen` build step on a Linux or Mac OS X workstation:

1. Clone the `cloud-builders-community` repo:

    ```sh
    $ git clone https://github.com/CloudBuildr/google-custom-builders
    ```

2. Go to the directory that has the source code for the `cdxgen` Docker image:

    ```sh
    $ cd google-custom-builders/cdxgen
    ```

3. Build the Docker image:

    ```
    $ gcloud builds submit --config cloudbuild.yaml .
    ```

4. View the image in Google Container Registry:

    ```sh
    $ gcloud container images list --filter cdxgen
    ```

### Use the build step with Cloud Build build

Once you've built the Docker image, you can use it as a build step in a Cloud Build build.

For example, below is the `cdxgen` build step in a YAML
[config file](https://cloud.google.com/cloud-build/docs/build-config), ready to be used in a Cloud Build build:

```yaml
steps:
    - name: "gcr.io/$PROJECT_ID/cdxgen"
      args: ["--output", "bom.xml", "src"]
```
