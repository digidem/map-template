# Template Background Maps

This repository serves as a template for generating map tiles using GitHub Actions and [mapgl-tile-renderer](https://github.com/ConservationMetrics/mapgl-tile-renderer). Once generated, the tiles are automatically uploaded to [Earth Defenders Toolkit Cloud](https://github.com/digidem/edt-cloud) for distribution to partners or for synchronization with Kakawa devices.

To use this template repository effectively, you must configure several secrets and customize the `manifest.json` file according to your project's requirements.

## Using This Template

To create a new repository from this template, click on the "Use this template" button located at the top of the repository page. This will prompt you to enter the new repository details.

## Setting up Secrets

In your new repository, set up the following secrets:

- `API_KEY`: The API key for accessing the tile rendering service (e.g., Planet, Mapbox, Google).
- `SSH_KEY`: The private SSH key that corresponds to the public key added to the server for secure file transfer.

Navigate to your repository settings, access the "Secrets" section, and add new repository secrets with the names `API_KEY` and `SSH_KEY`.

## Customizing `manifest.json`

The `manifest.json` file holds the configuration for the tile generation process. Modify the following variables to suit your project:

- `name` (required): The base name for the generated `.mbtiles` file.
- `style` (required): The map style to be used for tile generation. Specify 'self' for a self-provided style or use an online source such as 'mapbox', 'bing', 'planet', or 'esri'.
- `monthyear` (optional): The month and year for which the Planet tiles are generated, formatted as `YYYY-MM`. This is required if using 'planet' as the online source for tile generation.
- `bounds` (required): The geographical bounds for tile generation, formatted as `"minLongitude,minLatitude,maxLongitude,maxLatitude"`. Use a tool like [boundingbox](https://boundingbox.klokantech.com/) to obtain the values in CSV format.
- `zoom_level` (required): The max zoom level at which the tiles will be generated. (e.g., `16`).

Example options for `manifest.json`:

## Workflow Overview

The GitHub Actions workflow, defined in `.github/workflows/gen-tiles.yml`, executes the following steps:

1. Checks out the code from the repository.
2. Reads the `manifest.json` file and prepares the necessary command with the specified variables.
3. Executes a mapgl-tile-renderer Docker container to produce the tiles according to the `manifest.json` configuration.
4. Either uploads the generated `.mbtiles` file as an artifact or transfers it to a remote server via SSH, as configured in the workflow.

Review and adjust the workflow file to meet the specific needs of your project.
