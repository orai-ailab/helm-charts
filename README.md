# Public Helm Charts to Manage GPU Job

This repository contains Helm charts designed to help you manage GPU jobs efficiently on Kubernetes. These charts provide the necessary configurations and resources to deploy and run GPU-intensive applications seamlessly.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [MIG GPU Job Template](#mig-gpu-job-template)
- [GPU Instance Profiles](#gpu-instance-profiles)
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

## MIG GPU Job Template

If you are using Multi-Instance GPU (MIG) on NVIDIA GPUs, here is a template to deploy a GPU job with MIG:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: mig-gpu-job
spec:
  template:
    spec:
      containers:
      - name: mig-gpu-container
        image: nvidia/cuda:11.2.1-base
        resources:
          limits:
            nvidia.com/mig-1g.5gb: 1  # Request a specific MIG partition
        command: ["nvidia-smi"]
      restartPolicy: OnFailure
      tolerations:
      - key: "nvidia.com/mig"
        operator: "Equal"
        value: "true"
        effect: "NoSchedule"
      nodeSelector:
        nvidia.com/mig.config: "all"  # Ensure the job is scheduled on nodes with MIG configuration
```

Apply this template to your Kubernetes cluster using:

```sh
kubectl apply -f mig-gpu-job.yaml
```

## GPU Instance Profiles

```
+-----------------------------------------------------------------------------+
| GPU instance profiles:                                                      |
| GPU   Name             ID    Instances   Memory     P2P    SM    DEC   ENC  |
|                              Free/Total   GiB              CE    JPEG  OFA  |
|=============================================================================|
|   0  MIG 1g.5gb        19     0/7        4.75       No     14     0     0   |
|                                                             1     0     0   |
+-----------------------------------------------------------------------------+
|   0  MIG 1g.5gb+me     20     0/1        4.75       No     14     1     0   |
|                                                             1     1     1   |
+-----------------------------------------------------------------------------+
|   0  MIG 1g.10gb       15     0/4        9.62       No     14     1     0   |
|                                                             1     0     0   |
+-----------------------------------------------------------------------------+
|   0  MIG 2g.10gb       14     0/3        9.62       No     28     1     0   |
|                                                             2     0     0   |
+-----------------------------------------------------------------------------+
|   0  MIG 3g.20gb        9     0/2        19.50      No     42     2     0   |
|                                                             3     0     0   |
+-----------------------------------------------------------------------------+
|   0  MIG 4g.20gb        5     0/1        19.50      No     56     2     0   |
|                                                             4     0     0   |
+-----------------------------------------------------------------------------+
|   0  MIG 7g.40gb        0     0/1        39.25      No     98     5     0   |
|                                                             7     1     1   |
+-----------------------------------------------------------------------------+
|   1  MIG 1g.5gb        19     0/7        4.75       No     14     0     0   |
|                                                             1     0     0   |
+-----------------------------------------------------------------------------+
|   1  MIG 1g.5gb+me     20     0/1        4.75       No     14     1     0   |
|                                                             1     1     1   |
+-----------------------------------------------------------------------------+
|   1  MIG 1g.10gb       15     0/4        9.62       No     14     1     0   |
|                                                             1     0     0   |
+-----------------------------------------------------------------------------+
|   1  MIG 2g.10gb       14     0/3        9.62       No     28     1     0   |
|                                                             2     0     0   |
+-----------------------------------------------------------------------------+
|   1  MIG 3g.20gb        9     0/2        19.50      No     42     2     0   |
|                                                             3     0     0   |
+-----------------------------------------------------------------------------+
|   1  MIG 4g.20gb        5     0/1        19.50      No     56     2     0   |
|                                                             4     0     0   |
+-----------------------------------------------------------------------------+
|   1  MIG 7g.40gb        0     0/1        39.25      No     98     5     0   |
|                                                             7     1     1   |
+-----------------------------------------------------------------------------+
```

## Contributing

We welcome contributions to improve these Helm charts. If you have any suggestions, bug reports, or pull requests, please visit our [GitHub repository](https://github.com/example/gpu-job-charts).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
