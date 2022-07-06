---
author: iqqbot, mustard-mh, loujaybee
date: Wednesday, 6 July 2022 11:00:00 UTC
excerpt: Announcing SSH key upload support, and investment in Gitpod SSH support
image: teaser.png
slug: ssh-key-upload
subtitle: Announcing SSH key upload support, and investment in Gitpod SSH support
teaserImage: ../teaser.png
title: Gitpod does Desktop - SSH public key upload
---

<!-- TODO: Broaden title to include other SSH features? -->

SSH (secure shell) is a critical protocol for remote development. The market leaders of editors and IDEs: [JetBrains](https://www.jetbrains.com/) and [VS Code](https://code.visualstudio.com/) both use the SSH Protocol (See: [JetBrains Remote Development](https://www.jetbrains.com/help/idea/remote-development-a.html) and [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview), as the foundation for their remote development experiences.

<!-- Stack Overflow Survey -->

So naturally, a big focus at Gitpod is on our SSH connection experience for our various Gitpod desktop integrations. Today we're happy to announce that in Gitpod you can now upload your own public keys, to facilitate SSH access to Gitpod. We've also dropped the requirement for a mandatory public key to be available when using the SSH copy/paste command.

With SSH public key upload access you:

1. No longer need to navigate back to the dashboard to get an SSH when a workspace stops
2. Use the secure method of SSH public/private keys rather than using a token

### Getting Started

1. Navigate to the [keys](https://gitpod.io/keys) page in your Gitpod preferences
2. Upload a public SSH key (See: [SSH](https://www.gitpod.io/docs/configure/ssh) documentation)
3. Start a workspace
<!-- TODO: Just for running workspaces? -->
4. Copy the SSH command and connect from
   <!-- TODO: Copy the SSH command from workspace? -->
   <!-- TODO: VS Code Desktop -->

### Gitpod: Desktop & Browser

In the past, Gitpod has been well known for it's VS Code Browser experience, and if you've used Gitpod before, this should be quite obvious why that is.

Both [SaaS Gitpod](/pricing) users and [Self-Hosted](/self-hosted) customers who choose VS Code Browser as their default IDE or editor can have a fully functional Ubuntu Linux developer environment running in the cloud, configured with all the tools needed to run the project with a single click.

However, whilst VS Code Browser is a very capable editing experience for professional developers, the VS Code browser developer experience doesn't suit every developer preferences, or need.

**For example:**

1. You want to use a JetBrains IDE (and JetBrains is currently only available via desktop)
2. You want access to the in IDE or editor localhost port mapping tools available
3. You want to retain native keyboard shortcuts (e.g. `CMD + W` to close tab)

> **Note:** You don't need to use a desktop IDE to configure `localhost` for development. Gitpod [local companion](/docs/ides-and-editors/local-companion) will map all your workspace ports to the host machine). <br/> <br/> See the Gitpod [ports](/docs/config-ports) documentation for more on configuring Gitpod and `localhost`.

Gitpod is a tool for professional development teams, which means having the necessary flexibility in the different experiences and ways that developers can use Gitpod.

We announced a [partnership with JetBrains](https://3000-gitpodio-website-5k8f4cg8nd3.ws-eu51.gitpod.io/blog/gitpod-jetbrains) and expanded our JetBrains integrations powered by our custom [JetBrains Gateway](/docs/ides-and-editors/jetbrains-gateway) integration.

earlier this year to provide a seamless desktop experience, we built a new Gitpod component, called SSH Gateway. SSH Gateway acts as an intermediary server that validates SSH connections before forwarding connections to Gitpod workspaces.

### The evolution of SSH in Gitpod

We use SSH as the primary way to connect our desktop clients such as [JetBrains IDEs](https://www.gitpod.io/docs/ides-and-editors/jetbrains-gateway), [VS Code Desktop](https://www.gitpod.io/docs/ides-and-editors/vscode) and for [command-line access](https://www.gitpod.io/docs/ides-and-editors/command-line) to Gitpod.

Up until now, Gitpod was using a token-based approach to allow users to [directly SSH into their workspaces](https://3000-gitpodio-website-5k8f4cg8nd3.ws-eu51.gitpod.io/changelog/ssh-directly-into-workspaces), which we announced earlier this year. Despite this being

### Next steps for SSH in Gitpod

So, what are the future plans for SSH with Gitpod?

1. **Easier access of the SSH credentials** - We want to make it easier for you to access your SSH credentials from your workspace, either through direct IDE or editor integration, or via our gp CLI
2. **Integration with third-parties** - We're not the only tool to ask you for a private key upload, you might have also uploaded private keys into different places, such as in [GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) or

<!--
- Stability and speed of SSH connections (typing latency)
- Challenges because the owner token changes on each workspace start
-->
