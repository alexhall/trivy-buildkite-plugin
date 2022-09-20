# trivy buildkite plugin

The trivy buildkite plugin provides a convenient mechanism for running the
open-source trivy static analysis tool on your project. For more information
about trivy, please refer to their
[documentation](https://aquasecurity.github.io/trivy/latest/docs/).

## Features

- Automatically downloads and verifies the trivy executable if it cannot be
  found in the `PATH` environment variable's directories
- Executes a `filesystem` scan on the git repo cloned by buildkite. Refer to the
  [filesystem scan documentation](https://aquasecurity.github.io/trivy/latest/docs/vulnerability/scanning/filesystem/)
  for more information
- Executes an `image` scan against an existing Docker image ref. Refer to the
  [image scan documentation](https://aquasecurity.github.io/trivy/latest/docs/vulnerability/scanning/image/)
  for more information

## Basic example

The following code snippet demonstrates how to use the plugin in a pipeline
step with the default plugin configuration parameters:

```yml
steps:
  - command: ls
    plugins:
      - equinixmetal-buildkite/trivy#v1.15.0: ~
```

## Additional examples

Specify the `--exit-code` option as a plugin parameter in `pipeline.yml` to fail the pipeline when there are vulnerabilities:

```yml
steps:
  - command: ls
    plugins:
      - equinixmetal-buildkite/trivy#v1.15.0:
        exit-code: 1
```

Specify the `--severity` option as a plugin parameter in `pipeline.yml` to scan specific type of vulnerabilities. Below is an example for scanning `CRITICAL` vulnerabilities:

```yml
steps:
  - command: ls
    plugins:
      - equinixmetal-buildkite/trivy#v1.15.0:
        severity: "CRITICAL"
```

## Configuration

### `exit-code` (Optional, array)

Controls whether the security scan is blocking or not. This is done by setting the exit code of the plugin. If the exit code is set to 0, the pipeline will continue. If the exit code is set to 1, the pipeline will fail. (Defaults to 0)

### `severity` (Optional, string)

Controls the severity of the vulnerabilities to be scanned. (Defaults to "UNKNOWN,LOW,MEDIUM,HIGH,CRITICAL")

### `ignore-unfixed` (Optional, boolean)

Controls whether to display only fixed vulnerabilities. (Defaults to false)

### `security-checks` (Optional, string)

Controls the security checks to be performed. (Defaults to "vuln,config")

### `image-ref` (Optional, string)

**Important**: Please ensure the target Docker image is built prior to the trivy plugin running when using this option. The trivy plugin does not build Docker images; it only scans existing images.

Controls the image reference to be scanned. If no image is specified, the image scanning step is skipped. This is also able to infer the image from the [`docker-metadata` plugin](https://github.com/equinixmetal-buildkite/docker-metadata-buidkite-plugin). (Defaults to "")

### `trivy-version` (Optional, string)

Controls the version of trivy to be used.

## Developing

To run the tests:

```shell
make test
```
