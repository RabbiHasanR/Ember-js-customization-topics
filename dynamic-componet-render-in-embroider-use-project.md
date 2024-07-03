# 🚀 Rendering Dynamic Components in Ember.js with Embroider

Hey folks! 👋

Today, I want to share a powerful technique for rendering dynamic components in Ember.js, especially when using Embroider, Ember's next-generation build system. This approach is particularly useful when the component names are retrieved from an API, and you need to render these components dynamically.

## Why Dynamic Components?

Dynamic components allow you to build flexible and reusable applications. They enable you to determine which component to render at runtime, based on data or other conditions. This is particularly useful in scenarios like content management systems or dashboards, where the UI components can change based on user input or configuration.

## Using Embroider for Dynamic Components

Embroider provides tools like `importSync` and `ensureSafeComponent` to help with dynamic component rendering. Here's how you can leverage these tools to render components dynamically in Ember.js.

## Step-by-Step Implementation

### 1. Create a Dynamic Component

First, create a dynamic component that will handle the rendering of other components based on the name provided.

### app/components/dynamic-component.js

```javascript
import Component from "@glimmer/component";
import { ensureSafeComponent } from "@embroider/util";
import { importSync } from "@embroider/macros";
import { tracked } from "@glimmer/tracking";

export default class DynamicComponent extends Component {
  get componentName() {
    return this.args.componentName || null;
  }

  get argsData() {
    return this.args.argsData || null;
  }

  get componentLabel() {
    if (this.componentName) {
      let module = importSync(`/components/${this.componentName}`);
      return ensureSafeComponent(module.default, this);
    }
    return null;
  }
}
```

### Explanation

- importSync: This function allows synchronous dynamic imports. It ensures the component module is available at runtime.

- ensureSafeComponent: This utility ensures that the component is safe to render, which is particularly important when dealing with dynamic components.

- get componentName: This property holds the name of the component to be rendered which you passed as args. You can update this property based on the data received from an API.

### 2. Dynamic Component Template

Create the template for the dynamic component to render the desired component dynamically.

### app/templates/components/dynamic-component.hbs

```hbs
{{#if this.componentLabel}}
    <this.componentLabel @argsData={{this.argsData}} />
{{/if}}
```

### Explanation

- this.componentLabel: This invokes the dynamically imported component.
- @argsData: This passes any necessary data to the dynamically rendered component.

### Example Usage

- Here’s an example of how you might use this DynamicComponent in your application.

### app/controllers/application.js

```javascript
import Controller from "@ember/controller";
import { action } from "@ember/object";
import { tracked } from "@glimmer/tracking";

export default class ApplicationController extends Controller {
  @tracked dynamicComponentName = "component_one/nested_component";
  @tracked argsData = { key: "value" };

  @action
  updateComponentName(newComponentName) {
    this.dynamicComponentName = newComponentName;
  }
}
```

### app/templates/application.hbs

```hbs
<DynamicComponent
  @componentName={{this.dynamicComponentName}}
  @argsData={{this.argsData}}
/>

<button
  {{on "click" (fn this.updateComponentName "component_two/another_component")}}
>Switch Component</button>

```

### Explanation

- DynamicComponent: This component renders the dynamic component based on the `componentName` and `argsData` passed to it.
- updateComponentName: This action updates the component name, demonstrating how you can switch components dynamically.

### Conclusion

Rendering dynamic components in Ember.js using Embroider's utilities like `importSync` and `ensureSafeComponent` provides a flexible and powerful way to build dynamic and interactive applications. This approach is particularly beneficial in scenarios where component names are retrieved from an API or other dynamic sources.

Feel free to reach out if you have any questions or need further assistance with Ember.js and Embroider! 😊

#EmberJS #Embroider #JavaScript #WebDevelopment #Programming #APIDevelopment #TechTips
