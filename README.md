Gitter.im Services [![Gitter chat](https://badges.gitter.im/gitterHQ/services.png)](https://gitter.im/gitterHQ/services)
==================
The things that power your Gitter activity feed.

What services are available?
----------------------------
If its in the `lib` folder, we support it.

How to add your service
-----------------------
1. `git clone git@github.com:gitterHQ/services.git`
2. `cd services`
3. `npm install`
4. `npm test`

If everything passes, then you are ready!

### Basic structure
```
.
└── lib
    └── YOUR_SERVICE_NAME
        ├── index.js
        ├── icons
        │   ├── logo.png
        │   └── logo@2x.png
        ├── instructions.md
        ├── settings.json
        ├── examples
        │   └── some_example_webhook.json
        └── test
            └── index.js
```

* `index.js`: This module has to export the following:
  * `apiVersion`: (_number_) the major version of this api.
  * `friendlyName`: (_string_) how you would like your service named to the user (e.g github's friendly name is GitHub).
  * `parse`: (_function_) the function to parse any incoming webhooks.
* `icons`: This directory contains all the png icons that can be used by this service. They must follow the following rules:
  * each icon must exist as both a 16x16 png and a 32x32 png (`name.png` and `name@2x.png` respectively).
  * there must be a logo icon (`logo.png` and `logo@2x.png`).
* `instructions.md`: The instructions the will be displayed when someone needs to set up your service to emit webhooks.
* `settings.json`: This represents the settings available available to the user when creating an integration. At the moment, it's only list of events.
  Format is:
  ```json
  {
    "events": [
      {
        "id": "someId",
        "name": "My Event",
        "description": "An explanation of the event",
        "selected": true
      },
      ...
    ]
  }
  ```
* `examples`: This directory contains examples to be used in your tests (and our sanity testing). Again, rules:
   * all examples must be in `json`.
   * all examples must be in the format `{ headers: {...}, body: {...} }`.
* `test`: directory of standard [mocha](http://visionmedia.github.io/mocha) tests. Cool people write tests. You _are_ cool, arn't you?


Example
-------
Lets say that you want to your service on Gitter to have a button like this:

![Jenkins button](img/button.png)

Which opens up a configuration modal like this:

![Jenkins settings](img/settings.png)

Which then outputs something like this:

![Jenkins activity item](img/activity-item.png)

Then your `index.js` needs to look like this:
```javascript
module.exports = {
  apiVersion: 0,
  friendlyName: 'Jenkins',
  parse: function(headers, body, settings) {
    return {
      message: 'Jenkins [webhooks-handler](http://users-jenkins.server.com/job/webhooks-handler/6/) success',
      icon: 'smile',
      errorLevel: 'normal'
    };  
  },
  options: [
    { id: "started", name: "Started", description: "When a build is started.", selected: false },
    { id: "success", name: "Success", description: "When a build finishes successfully.", selected: false },
    { id: "failure", name: "Failure", description: "When a build fails. Sad face.", selected: true },
  ]
};
```
Your `settings.json` needs to look like this:
```json
{
  "options": [
    { "id": "started", "name": "Started", "description": "When a build is started.", "selected": false },
    { "id": "success", "name": "Success", "description": "When a build finishes successfully.", "selected": false },
    { "id": "failure", "name": "Failure", "description": "When a build fails. Sad face.", "selected": true },
  ]
}
```
Your `instructions.md` should look like this:
```markdown
1. Select your job in Jenkins
2. Click the "Configure" link
3. Scroll down to Job notifications
4. Click the "Add Endpoint" button
5. Make sure Format is "JSON"
6. Make sure protocol is "HTTP"
7. Paste in your webhook url into "URL"
8. Click "Apply"
```
And finally, your `icons` directory needs to contain these:

![Jenkins icons](img/icons.png)

Community
---------
Are things going wrong? Are things going right? Do you have an awesome idea that you need a hand with?

Come and chat with everyone at [gitter.im/gitterHQ/services](https://gitter.im/gitterHQ/services).

Authors
-------

Started by [malditogeek](https://github.com/malditogeek), shuffled around by [trevorah](https://github.com/trevorah).

License
-------
MIT
