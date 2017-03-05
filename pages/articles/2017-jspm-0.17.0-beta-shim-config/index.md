---
title: "Shim Config with Jspm 0.17.0-beta"
date: "2017-03-05T21:20:24Z"
layout: post
path: "/shim-config-with-jspm/"
category: "Development with JSPM"
description: "Getting JSPM configuration can be tricky. With 0.17.0-beta, the meta key was introduced
to replace shim key. Learn about how to shim jQuery for various jQuery plugins that are written in global style."
---

In previous years, you may have seen about overrides like [this](https://github.com/jspm/jspm-cli/issues/689#issuecomment-93955524):

```
jspm install x -o "{ shim: { 'file.js': { deps: ['./another-file'] } } }"
```

With `jspm` 0.17.0-beta, `shim` has been renamed `meta`. I was struggling last week
with getting handful jQuery plugins (`signalr`, `slick-carousel`, `jquery-idletimer`)
to use the shimmed jQuery correctly. Here is what it boiled down for me:

Lets go through what is needed for `signalr` (aka `github:SignalR/bower-signalr@2.2.1`)
to work.

Add an override in `package.json` for the package:

```
{
  "format": "global",
  "main": "./jquery.signalR.js",
  "meta": {
    "jquery.signalR.js": {
      "globals": {
        "jQuery": "jquery"
      },
      "deps": [
        "jquery"
      ],
      "exports": "jQuery"
    }
  }
}
```

In `jspm.config.js`: add the appropriate mapping for `jquery` under `signalr` package:

```
SystemJS.config({
  packages: {
    'github:SignalR/bower-signalr@2.2.1': {
      'map': {
        'jquery': 'npm:jquery@2.2.4'
      }
    }
}
```

Note that any `package.json` override changes need a `jspm install` to be picked up
and translated to the JSON files stored under `jspm_packages/`. If you are unsure of your changes
and wanna iterate quickly, you can also modify those JSON files like `jspm_packages/github/SignalR/bower-signalr@2.2.1.json`
directly. Of course, having them in package.json is what will be the correct way to persist them.

Hopefully, that works for you. The correct way to use a jQuery function
added by the `signalr` module would be to do the following:

```
// Remember we exported the jQuery object from the signalr meta configuration
// why jQuery, because that's what gets injected as a global and gets modified
// over the course of loading of the signlar module

import $ from 'SignalR/bower-signalr';
$.hubConnection
```

See [SystemJS source](https://github.com/systemjs/systemjs/blob/master/dist/system.src.js#L3100-L3139)
if in doubt. Don't hesitate to switch to `system.src.js` instead of `system.js` in your `index.html`
and put a breakpoint there to see what's at play.

Cheers.
