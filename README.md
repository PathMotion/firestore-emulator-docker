# Google Cloud Datastore Emulator

A [Google Cloud Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator/) container image. The image is meant to be used for creating an standalone emulator for testing.

## Environment

The following environment variables must be set:

- `DATASTORE_LISTEN_ADDRESS`: The address should refer to a listen address, meaning that `0.0.0.0` can be used. The address must use the syntax `HOST:PORT`, for example `0.0.0.0:8081`. The container must expose the port used by the Datastore emulator.
- `DATASTORE_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

## Connect application with the emulator

The following environment variables need to be set so your application connects to the emulator instead of the production Cloud Datastore environment:

- `DATASTORE_EMULATOR_HOST`: The listen address used by the emulator.
- `DATASTORE_PROJECT_ID`: The ID of the Google Cloud project used by the emulator.

## Custom commands

This image contains a script named `start-datastore` (included in the PATH). This script is used to initialize the Datastore emulator.

### Starting an emulator

By default, the following command is called:

```sh
start-datastore
```
### Starting an emulator with options

This image comes with the following options: `--no-store-on-disk` and `--consistency`. Check [Datastore Emulator Start](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/datastore/start). `--legacy`, `--data-dir` and `--host-port` are not supported by this image.

```sh
start-datastore --no-store-on-disk --consistency=1.0
```

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
      - DATASTORE_EMULATOR_HOST=firestore
      - FIRESTORE_PROJECT_ID=project-test
```