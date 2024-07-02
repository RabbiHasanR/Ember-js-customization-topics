# Customizing RESTAdapter in Ember.js for Form Data POST Requests

I recently tackled a project where I needed to customize the `RESTAdapter` in Ember.js to handle `createRecord` requests using form data. Here's a quick guide on how I achieved it:

## 💡 Why Customize `RESTAdapter`?
In some scenarios, especially when dealing with file uploads or complex nested data, sending data as `application/json` might not be suitable. Instead, sending the data as `multipart/form-data` can be more appropriate.

## 🔧 Custom Implementation

Below is an implementation of the `createRecord` method in a custom `ApplicationAdapter` that sends data as `multipart/form-data` using jQuery's `$.ajax`.

### Step-by-Step Code Breakdown

1. **Extend RESTAdapter**:
   - We start by extending the `RESTAdapter` provided by Ember Data.

2. **Define API Namespace and Host**:
   - Set the `namespace` and `host` for our API.

3. **Override `createRecord` Method**:
   - Build the URL for the `createRecord` request.
   - Extract the attributes from the snapshot and append them to a `FormData` object.
   - Use `RSVP.Promise` to handle the asynchronous request.
   - Utilize `$.ajax` to send a POST request with the form data.

Here's the complete code:

```javascript
import RESTAdapter from '@ember-data/adapter/rest';
import RSVP from 'rsvp';
import $ from 'jquery';

export default class ApplicationAdapter extends RESTAdapter {
  namespace = 'api/v1';
  host = 'https://example.com';

  createRecord(store, type, snapshot) {
    let url = this.buildURL(type.modelName, null, snapshot, 'createRecord');
    let data = snapshot.attributes();
    let formData = new FormData();

    // Append attributes to FormData
    for (let key in data) {
      if (data[key]) {
        formData.append(key, data[key]);
      }
    }

    return new RSVP.Promise((resolve, reject) => {
      $.ajax({
        type: 'POST',
        url: url,
        data: formData,
        contentType: false,
        processData: false,
      }).then(
        (data) => resolve(data),
        (jqXHR) => {
          jqXHR.then = null;
          reject(jqXHR);
        }
      );
    });
  }
}
```

### How It Works:
- Build the URL: `this.buildURL(type.modelName, null, snapshot, 'createRecord')` constructs the API endpoint for the `createRecord` request.
- Prepare FormData: Iterate over the snapshot attributes and append them to the `FormData` object.
- Handle AJAX Request: Use `$.ajax` to send a POST request with the form data, setting contentType and `processData` to `false`.

## 🚀 Why Use This Approach?
- Flexibility: Easily handle file uploads and nested data.
- Compatibility: Works well with APIs expecting `multipart/form-data`.