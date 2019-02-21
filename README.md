# Google Cloud Firestore Emulator

A [Google Cloud Firestore Emulator](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/firestore/) container image. The image is meant to be used for creating an standalone emulator for testing.

## Environment

The following environment variables must be set:

- `FIRESTORE_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

## Connect application with the emulator

The following environment variables need to be set so your application connects to the emulator instead of the production Cloud Firestore environment:

- `FIRESTORE_EMULATOR_HOST`: The listen address used by the emulator (ie. `firestore-emulator:8080`)
- `FIRESTORE_PROJECT_ID`: The ID of the Google Cloud project used by the emulator.

## Custom commands

This image contains a script named `start-firestore` (included in the PATH). This script is used to initialize the Datastore emulator.

### Starting an emulator

By default, the following command is called:

```sh
start-firestore
```
### Starting an emulator with options

This image comes with options. Check [Firestore Emulator GCloud Wide Flags](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/firestore/). `--legacy`, `--data-dir` and `--host-port` are not supported by this image.

## Creating a Firestore emulator with Docker Compose

The easiest way to create an emulator with this image is by using [Docker Compose](https://docs.docker.com/compose). The following snippet can be used as a `docker-compose.yml` for a firestore emulator:

```YAML
version: "2"

services:
  firestore:
    image: perrystallings/cloud-firestore-emulator
    environment:
      - FIRESTORE_PROJECT_ID=project-test
  app:
    image: your-app-image
    environment:
      - FIRESTORE_EMULATOR_HOST=firestore
      - FIRESTORE_PROJECT_ID=project-test
```
