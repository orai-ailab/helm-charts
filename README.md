# Public Helm Charts to Manage GPU Job

This repository contains Helm charts designed to help you manage GPU jobs efficiently on Kubernetes. These charts provide the necessary configurations and resources to deploy and run GPU-intensive applications seamlessly.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Managing GPU jobs on Kubernetes can be complex due to the specialized requirements of GPU resources. This repository aims to simplify that process by providing a set of Helm charts tailored for GPU job management. These charts include configurations for GPU scheduling, resource allocation, and monitoring.

## Prerequisites

Before you begin, ensure you have met the following requirements:

- Kubernetes cluster running version 1.14 or higher
- Helm 3 installed on your local machine
- NVIDIA GPU nodes available in your Kubernetes cluster
- NVIDIA device plugin installed on your cluster

## Installation

To install the Helm charts, follow these steps:

1. Add the Helm repository:

   ```sh
   helm repo add gpu-jobs https://example.com/helm-charts
   ```

2. Update your Helm repositories:

   ```sh
   helm repo update
   ```

3. Install the GPU job chart:

   ```sh
   helm install gpu-job gpu-jobs/gpu-job-chart
   ```

## Usage

After installing the chart, you can deploy GPU jobs by creating Kubernetes manifests that request GPU resources. Here is an example:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: gpu-job-example
spec:
  template:
    spec:
      containers:
      - name: gpu-job-container
        image: nvidia/cuda:10.1-base
        resources:
          limits:
            nvidia.com/gpu: 1 # Request 1 GPU
        command: ["nvidia-smi"]
      restartPolicy: OnFailure
```

Apply the manifest to your cluster:

```sh
kubectl apply -f gpu-job-example.yaml
```

## Configuration

The Helm chart comes with a set of configurable parameters. You can customize the chart according to your requirements by providing values through a `values.yaml` file or via the `--set` flag during installation.

Example `values.yaml`:

```yaml
replicaCount: 1
image:
  repository: nvidia/cuda
  tag: 10.1-base
  pullPolicy: IfNotPresent
resources:
  limits:
    nvidia.com/gpu: 1
nodeSelector:
  gpu: "true"
```

To install the chart with custom values:

```sh
helm install gpu-job gpu-jobs/gpu-job-chart -f values.yaml
```

## Contributing

We welcome contributions to improve these Helm charts. If you have any suggestions, bug reports, or pull requests, please visit our [GitHub repository](https://github.com/example/gpu-job-charts).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
