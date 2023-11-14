# Source

## Building the scripts and containers

* [Poetry](https://python-poetry.org/docs/master/)
    * Once installed, run `poetry install --sync` inside the main directory. This will install the other requirements.

### Deploy scripts

Building the deployment scripts is easy, simply run `build_deploy.sh` in the main directory.

### Containers

The containers have dedicated GitHub Actions workflows to build them, so in general you should never have to build them locally. Take a look at the workflows to see how they are built.