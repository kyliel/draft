# Draft Starter Packs

`draft create` has an option flag called `--pack` that allows a user to bootstrap an application
from a defined starter pack in their local filesystem. Starter packs consist of a Dockerfile and
a chart that demonstrates best practices for an application of a given language pack. For example:

```
$ draft create --pack=rails
--> Created chart/
--> Created Dockerfile
--> Ready to sail
```

This document explains the starter pack format and provides basic guidance on creating your own
starter packs.

## The Starter Pack Structure

A starter pack is organized inside a directory in `$(draft home)/packs`. Inside the pack's
directory, there will be a template `chart/` and a `Dockerfile` that will be injected into the
application when the starter pack is requested.

Inside this directory, Draft will expect a structure like this:

```
python/               # the name of the directory is the name of the starter pack
  chart/
    Chart.yaml        # A YAML file containing information about the chart
    LICENSE           # OPTIONAL: A plain text file containing the license for the chart
    README.md         # OPTIONAL: A human-readable README file
    values.yaml       # The default configuration values for this chart
    charts/           # OPTIONAL: A directory containing any charts upon which this chart depends.
    templates/        # OPTIONAL: A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  Dockerfile          # A Dockerfile for building the application
```

We could then run `draft create` with this pack like so:

```
$ draft create --pack=python
--> Created chart/
--> Created Dockerfile
--> Ready to sail
```

The easiest way to create and work with a starter pack is with the following commands:

```
$ cd $(draft home)/packs
$ mkdir python
$ cd python
$ helm create chart
Creating chart
$ echo "FROM python:onbuild" > Dockerfile
```

See [Helm's documentation on Charts][charts] for more information on the Chart file structure.

## Pack Detection

When `draft create` is executed on an application, Draft performs a deep search on the current
directory to determine the language, then starts iterating through the packs available in
`$(draft home)/packs`. If it finds a pack that matches the language description, it will then use
that pack to bootstrap the application.

Draft's smart pack detection can be overridden with the `--pack` flag. The detection logic will not
be run and Draft will bootstrap the app with the specified pack, no questions asked.


[charts]: https://github.com/kubernetes/helm/blob/master/docs/charts.md
