# Pruebas son buenas, pero impossible es mejor
<hr />
<h4 class="subtitle">yendo más allá con tipos</h4>

---

## Roberto Soares
<hr />
github @roberto

twitter @bt1

React, Redux, Elm, Functional Programming, Babel

---

## Let's refactor this:

```js
export function sendData (page, action, label, flowId, isError) {
  if (!label || !action || !page) {
    throw new Error('Cannot send event')
  }
  //(...)
}
```

Note: our example

----

```js
export function sendData (page, action, label, flowId, isError) {
  //(...)
}
```

Note: let's focus in the params

----

```js
sendData('home', 'click', 'next')
sendData('faq', 'click', 'not-found', '123', true)
sendData('step4', 'submit', 'buying', null, false)
```

Note: using the function. remember the order, types

----

```js
sendData('step4')
sendData('step4', 'click')
sendData()
```

Note: forget something and it will throw an error

----

```js
describe('sendData', () => {
  context('correct arguments', () => {
    //(...)
  })
  context('incorrect arguments', () => {
    it('throws error when receives only page')
    it('throws error when receives only label')
    it('throws error when receives only action')
    it('throws error when receives only page and label')
    it('throws error when receives only page and action')
    it('throws error when receives only label and action')
  })
})
```

----

```js
sendData('step4', 'submit', 'buying', null, true) // forced to pass flowId null
sendData('faq', '123', true, 'not-found') // wrong values
```

old school

```js
export function sendData (data) {
  console.log(data.page)
  console.log(data.action)
  //(...)
}
```

ES6 - Babel language

```js
export const sendData = ({page, action, label, flowId, isError}) => {
  console.log(page)
  console.log(action)
  //(...)
}
```

```js
sendData({page: 'home', action: 'click', label: 'next'})
sendData({page: 'step4', action: 'submit', label: 'buying', isError: false})
sendData({page: 'faq', action: 'click', label: 'not-found', flowId: '123', isError: true})
```

```js
sendData({page: 'step4'})
sendData({page: 'step4', action: 'click'})
sendData()
```

TODO testes

```js
sendData('step4', 'submit', 'buying', null, true) // forced to pass flowId null
sendData({page: 'step4', action: 'submit', label: 'buying'})

sendData('faq', '123', true, 'not-found') // wrong values
sendData({page: 'faq', action: '123', label: true, flowId: 'not-found'})
```

FLOW!!

Created by Facebook
Static Type Analyzer

```
const sum = (a, b) => a + b
```

```
const sum = (a: number, b: number) => a + b
```

```js
export const sendData = ({page, action, label, flowId, isError}: {page: string, action: string, label: string, flowId: string, isError: boolean}) => {
  //(...)
}
```

First example

```js
type Event = {
  page: string,
  action: string,
  label: string,
  flowId: string,
  isError: boolean
}

export const sendData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
sendData({page: 'step4', action: 'submit', label: 'buying', isError: false})
```

Optional values

```js
type Event = {
  page: string,
  action: string,
  label: string,
  flowId?: string,
  isError?: boolean
}

export const sendData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
sendData({page: 'faq', action: '123', label: true, flowId: 'not-found'})
```

```js
describe('sendData', () => {
  context('correct arguments', () => {
    //(...)
  })
  context('incorrect arguments', () => {
    it('throws error when receives only page')
    it('throws error when receives only label')
    it('throws error when receives only action')
    it('throws error when receives only page and label')
    it('throws error when receives only page and action')
    it('throws error when receives only label and action')
  })
})
```

```js
describe('sendData', () => {
  //(...)
})
```
