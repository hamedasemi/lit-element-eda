# LitElement EDA
Event-driven architecture (EDA), is a software architecture pattern promoting the production, detection, consumption of, and reaction to events.

## Background
An event can be defined as "a significant change in state". For example, when a consumer purchases a car, the car's state changes from "for sale" to "sold". A car dealer's system architecture may treat this state change as an event whose occurrence can be made known to other applications within the architecture.

This architectural pattern may be applied by the design and implementation of applications and systems that transmit events among loosely coupled software components and services. An event-driven system typically consists of event emitters (or agents), event consumers (or sinks), and event channels. Emitters have the responsibility to detect, gather, and transfer events. An Event Emitter does not know the consumers of the event, it does not even know if a consumer exists, and in case it exists, it does not know how the event is used or further processed.

## Install
```sh
npm install lit-element-eda --save
```

## Usage
```js
import { LitElement, html } from 'lit-element'
import { events } from 'lit-element-eda'

class DemoApp extends LitElement {
    static get properties() {
        return {
            state: { type: Object }
        }
    }

    constructor() {
        super()
        this.state = {}
        this.state.number = 0
        events.on('sub', this.subHandler.bind(this))
        events.on('add', this.addHandler.bind(this))
    }

    render() {
        return html`
            <h2>${this.state.number}</h2>
            <button @click='${this.sub}'>-</button>
            <button @click='${this.add}'>+</button>
        `
    }

    sub(){ events.dispatch('sub') }
    add(){ events.dispatch('add') }

    subHandler() {
        this.state = { ...this.state, number: --this.state.number }
    }

    addHandler() {
        this.state = { ...this.state, number: ++this.state.number }
    }
}

customElements.define('demo-app', DemoApp)
```