# 🚀 How You Can Disable Incompatible Dependencies with ember-cli-fastboot: Example for ember-cli-bootstrap-4

Hey folks! 👋

Today, I want to share how you can disable incompatible dependencies with FastBoot. In modern web development, ensuring that our applications are performant and can run in diverse environments, including server-side rendering with FastBoot, is essential. One common challenge is dealing with addons that rely on jQuery or directly manipulate the window object. Here’s a quick guide on how to make such addons FastBoot compatible, using `ember-cli-bootstrap-4` as an example!

## Step-by-Step Guide:

### Install FastBoot and Dependencies:

```bash
ember install ember-cli-fastboot
ember install ember-cli-bootstrap-4

```

### Configure FastBoot Shims:

Modify your ember-cli-build.js to import the necessary libraries with the fastbootShim transformation.

```javascript
'use strict';

const EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function (defaults) {
  const app = new EmberApp(defaults, {});

  // Import jQuery
  app.import('node_modules/jquery/dist/jquery.js', {
    using: [{ transformation: 'fastbootShim' }],
  });

  // Import Popper.js and its utils
  app.import('node_modules/popper.js/dist/umd/popper.js', {
    using: [{ transformation: 'fastbootShim' }],
  });
  app.import('node_modules/popper.js/dist/umd/popper-utils.js', {
    using: [{ transformation: 'fastbootShim' }],
  });

  // Import Bootstrap JS
  app.import('node_modules/bootstrap/dist/js/bootstrap.js', {
    using: [{ transformation: 'fastbootShim' }],
  });

  return app.toTree();
};

```

### Ensure Addon Compatibility:

Ensure that the addon, such as ember-cli-bootstrap-4, is FastBoot compatible or add necessary shims.

By following these steps, you can ensure smooth server-side rendering for your Ember app, even when using addons that depend on browser-specific objects.

Feel free to reach out if you have any questions or need further assistance with Ember.js and ember-cli-fastboot! 😊

#EmberJS #FastBoot #WebDevelopment #JavaScript #jQuery #ServerSideRendering #FrontendDevelopment #TechTips