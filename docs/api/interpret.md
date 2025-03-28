---
layout: api.njk
title: interpret
tags: api
permalink: api/interpret.html
---

# interpret

__Table of Contents__
@[toc]

The __interpret__ function takes a [machine](./createMachine.html) and creates a `service` that can send events into the machine, changing its states. A service *does not* mutate a machine, but rather creates derived machines with the current state set.

```js
import { createMachine, interpret, state, transition } from 'robot3';

const machine = createMachine({
  asleep: state(
    transition('wake', 'breakfast')
  ),
  breakfast: state()
});

const service = interpret(machine, () => {
  if(service.machine.current === 'breakfast') {
    console.log('Wake up!');
  }
});

// Later
service.send('wake'); // Wake up!
```

## Arguments

__signature__: `interpret(machine, onChange, event)`

These are the arguments to `interpret`:

### machine

The state machine, created with [createMachine](./machine.html) to create a new service for.

### onChange

A [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) that is called when the machine completes a transition. Even if the transition results in returning to the same state, the `onChange` callback is still called.

The `onChange` function is called back with the `service`.

### event

The third argument `event`, can be any object. It is passed to the [context function](./createMachine#context) like so:

```js
const context = event => ({
  foo: event.foo
});

const machine = createMachine({
  idle: state()
}, context);

const event = {
  foo: 'bar'
};

interpret(machine, service => {
  // Do stuff when the service changes.
}, event);
```

## Service

A `service` is an object that interprets a machine, keeping track of the current `state`. Typically you will only really use `service.send` to send events and `service.context` to read extended state values (like those captured from form fields or API requests).

### Properties

#### machine

An extended version of the machine passed into `interpret`, contains a `current` property which is the current state (string name), and a `state` property which is an object with the `name` and the [state](./state.html) object (as the property `value`).

#### send

The `send` function is used to send events into the machine:

```js
service.send('wake');
```

#### context

The `context` for the service is an object that results from the [context function](./createMachine.html) when creating the machine, as well as any [reducers](./reduce.html) that have run as a result of state changes.