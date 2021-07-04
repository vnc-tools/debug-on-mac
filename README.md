# debug-on-mac

> Get a MacOS desktop over VNC, for free, in around 5 minutes

Shell that configure a MacOS VNC server that can run in a Github Actions VM and can be accessed through a tcp tunnel. 

**NOTE:** This repo is based in [fastmac](https://github.com/fastai/fastmac) and [fastmac-VNCgui](https://github.com/dakotaKat/fastmac-VNCgui).

---

## What is this?
 
This repo includes a shell and a pipeline for Github Actions that
  - enables the built in MacOS VNC server, doing a hacky trick to set a VNC password and a new admin user 
  - installs `ngrok` and uses it to set up a tcp tunnel for VNC/Apple Screen Sharing.
  - runs tmate to setup a Web Shell and a SSH connection

---

## How to use it?

You must clone this repo, configure some secrets in the cloned repo and execute one of the Github Actions pipelines.

---

## Setup

### Clone Template

This repository has been configured as a template. You can create a new repository using [this link](https://github.com/vnc-tools/debug-on-mac/generate)
  - Type `debug-on-mac` under "repository name"
  - and then click "Create repository from template".

### Configuration

To get a running MacOS desktop, you must configure three secrets for the Github Actions:

  - `NGROK_AUTH_TOKEN` with your auth key from https://dashboard.ngrok.com/auth
  - `VNC_USER_PASSWORD` with the desired password for the "VNC User" (`vncuser`) account
  - `VNC_PASSWORD` for the VNC-only password

---

## MacOS Desktop

### Starting a MacOS Desktop

After configuring the secrets, you can start a MacOS desktop by running the "macos-with-vnc" Github Actions
  - Go to the <a href="../../actions?query=workflow%3Amacos-with-vnc">macos-with-vnc</a> pipeline
  - Click the "Run workflow" dropdown on the right, and 
  - Click the green "Run workflow" button that appears.

<img width="365" src="https://user-images.githubusercontent.com/346999/92965396-91320680-f42a-11ea-9bc3-90682e740343.png" />


### Accessing the MacOS Desktop

Once the pipeline is started and you're in the status, you can view the 'you can VNC to...' section in the workflow log for your ngrok tunnel VNC address.

If you are using **Apple Screen Sharing** or **RealVNC Viewer**
  - use the ngrok tunnel VNC address as the VNC ip address
  - use the system username and password ("VNC User"/your set password)
  
If you are using any other VNC client
  - use the ngrok tunnel VNC address as the VNC ip address
  - use the VNC-only password, configured above as a secret.

**NOTE:** If you start a MacOS desktop, you can access the desktop using a SSH shell. You may follow the steps described bellow to connect to the MacOs Shell.

---

## MacOS Shell

### Starting a MacOS Shell

After configuring the secrets, you can start a MacOS shell by running the "macos-with-ssh" Github Actions
  - Go to the <a href="../../actions?query=workflow%3Amacos-with-ssh">macos-with-ssh</a> pipeline
  - Click the "Run workflow" dropdown on the right, and 
  - Click the green "Run workflow" button that appears.

<img width="365" src="https://user-images.githubusercontent.com/346999/92965396-91320680-f42a-11ea-9bc3-90682e740343.png" />

### Connecting to the MacOS Shell

Once the pipeline is started and you're in the status, you can view the "Setup tmate session" section. There, once the tmate software is installed, it will repeatedly print lines like this:

```
WebURL: https://tmate.io/t/rXbusP3qkYsfALDSLMQZVwG3d

SSH: ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io
```

Copy and paste the ssh line (e.g `ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io` in this case) into your terminal (Windows users: I strongly recommend you use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) if possible) and press <kbd>Enter</kbd>.

---

## Stop the MacOS session

Your session will run for up to six hours. When you're finished, you should close it, since otherwise you're taking up a whole computer that someone else could otherwise be using!

To close the session, click the red "Cancel workflow" on the right-hand side of the Actions screen (the one you copied the `ssh` line from).



---

## Considerations

### Github Actions Terms of Service

Please check the [GitHub Actions Terms of Service](https://docs.github.com/en/github/site-policy/github-additional-product-terms#5-actions-and-packages). Note that your repo needs to be public, otherwise you have a strict monthly limit on how many minutes you can use. Note also that according to the TOS the repo that contains these files needs to be the same one where you're developing the project that you're using it for, and specifically that you are using it for the "*production, testing, deployment, or publication of [that] software project*".

### tmate expose a shareable shell

This pipeline is a thin wrapper around [tmate](https://tmate.io/), so all the features of tmate are available. tmate itself is based on [tmux](https://github.com/tmux/tmux/wiki), so you have all that functionality too. In practice, that means other people can connect to the same ssh session, and you'll all be sharing the same screen! This can be very handy for debugging and support. The integration with Github Actions is provided by [action-tmate](https://github.com/mxschmitt/action-tmate).
