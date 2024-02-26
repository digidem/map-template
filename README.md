# Template Background Maps

[![Generate Tiles](https://github.com/digidem/map-template/actions/workflows/gen-tiles.yml/badge.svg?branch=main)](https://github.com/digidem/map-template/actions/workflows/gen-tiles.yml)

This repository serves as a template for generating map tiles using GitHub Actions and [mapgl-tile-renderer](https://github.com/ConservationMetrics/mapgl-tile-renderer).

To use this template repository effectively, customize the `manifest.json` file according to your project's requirements and configure secrets if using an online source that requires an API key, such as Planet or Mapbox.

You can configure this template to copy the generated tiles to a remote cloud server. In the example `manifest.json`, the tiles are automatically uploaded to [Earth Defenders Toolkit Cloud](https://github.com/digidem/edt-cloud) for distribution to partners or for synchronization with [Kakawa devices](http://docs.earthdefenderstoolkit.com/).

## Using This Template

To create a new repository from this template, click on the "Use this template" button located at the top of the repository page. This will prompt you to enter the new repository details.

You can either clone the repository and modify locally, or edit files directly on the Github interface. On every change a test will run to check if everything is working fine. 

When ready to build the tiles, head to the `Actions` tab, select the `Generate Tile` workflow and click on the `Run workflow` button.

![run workflow](https://res.cloudinary.com/practicaldev/image/fetch/s--EF0YFTak--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tas45tcdz8v3ogomr2zk.png)

## Setting up Secrets

In your new repository, you will need to set up secrets to securely store sensitive information:

- `API_KEY`: The default API key for accessing the tile rendering service (e.g., Planet, Mapbox, Google). If you are using multiple services, you may add additional API keys as secrets with distinct names (e.g., `MAPBOX_API_KEY`, `GOOGLE_API_KEY`).
- `SSH_KEY`: The private SSH key that corresponds to the public key added to the server for secure file transfer.

To add these secrets, navigate to your repository settings, access the "Secrets" section, and click on "New repository secret". Add each secret with the appropriate name (`API_KEY`, `SSH_KEY`, etc.).

Additionally, in the `manifest.json` file, you can specify which API key to use for each tile set by setting the `apiKeySecret` option to the name of the secret that contains the desired API key. If `apiKeySecret` is not specified, it will default to using `API_KEY`.

![adding github action secrets](https://www.edwardthomson.com/blog/images/githubactions/11-addingsecret.png)

## Customizing `manifest.json`

The `manifest.json` file holds the configuration for the tile generation process. Modify the following variables to suit your project:

- `name` (required): The base name for the generated `.mbtiles` file.
- `style` (required): The map style to be used for tile generation. Specify 'self' for a self-provided style or use an online source such as 'mapbox', 'bing', 'planet', or 'esri'.
- `bounds` (required): The geographical bounds for tile generation, formatted as `"minLongitude,minLatitude,maxLongitude,maxLatitude"`. Use a tool like [boundingbox](https://boundingbox.klokantech.com/) to obtain the values in CSV format.
- `maxzoom` (required): The maximum zoom level at which the tiles will be generated. (e.g., `16`).
- `minzoom` (optional): The minimum zoom level for which the tiles will be generated. If not specified, the default minimum zoom level is used.
- `monthyear` (optional): The month and year for which the Planet tiles are generated, formatted as `YYYY-MM`. This is required if using 'planet' as the online source for tile generation.
- `mapbox_style` (optional): The Mapbox style in the format `<yourusername>/<styleid>`. This is required if using 'mapbox' as the online source for tile generation.
- `apiKeySecret` (optional): The name of the secret where the API key is stored. Defaults to `API_KEY` if not specified.

## Workflow Overview

The GitHub Actions workflow, defined in `.github/workflows/gen-tiles.yml`, executes the following steps:

1. Checks out the code from the repository.
2. Reads the `manifest.json` file and prepares the necessary command with the specified variables.
3. Executes a mapgl-tile-renderer Docker container to produce the tiles according to the `manifest.json` configuration.
4. Either uploads the generated `.mbtiles` file as an artifact or transfers it to a remote server via SSH, as configured in the workflow.

Review and adjust the workflow file to meet the specific needs of your project.
