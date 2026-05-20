# Environment Setup

## How is it done?

`Pixi` is used to manage all practical environments containerzied in `Docker` images. There is a single `env` directory in the root of the project, which has a single `Pixi` setup (`pixi.toml` & `pixi.lock`) with a single `Dockerfile`. This single set of file has definitions for all environments, and while spinning Docker containers passing a variable to `Docker` will spin specific pratical container. 

## Example

Taking an example of `Practical 0`,

The Pixi `toml` file:

```toml
[workspace]
authors = ["Aditya Singh <dr.singhaditya@hotmail.com>"]
channels = ["conda-forge", "bioconda"]
name = "env"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
ipykernel = ">=7.2.0,<8"
jupyter = ">=1.1.1,<2"
curl = ">=8.20.0,<9"
tar = ">=1.35,<2"
gzip = ">=1.14,<2"

[feature.prac-0.dependencies]
seaborn = "*"
matplotlib = "*"
pandas = "*"
numpy = "*"
anndata = "*"
scipy = "*"
spatialdata = "*"
scanpy = "*"

[feature.prac-0.pypi-dependencies]
abc-atlas-access = { git = "https://github.com/alleninstitute/abc_atlas_access.git", extras = ["notebook"] }

[environments]
prac-0 = ["prac-0"]
```

### Brief explanation of Pixi features

In the example Pixi configuration shown above, the `feature.*` tables are used to group optional, named sets of dependencies that can be enabled when building a specific environment or container.

Key points:

- `feature.prac-0.dependencies`: lists conda/conda-forge packages that belong to the "prac-0" feature (seaborn, matplotlib, pandas, numpy, anndata, scipy, spatialdata, scanpy).
- `feature.prac-0.pypi-dependencies`: lists pip/pyproject-style installs for the same feature; here it pulls a Git repository for `abc-atlas-access` with the `notebook` extras.
- The top-level `[environments]` section maps environment names to feature sets (e.g. `prac-0 = ["prac-0"]`), so the build for the `prac-0` environment will include the packages defined under that feature.
- `dependencies` at the top level are common dependencies that will be included in all environments, while features allow you to add specific dependencies only for certain environments without cluttering the main dependency list. This includes `Jupyter` and `ipykernel` which are needed for all environments, so you don't need to worry about enabling it.
- For cases where `R` is needed, one can skip the default dependencies and use `Rstudio` instead

Using features keeps the main dependency list small and lets you compose multiple optional sets for different practicals or use-cases without duplicating entries.


# Spinning the container

`Dockerfile` is programmed to receive a system argument `PRAC` followed by the practical number, this should match the feature name in `Pixi` configuration. For example, to spin a container for `Practical 0`, the command would be:

```bash
docker build --build-arg PRAC=0 --platform linux/amd64 -t saditya88/elixir-prac-0:0.1.3
```

## Adding environments

To add a new environment, you need to:

1. Add a new feature in the `Pixi` configuration with the required dependencies.
2. Map the new feature to an environment name in the `[environments]` section.
3. Build the Docker image for the new environment by passing the appropriate `PRAC` argument during the build process.

Example, if you wish to build `Practical 1` with `scanpy` and `squidpy` as dependencies, you would first add a new feature in the `Pixi` configuration:


```bash
pixi add --no-install --feature prac-1 scanpy squidpy
```

This will add a new feature named `prac-1` with the specified dependencies to the `pixi.toml` file. You can also add any additional dependencies or pip packages as needed.

Then, map this feature to an environment name in the `[environments]` section:

```toml
[environments]
prac-0 = ["prac-0"]
prac-1 = ["prac-1"]
```

Finally, build the Docker image for `Practical 1`:

```bash
docker build --build-arg PRAC=1 --platform linux/amd64 -t saditya88/elixir-prac-1:0.1.0
```
