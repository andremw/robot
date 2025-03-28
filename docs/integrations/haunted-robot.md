---
layout: page.njk
title: haunted-robot
tags: integrations
permalink: integrations/haunted-robot.html
---

# haunted-robot

__Table of Contents__
@[toc]

The __haunted-robot__ package provides the `useMachine` hook for use with [Haunted](https://github.com/matthewp/haunted/pull/136).

```js
import { createMachine, state, transition } from 'robot3';
import { component } from 'haunted';
import { html } from 'lit-html';
import { useMachine } from 'haunted-robot';

const machine = createMachine({
  off: state(
    transition('toggle', 'on')
  ),
  on: state(
    transition('toggle', 'off')
  )
});

function App() {
  const [current, send] = useMachine(machine);

  return html`
    <div>State: {current.name}</div>
    <button @click=${() => send('toggle')}>
      Toggle
    </button>
  `;
}

customElements.define('my-app', component(App));
```

## Installation

Available as `haunted-robot` on [npm](https://www.npmjs.com/package/haunted-robot):

```bash
npm install haunted-robot --save
```

Or through [Yarn](https://yarnpkg.com):

```bash
yarn add haunted-robot
```

## API

<code class="api-signature">useMachine(machine)</code>

Includes the __arguments__:

* `machine`: A Robot [state machine](../api/createMachine.html).

__Returns__ the following as an array:

`[current, send, service]`

* `current`: A __state__ object with the properties:
  * `name`: The name of the state (like `off` from the above example).
  * `context`: The [context](../api/createMachine.html#context) object for the [service](../api/interpret.html#service).
* `service`: A [service](../api/interpret.html#service) like you normally get from [interpret](../api/interpret.html).

Normally only the `current` and `send` properties are needed to be used.

Here the state `name` is logged, as well as data from the [context](../api/createMachine.html#context).

```js
const [current, send] = useMachine(machine);
const { userName } = current.context;

console.log('Current state:', current.name);
console.log('Username from context', userName);
```

The `send` property is used to send events into the state machine:

```js
const [current, send] = useMachine(machine);

return html`
  <button @click=${() => send('toggle')}>
    Toggle
  </button>
`;
```

## License

[BSD 2-Clause](https://opensource.org/licenses/BSD-2-Clause)