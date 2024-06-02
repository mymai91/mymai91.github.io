---
layout: post
title:  "[React - Sentry] React - Sentry Gatsby - beforeSend option to avoid crawler"
comments: true
date:  2023-12-02 15:07:33 +0700
categories: Sentry
---

# **React - Sentry Gatsby - beforeSend option to avoid crawler**

According to the Gatsby Sentry documentation https://github.com/WyriHaximus/sentry-docs/blob/cd4e1c2732e6b31319fecdb9f618da1a48f14442/src/platforms/javascript/guides/gatsby/index.mdx#L94

If you config `beforeSend` in the gatsby-config.js it won't support, so you have to refine it using `Sentry.init`

How to do

At `gatsby-config.js` file

```
const gatsbyConfig: GatsbyConfig = {
  pathPrefix: pathPrefix,
  siteMetadata: {
    title: ``,
    description: ``,
    author: `@mea`,
  },
  flags: {
    PRESERVE_WEBPACK_CACHE: false, //process.env.NODE_ENV !== "production",
    PRESERVE_FILE_DOWNLOAD_CACHE: false, //process.env.NODE_ENV !== "production",
  },
  plugins: [
    `gatsby-plugin-typescript`,
    {
      resolve: "@sentry/gatsby",

    },
   ]
}

```

At root folder create same level with `yarn.lock`

Create `sentry.c‎onfig.ts‎`

```
import { init } from "@sentry/gatsby"

  init({
    release: `${process.env.npm_package_version}`,
    dsn: sentryDns,
    environment: sentryEnvironment,
    beforeSend(event) {
      const userAgent = navigator.userAgent.toLowerCase()
      const blockedUserAgents = ["bytespider", "bytedance"]
      const isBlocked = blockedUserAgents.some(blockUserAgent =>
        userAgent.includes(blockUserAgent)
      )
      if (isBlocked) {
        return null
      }

      return event
    },
  })

  console.log("=====Enabled Sentry Config=====")
```
