# Niro modifications v2.4.9

I have had to make a few small fixes to make things work.

## SDK
Fixing setting the cvat server url on file upload.
To publish a new version:

```shell
cd cvat-sdk
python -m pip install -r gen/requirements.txt
./gen/generate.sh

# modify package name in setup.py to nirovision-cvat-sdk
python setup.py bdist_wheel

twine upload \
    -u "$NEXUS_USERNAME" \
    -p "$NEXUS_PASSWORD" \
    --repository-url "https://nexus.nirovision.com/repository/python-releases/" \
    dist/*
```

## Server
I ran into some small bug with the server. We can build and use our own version, I did it like this:
```shell
# build the image
docker compose -f docker-compose.dev.yml -f docker-compose.yml build
# tag for our repo
docker tag cvat/server:v2.4.9 445002481121.dkr.ecr.ap-southeast-2.amazonaws.com/cortex:cvat-v2.4.9-debug
# push
docker push 445002481121.dkr.ecr.ap-southeast-2.amazonaws.com/cortex:cvat-v2.4.9-debug
# change the image used by the cvat deployment to be our image
```
