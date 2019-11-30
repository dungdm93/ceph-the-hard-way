Prerequisites
=============

## 1. Google Cloud Platform

This tutorial leverages the [Google Cloud Platform](https://cloud.google.com/) to streamline provisioning of the compute infrastructure required to bootstrap a Ceph cluster from the ground up. [Sign up](https://cloud.google.com/free/) for $300 in free credits.

> The compute resources required for this tutorial exceed the Google Cloud Platform free tier.

## 2. Google Cloud Platform SDK

### 2.1 Install the Google Cloud SDK

Follow the Google Cloud SDK [documentation](https://cloud.google.com/sdk/) to install and configure the `gcloud` command line utility.

Verify the Google Cloud SDK version is 262.0.0 or higher:

```bash
gcloud version
```

### 2.2 Initialize & Login to Google Cloud

This tutorial assumes a default compute region and zone have been configured.

If you are using the `gcloud` command-line tool for the first time `init` is the easiest way to do this:

```bash
gcloud init
```

Then be sure to authorize gcloud to access the Cloud Platform with your Google user credentials:

```bash
gcloud auth login
```

### 2.3 Set default Compute Region and Zone
Use the `gcloud compute zones list` command to view additional regions and zones.

Next set a default compute region and compute zone:

```bash
gcloud config set compute/region [REGION]
```

Set a default compute zone:

```bash
gcloud config set compute/zone [ZONE]
```
