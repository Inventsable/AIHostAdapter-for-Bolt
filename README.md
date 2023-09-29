# Bolt AIHostAdapter Demo

### This panel is very barebones and [relies on AIHostAdapter](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_11.x/AIHostAdapter) to function, which you [must install manually in your program files](https://community.adobe.com/t5/illustrator-discussions/aihostadapter-plugin-windows-install/td-p/12727345) as an SDK extension. This panel receives a call each time the selection of the document is changed to update in console as an example of how to implement AIHostAdapter in a Bolt project.

## With Bolt

If you're using a project created from Bolt-CEP, you'll need to explicitly import and export required objects from [the HostAdapter file located here](./src/public/BoltHostAdapter.js):

```js
//    ./src/public/BoltHostAdapter.js

import { CSInterface, CSEvent } from "../js/lib/cep/csinterface"; // We need to pull CSInterface from Bolt first

/**
 * EventAdapter provides the base implementation for Creative Cloud
 * application-specific event adapter libraries.
 *
 * ...
 **/

export { AIEventAdapter, AIEvent }; // then export our needs from this file
```

Then you're free to import and use the HostAdapter as needed in your project:

```js
// ./src/js/main.vue

// @ts-ignore - Since HostAdapter isn't explicitly typed, linting throws an error we'll ignore
import { AIEventAdapter, AIEvent } from "../../public/BoltHostAdapter.js";

const adapter = AIEventAdapter.getInstance();
adapter.addEventListener(AIEvent.ART_SELECTION_CHANGED, async (e: any) => {
  console.log("Host Adapter triggered:");
  console.log(e);
});
console.log(AIEventAdapter); // Successfully fires each time a user changes selection in app (even empty so includes any click on canvas, really)
```

## Without Bolt

If you're trying to get HostAdapter to work with a smaller or simpler project that isn't generated from Bolt-CEP and Vite, the needs are essentially the same as any other library.

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My CEP PAnel</title>
    <!-- Manually include it in the head of your HTML document, potentially after loading CSInterface.js -->
    <script src="./aiHostAdapter.js"></script>
  </head>
</html>
```

A non-Bolt example of using the library [can be found here](https://github.com/Inventsable/Simple-resolution-panel/blob/master/public/aiHostAdapter.js) with an unmodified file.
