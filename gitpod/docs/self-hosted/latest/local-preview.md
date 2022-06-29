---
section: self-hosted/latest
title: Local Preview of Gitpod Self-Hosted
---

<script context="module"> 
  export const prerender = true;
</script>

# Local Preview of Gitpod Self-Hosted

> **Note:** Currently, this Local Preview of Gitpod Self-Hosted is in [alpha](../../references/gitpod-releases). It works only on Linux systems. Windows and Mac
> support is in the works. See [the relevant issue](https://github.com/gitpod-io/gitpod/issues/9075)
> for more information.

The Local Preview of Gitpod Self-Hosted is the easiest way to try out Gitpod locally without having to build a production setup (i.e. spin up a Kubernetes cluster). This is aimed at users who want to try Gitpod Self-Hosted locally with minimal effort and resource requirements. This is **not intended for production** usage. Please refer to the [Getting Started](./getting-started) page for instructions on how to install Gitpod for production usage.

## Running the Docker container

This install method runs a [K3s](https://k3s.io/) cluster inside a Docker container. Self-signed certificates are
automatically created and a Gitpod instance will be installed. The
created instance will be accessible to the user on one of the [docker bridge network](https://docs.docker.com/network/network-tutorial-standalone/#use-the-default-bridge-network)
IP addresses.

Run the following command to get the Docker container up and running:

```
docker run --privileged --name gitpod --rm \\
    -v /tmp/gitpod:/var/gitpod \\
    eu.gcr.io/gitpod-core-dev/build/preview-install
```

> **Note:** This command implicitly uses the `latest` tag of the Docker image. To make sure you are using the newest version, we recommend to use a dedicated [release tag](./releases) (`release-YYYY.MM.V`) instead.

Unpacking the above command:

- `--privileged` is need to run K3s inside the Docker container. This is necessary.
- `--name gitpod` to set the name of the Docker container for further access.
- `--rm` to delete the Docker container after stopping.
- `/tmp/gitpod:/var/gitpod` to store the workspace files, root CA cert, etc. into `/tmp/gitpod` temporary directory.

## Accessing Gitpod

As this is a self-signed instance of Gitpod, the root CA cert has to be imported into your browser manually to be able to access Gitpod. This can be done by going into your browser settings and importing the certificate present at `/tmp/gitpod/gitpod-ca.crt`. Most browsers also require a restart before they can start to use the imported certificate.

Once the certificate is loaded, the URL to access the Gitpod instance can be retrieved by running:

```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' gitpod | sed -r 's/[.]+/-/g' | sed 's/$/.nip.io/g'
```

The URL will be in the form of `<ip-address>.nip.io`. [nip.io](https://nip.io/) is a wildcard DNS that resolves to a local IP address on your computer. Gitpod will not be available in your network.

Open your Gitpod URL in your browser to access your running Gitpod instance. The first screen you see will ask you to configure a Git integration, which will also serve as the way that you and your users authenticate against Gitpod. You can find out more in the [integrations](../../integrations) section.

> **Important:** Public (SaaS) Source Control Management Systems (SCMs) (i.e. [GitLab.com](http://Gitlab.com), [GitHub.com](http://github.com/) and [Bitbucket.org](http://Bitbucket.org)) are **not** integrated by default with a self-hosted Gitpod instance because OAuth apps are tied to domains. Therefore, these public SCMs need to be integrated manually with an OAuth application you specifically create for your domain. This is done similarly to how it is done for the private/self-hosted versions of each SCM. As such their respective guides also apply here:<br \> - Follow [these](../../gitlab-integration#registering-a-self-hosted-gitlab-installation) steps to integrate [`GitLab.com`](https://gitlab.com/) with your self-hosted Gitpod instance. You will need to enter `gitlab.com` as the `Provider Host Name` in the New Git Integration Modal.<br \>- Follow [these](../../github-enterprise-integration) steps to integrate [`GitHub.com`](http://github.com) with your self-hosted Gitpod instance. You will need to enter `github.com` as the `Provider Host Name` in the New Git Integration Modal. <br \>- Follow [these](../../bitbucket-server-integration) steps to integrate [`Bitbucket.org`](https://bitbucket.org/) with your self-hosted Gitpod instance. Select `Bitbucket` as the `Provider Type` in the New Git Integration Modal. For bitbucket.org this requires configuring an "OAuth consumer" on a "workspace". This is slightly different from the documented Bitbucket Server integration. See [gitpod PR #9894](https://github.com/gitpod-io/gitpod/pull/9894#pullrequestreview-969013833) for an example.

> **Note:** Your first workspace start can take a bit of time because the workspace image first needs to be built and then downloaded. Subsequent workspace starts should be much quicker.
