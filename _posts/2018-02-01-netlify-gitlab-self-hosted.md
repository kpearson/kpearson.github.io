---
title: Netlify auto deploy with Gitlab
---

Let's deploy a static site like [Jekyll.rb] site using [Netlify] and a self
hosted instance of [Gitlab].

## Goals

- Set up Netlify to manage the build and deploy of a static site
- Trigger the build process baised given actions on the remote repository
- Receive notifications from Netlify about the success of failure of the deploy

## Netlify Setup

First lets push up our newly minted site to Gitlab. With that we have a remote
web address for the project and git knows what it is. We wont need to see it
again but it's important that it's setup using _ssh_.

Next install the Netlify CLI.

```
brew install netlify-cli
```

With that we can tell Netlify how to create and configure our site. Run the 
command `netlify init` and answer the questions.

```
? Directory to deploy (blank for current dir):

```

Leaving this blank just means you need to be in the project Directory to run
`netlify` commands on it.

```
? Your build command (middleman build/grunt build/etc):
```

Enter the command needed to build your site. In my case `jekyll build`.

## Connect Netlify to Gitlab

Next we need to give Netlify access to the remote git repo. If your using
Github Github, or Bitbucket the setup is straight forwared, enter you user name
and password. If your like me and going the self-hosted route the cli will
given spit out a _ssh public key_ which you have to add to you remote host in
my case a self-hosted Gitlab. Go to _Settings > Repository > Deploy Keys_ give
the key a name (it doesent matter what it is) and add the _ssh public key_ in
the key field.

## Receiving The Build Signal

Netlify needs to told how to listen for the incomming signal to build and
deploy. On the Netlify setting page for your site, scroll down to _Build hooks_
and click "Add build hook". Again the name doesn't matter, but chouse something
discriptive and select the branch for that particular build. The master branch
is an obvious choice but you may also want one for a staging branch which it
not viewable by the world. The created build hook will generage a url which you
will need.

## Sending The Build Signal

Now lets tell Gitlab to send a signal to that url when a given action takes
place on the repo. Back in Gitlab _Settings > Integrations_ add the url to the
url field and choose the "Trigger" to tell Netlify to build and publish. Click
"Add webhook" and in the newly created web hook use the test drop down click
your event. Over in the Netlify go to the "Deploys" tab and you should see your
deploy trigger.

Next time you push to the branch you set the trigger, your site will be
automatical updated on the web.

