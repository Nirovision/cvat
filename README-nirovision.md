# Cvat
This is a fork of the official cvat repo. We needed some small modifications
for the cli to with our deployment.

## Build cli
To build the cli:
```bash
mkdir -p cvat-sdk/schema
docker-compose -f docker-compose.yml -f docker-compose.dev.yml run \
    --no-deps --entrypoint '/usr/bin/env python' --rm -u "$(id -u)":"$(id -g)" -v "$PWD":"/local" \
    cvat_server \
    manage.py spectacular --file /local/cvat-sdk/schema/schema.yml

cd cvat-sdk
python -m pip install -r ./gen/requirements.txt
./gen/generate.sh
```

## Development run cli
```bash
# from the repo root
export PYTHONPATH="$(pwd)/cvat-cli/src:$(pwd)/cvat-sdk"

python cvat-cli/src/cvat_cli/__main__.py --server-host https://cvat.ml.nirovision.com --server-port 443 --auth username:password ls
```

## Publish
First update package name and version in cvat-sdk and in cvat-cli.
Then the following to build and publish
```bash
cd cvat-sdk

python setup.py sdist
# nb: install twine if you don't have it
twine upload -u "$NEXUS_USERNAME" -p "$NEXUS_PASSWORD" --repository-url "https://nexus.nirovision.com/repository/python-releases/" dist/nirovision-cvat-sdk-xxx

cd ../cvat-cli
python setup.py sdist
twine upload -u "$NEXUS_USERNAME" -p "$NEXUS_PASSWORD" --repository-url "https://nexus.nirovision.com/repository/python-releases/" dist/nirovision-cvat-cli-xxx
```
