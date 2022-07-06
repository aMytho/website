---
section: self-hosted/latest
title: Local Preview of Gitpod Self-Hosted
---

<script context="module"> 
  export const prerender = true;
</script>

# Local Preview of Gitpod Self-Hosted

> **Note:** Currently, this Local Preview of Gitpod Self-Hosted is in [beta](../../references/gitpod-releases). It does not work on M1 Mac. See [the relevant issue](https://github.com/gitpod-io/gitpod/issues/9075) for more information.

The Local Preview of Gitpod Self-Hosted is the easiest way to try out Gitpod locally without having to build a production setup (i.e. spin up a Kubernetes cluster). This is aimed at users who want to try Self-Hosted Gitpod locally with minimal effort and resource requirements. This is **not intended for production** usage. Please refer to the [Getting Started](./getting-started) page for instructions on how to install Gitpod for production usage.

## Running the docker container

First, Create a volume to which the gitpod container will attach to.

```bash
docker volume create gitpod
```

This install method runs a `k3s` cluster inside a docker container. self-signed certificates are automatically created and a self-signed Gitpod instance will be installed into the `k3s` cluster.
The Gitpod instance can be accessed through your local private IP address which can be retrieved by running either `ifconfig` (on Mac and Linux) or `ipconfig` (on Windows).
As Gitpod needs this information in a domain format (and not just a IP address), For this to work an `nip` URL can be created by replacing the `.` with `-`, and suffixing it with `nip.io`. For example, `10.0.0.7` private IP address would become `10-0-0-7.nip.io`. This is then passed to the container through the `DOMAIN` environment variable.

Run the following command to get the `preview-install` docker container up and running:

```bash
docker run -p 443:443 -e DOMAIN=10-0-0-7.nip.io --privileged --name gitpod --rm -it -v gitpod:/var/gitpod eu.gcr.io/gitpod-core-dev/build/preview-install
```

Unpacking the above command:

- `-p 443:443` to map the `443` container port to host.
- `-e DOMAIN=10-0-0-7.nip.io` to pass the DOMAIN to the Gitpod installation. [nip.io](https://nip.io/) is just wildcard DNS for local addresses, so all off this is local, and cannot be accessed over the internet.
- `--privileged` to be able to run docker (and hence `k3s`) inside the container. This is necessary.
- `--name gitpod` to set the name of the docker container for further access.
- `gitpod:/var/gitpod` to store the workspace files, etc into the `gitpod` volume.

## Accessing Gitpod

As this is a self-signed instance of Gitpod, the gitpod root CA cert has to be imported into your browser manually to access the full functionality of Gitpod. The certificate can be retrieved by running

```bash
docker cp gitpod:/var/gitpod/gitpod-ca.crt $HOME/gitpod-ca.crt
```

This certificate at `$HOME/gitpod-ca.crt` can then be loaded into your browser. Most browsers also require a restart before they can start to use the imported certificate.

Once the certificate is loaded, The URL to access the Gitpod instance is the same `$DOMAIN` env variable that was passed in the run command.

Open your Gitpod URL in your browser to access your running Gitpod instance. The first screen you see will ask you to configure a git integration. This git integration will also serve as the way that you and your users authenticated against Gitpod. You can find out more in the [integrations](../../integrations) section.

> **Important:** Public (SaaS) Source Control Management Systems (SCMs) (i.e. [GitLab.com](http://Gitlab.com), [GitHub.com](http://github.com/) and [Bitbucket.org](http://Bitbucket.org)) are **not** integrated by default with a Self-Hosted Gitpod instance because OAuth apps are tied to domains. Therefore, these public SCMs need to be integrated manually with an OAuth application you specifically create for your domain. This is done similarly to how it is done for the private/self-hosted versions of each SCM. As such their respective guides also apply here:<br \> - Follow [these](../../gitlab-integration#registering-a-self-hosted-gitlab-installation) steps to integrate [`GitLab.com`](https://gitlab.com/) with your self-hosted Gitpod instance. You will need to enter `gitlab.com` as the `Provider Host Name` in the New Git Integration Modal.<br \>- Follow [these](../../github-enterprise-integration) steps to integrate [`GitHub.com`](http://github.com) with your self-hosted Gitpod instance. You will need to enter `github.com` as the `Provider Host Name` in the New Git Integration Modal. <br \>- Follow [these](../../bitbucket-server-integration) steps to integrate [`Bitbucket.org`](https://bitbucket.org/) with your self-hosted Gitpod instance. Select `Bitbucket` as the `Provider Type` in the New Git Integration Modal. For bitbucket.org this requires configuring an "OAuth consumer" on a "workspace". This is slightly different from the documented Bitbucket Server integration. See [gitpod PR #9894](https://github.com/gitpod-io/gitpod/pull/9894#pullrequestreview-969013833) for an example.

> **Note:** Your first workspace start can take a bit of time because the workspace image first needs to be built and then downloaded. Subsequent workspace starts should be much quicker.
