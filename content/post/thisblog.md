---
title: "Let's Talk About This Blog"
date: 2018-02-15T17:19:05+07:00
author: Felicia
tags: ["Felicia", "Blog", "Sprint 0"]
---


As per the requirement of this course, this blog is made to document the journey of Aboena. Plenty of blogging platform out there, all are so mainstream. We like complicated, cutting edge and hipster technology so we refused to use them all. Actually, Fersandi did insists to use medium blog in order to reduce headache. But what kind of life is that XD.

So here we are, a non mainstream blog based on Hugo, a golang static website generator deployed through Codeship CD for every push to this blog's git master branch on surge.sh. Which, by the way is quite a big feat of creating this. Since I'm the only one insisting on using edgy technology, the setup of this blog is mainly done by me.

## Behind the hood
Basically all you need is [github](github.com), [hugo](gohugo.io), [codeship](codeship.com), and [surge](surge.sh)

Codeship is used because of it's simplicity and I do not have to set up the webhook to github. The limitation is the free account is only limited to 100 builds per month. Beyond that, we just have to deploy it manually, I guess.
The brief steps to create this blog is:

1. Install Go, Hugo, Git and Surge

2. Create Github Repository for blog

3. Initialize Hugo in the folder

4. Setup Surge in the folder

```yaml
# Install stable node.js
nvm install stable
nvm use stable
npm install --save-dev surge
# Install a custom Go version, https://golang.org/
#
# Add at least the following environment variables to your project configuration
# (otherwise the defaults below will be used).
# * GO_VERSION
#
# Include in your builds via
# source /dev/stdin <<< "$(curl -sSL https://raw.githubusercontent.com/codeship/scripts/master/languages/go.sh)"
GO_VERSION=${GO_VERSION:="1.8.3"}
# strip all components from PATH which point toa GO installation and configure the
# download location
CLEANED_PATH=$(echo $PATH | sed -r 's|/(usr/local\|tmp)/go(/([0-9]\.)+[0-9])?/bin:||g')
CACHED_DOWNLOAD="${HOME}/cache/go${GO_VERSION}.linux-amd64.tar.gz"
# configure the new GOROOT and PATH
export GOROOT="/tmp/go/${GO_VERSION}"
export PATH="${GOROOT}/bin:${CLEANED_PATH}"
# no set -e because this file is sourced and with the option set a failing command
# would cause an infrastructure error message on Codeship.
mkdir -p "${GOROOT}"
wget --continue --output-document "${CACHED_DOWNLOAD}" "https://storage.googleapis.com/golang/go${GO_VERSION}.linux-amd64.tar.gz"
tar -xaf "${CACHED_DOWNLOAD}" --strip-components=1 --directory "${GOROOT}"
# check the correct version is used
go version | grep ${GO_VERSION}
```
**this post is to be continued**
