# pyspark-ubi
Minimalist install of pyspark on top of Red Hat UBI

This image is available at: `quay.io/erikerlandson/pyspark-ubi`

Various steps have been taken to keep this image minimal:
- built on Red Hat `ubi-minimal` base image
- headless java install
- removal of intermediate caching
- only `pyspark` has been installed to the pipenv environment.

The pyspark environment was created using pipenv,
to make it straightforward to include additional python package dependencies.
For example, starting from this base image and adding packages with docker RUN:
```
RUN cd /opt/pyspark \
 && pipenv install ...
```

An example of a script that could run this image against a file `spark-app.py`
```sh
cd /opt/pyspark
pipenv run python3 /your/path/to/spark-app.py
```

In the example above, the script or `spark-app.py` might be staged using S2I or made available via a volume mounted to a pod, etc.
