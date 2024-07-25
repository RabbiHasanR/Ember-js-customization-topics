# Dynamically or Programmatically Using Ember.js Services

In this guide, we'll demonstrate how to dynamically or programmatically load and use Ember.js services within a component. This can be particularly useful when the service to be used is determined at runtime based on component arguments or other dynamic factors.

## Example: `LoadServiceComponent`

Below is an example of an Ember.js component that dynamically loads a service based on a component argument.

```javascript
import Component from "@glimmer/component";
import { getOwner } from "@ember/application";
import { tracked } from "@glimmer/tracking";
import { action } from "@ember/object";

export default class LoadServiceComponent extends Component {
  @tracked serviceInstance = null;

  constructor() {
    super(...arguments);
    this.loadService();
  }

  @action
  didUpdateServiceName(element, [serviceName]) {
    this.loadService();
  }

  loadService() {
    let owner = getOwner(this);
    this.serviceInstance = owner.lookup(`service:${this.args.serviceName}`);
  }
}
```

The corresponding template for this component:

```hbs
<div
  class="data-fetcher-container"
  {{did-update this.didUpdateServiceName @arguments.serviceName}}
>
  {{log "items:" this.serviceInstance.items}}
</div>
```


### Explanation
#### 1. Component Class (LoadServiceComponent):

- Imports:
    - Component from @glimmer/component: Base class for Glimmer components.
    - getOwner from @ember/application: Utility to get the application's owner.
    - tracked from @glimmer/tracking: Decorator to create reactive state properties.
    - action from @ember/object: Decorator to define actions in the component.

- Class Definition:

    - serviceInstance: A tracked property to hold the dynamically loaded service instance.
    - constructor(): Calls the loadService() method to initialize the service instance when the component is instantiated.
    - didUpdateServiceName(element, [serviceName]): An action that triggers loadService() whenever the serviceName argument changes.
    - loadService(): Uses getOwner to lookup the service instance based on the serviceName argument and assigns it to the serviceInstance property.

#### 2. Template:

- The {{did-update}} modifier is used to call didUpdateServiceName whenever the serviceName argument changes.
- The {{log}} helper logs the items of the dynamically loaded service to the console.

### Benefits
- Dynamic Service Loading: This approach allows you to load different services dynamically based on component arguments, making the component more flexible and reusable.
- Reactive Updates: The use of @tracked ensures that changes to the service instance are reactive and will update the template accordingly.

### Usage

To use this component, simply pass the name of the service you wish to load as an argument:

```hbs
<LoadServiceComponent @serviceName="someService"/>
```

This will dynamically load the someService service and log its items to the console.

### Conclusion
This guide provides a straightforward example of how to dynamically load and use services in Ember.js components. By leveraging @tracked properties and @action decorators, you can create flexible and reactive components that adapt to changing arguments.

Feel free to adapt this documentation to fit your project's needs, adding any additional details or context specific to your application.