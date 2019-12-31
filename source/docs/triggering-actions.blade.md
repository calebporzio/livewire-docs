---
title: Triggering Actions
extends: _layouts.documentation
section: content
---

Livewire currently offers a handful of directives to make listening to browser events trivial. The common format for all of them is: `wire:[native event]="[action]"`.

Here are some common events you may need to listen for:

@table
Event | Directive
--- | ---
click | `wire:click`
keydown | `wire:keydown`
submit | `wire:submit`
@endtable

Here are a few examples of each in HTML:

**click**
@code
<button wire:click="showModal">Show Modal</button>
@endcode

**keydown**
@code
<input wire:keydown.enter="search">
@endcode

**submit**
@code
<form wire:submit.prevent="addTodo">
    <input wire:model="title">
    <button>Add Todo</button>
</form>
@endcode

@tip
You can listen for any event emitted by the element you are binding to. Let's say you have an element that fires a browser event called "foo", you could listen for that event like so: <code>&lt;button wire:foo="someAction"&gt;</code>
@endtip

@tip
You can pass parameters to the triggered method from the Blade component. For example, a button such as: <code>&lt;button wire:click="someAction('parameter')"&gt;</code> will pass the parameter to the component class' method: <code>public function someAction($parameter)</code>
@endtip

## Modifiers {#modifiers}

Like you saw in the **keydown** example, Livewire directives sometimes offer "modifiers" to add extra functionality to an event. Below are the available modifiers that can be used with any event:

@table
Modifier | Description
--- | ---
stop | Equivalent of `event.stopPropagation()`
prevent | Equivalent of `event.preventDefault()`
self | Only triggers an action if the event was triggered on itself. This prevents outer elements from catching events that were triggered from a child element. (Like often in the case of registering a listener on a modal backdrop)
@endtable

## Keydown Modifiers {#keydown-modifiers}

To listen for specific keys on **keydown** events, you can pass the name of the key as a modifier. You can directly use any valid key names exposed via [KeyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) as modifiers by converting them to kebab-case.

Here is a quick list of some common ones you may need:

@table
Native Browser Event | Livewire Modifier
--- | ---
Backspace | backspace
Escape | escape
Shift | shift
Tab | tab
ArrowRight | arrow-right
@endtable

@code
<input wire:keydown.page-down="foo">
@endcode

In the above example, the handler will only be called if `event.key` is equal to 'PageDown'.

## Special Actions {#special-actions}
In Livewire, there are some "special" actions that are usually prefixed with a "$" symbol:

@table
Function | Description
--- | ---
$set('_property_', _value_) | Shortcut to update the value of a property
$toggle('_property_') | Shortcut to toggle boolean properties on or off
$refresh | Will re-render the component without firing any action
$emit('_event_', _...params_) | Will emit an event on the global event bus, with the provided params
$event | A _special_ variable that holds the value of the event fired that triggered the action. Example usage: `wire:change="setSomeProperty($event.target.value)"`
@endtable

You can pass these as the value of an event listener to do special things in Livewire.

Let's take `$set()` for example. It can be used to manually set a component property's value. Consider the `Counter` component's view.

**Before**

@code(['lang' => 'php'])
@verbatim
<div>
    {{ $this->message }}
    <button wire:click="setMessageToHello">Say Hi</button>
</div>
@endverbatim
@endcode

**After**

@code(['lang' => 'php'])
@verbatim
<div>
    {{ $this->message }}
    <button wire:click="$set('message', 'Hello')">Say Hi</button>
</div>
@endverbatim
@endcode

Notice that we are no longer calling the `setMessageToHello` function, we are directly specifying, what we want data set to.

@tip
This can save on lots of redundant, one-line component methods that only exist to set, or toggle the value of component property.
@endtip
