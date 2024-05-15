---
layout: default
title: Troubleshooting
nav_order: 8
parent:  Custom Data Commons
---

## Troubleshooting

### Docker permission errors

#### Linux "permission denied"

If you see this error:

```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: ...
dial unix /var/run/docker.sock: connect: permission denied.
```
or this:

```
docker: Error response from daemon: pull access denied for datacommons-website-compose, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.
```

1. Use `sudo` with your `docker` invocations or set up a "sudoless" docker group, as described in [Linux post-installation steps for Docker Engine](https://docs.docker.com/engine/install/linux-postinstall/).
1. If you've just installed Docker, try rebooting the machine.


### Local build errors

#### "file not found in build context"

If you are building a local instance and get this error:

```
Step 7/62 : COPY mixer/go.mod mixer/go.sum ./  
COPY failed: file not found in build context or excluded by .dockerignore: stat mixer/go.mod: file does not exist
```

You need to download additional git modules. See [One-time setup: download build dependencies](build_repo.md#download_deps).

### Data loading problems

If you try to load data using the `/admin page`, and see the following errors:

`Error running import` or  `invalid input`

There is a problem with how you have set up your CSV files and/or config.json file. Check that your CSV files conform to the structure described in [Prepare the CSV files](custom_data.md#prepare-csv).

If the load page does not show any errors but data still does not load, try checking the following:

1. In the `_env.list` file you are using, check that you are not using single or double quotes around any of the values.
1. Check your Docker command line for invalid arguments. Often Docker won't give any error messages but failures will show up at runtime.

### Website display problems

If styles aren't rendering properly because CSS, logo files or JS files are not loading, check your Docker command line for invalid arguments. Often Docker won't give any error messages but failures will show up at runtime.

### Website form input problems

If you try to enter input into any of the explorer tools fields, and you get this:

![screenshot_troubleshoot](/assets/images/custom_dc/customdc_screenshot7.png){: width="800"}

This is because you are missing a valid API key or the necessary APIs are not enabled. Follow procedures in [Enable Google Cloud APIs and get a Maps API key](quickstart.md#maps-key), and be sure to obtain a permanent Maps/Places API key.

### Cloud Run Service problems

In general, whenever you encounter problems with any Google Cloud Run service, check the **Logs** page for your Cloud Run service, to get detailed output from the services. 

#### "502 Bad Gateway" 

If you see no errors in the logs, except `connect() failed (111: Connection refused) while connecting to upstream`, try the following:

1. Wait a few minutes and try to access the app again. Sometimes it appears to be deployed, but some of the services haven't yet started up.
1. In the Cloud Run **Service details** page, click the **Revisions** tab. Under the **Containers** tab, under **General**, check that **CPU Allocation** is set to **CPU is always allocated**. If it is not, click **Edit & Deploy New Revision**, and the **Containers** tab. Under **CPU allocation and pricing** enable **CPU is always allocated** and click **Deploy**. 