+++
date = "2016-09-09T14:19:15-07:00"
description = "Using Travis CI to auto-deploy your hugo site"
tags = []
title = "Hugo to Rackspace using Travis"
topics = []

+++


After reading Evan Browns post [Hugo on the go](http://evanbrown.io/post/hugo-on-the-go/) I was inspired to figure out how to do this exact same thing but as opposed to using AWS I wanted to use [Rackspace Cloud FIles](http://www.rackspace.com/cloud/files) and more specfically Rackspace Static Website feature, because Rackspace Cloud Files is an inexpensive way to store and distribute objects, the Cloud Files static website feature coupled with the lightning-fast content delivery network (CDN) is a winning combination in terms of both performance and price.

Creating a static website—a site that does not change for each request—with Cloud Files and the updated Control Panel is as easy as pointing and clicking.

#### To set up a static webiste using the Rackspace Control Panel ####

In the Control Panel, you begin to set up your static website by creating a container.

1. Login to the [Control Panel](https://mycloud.rackspace.com)
2. In the Files section of the Control Panel, click **Create Container**
3. Fill in a name and a region for your static webite.
4. For **Type**, select **Static Website**.
5. Click **Create Container**
6.  In the Control Panel, click on your static website container, and then click the **Upload Files** button.
7. In the File Upload dialog box, click **Choose Files** and navigate to the folder with your content.
8. Select your content (you can select multiple files), and then click **Open**.
9. In the Control Panel, navigate to your container listing by clicking the **Containers** link.
10. Click the cog wheel next to your static website container, and then click **View All Links**.

All the CDN URLs for your container are displayed. For HTML pages and pictures, use the HTTP link for your static website container.

#### To set up a Automated Deployments using Travis CI ####

Once again my *.travis.yml* file was very similar to Evan Browns except im using a few pieces to talk to rackspace's cloud - 

```bash
language: go                                                                                                                                  
go:
- 1.4
install:
- go get github.com/spf13/hugo
script: hugo
before_deploy: cd public
deploy:
  provider: cloudfiles
  username: RACKSPACE USERNAME
  api_key:
    secure: APIKEYFROMRACKSPACE
  region: REGION
  container: CONTAINERNAME
  skip_cleanup: true
  on:
    repo: caseyCaseybecking/www_caseybecking_com_hugo
```

1. Login to the Control Panel and goto **Account Settings**
2. Under **Login Details** you will see **API KEY** - Copy That
3. Install the [Travis CLI](https://github.com/travis-ci/travis.rb)
4. CD into your current hugo working directory
5. Execute ```travis setup cloudfiles```
	_This will ask you a few questions for setup and create the *.travis.yml*_ 

Once this file is created you can push it to your repo (modification may be needed).

Now withing Travis you should be able to view the repo within _My Repositories_ flip the switch so it is enabled and it should try to build.