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

Nuestro Ejemplo

```js
function sendData (page, action, label, flowId, isError) {
  if (!label || !action || !page) {
    throw new Error('Cannot send event')
  }
  //(...)
}
```

Note: our example

----

¿Cuál es la orden? Label después de action?

```js
sendData('home', 'click', 'next')
sendData('faq', 'click', 'not-found', '123', true)
sendData('step4', 'submit', 'buying', null, false)
```

Note: using the function. you have to check the order and types

----

¡Error!

```js
sendData('step4')
sendData('step4', 'click')
sendData()
```

Note: Forget something and it will throw an error

----

Muchos escenarios -> muchas pruebas

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
describe('A component', () => {
  it('sends data to analytics', () => {
    expect(sendData).toHaveBeenCalledWith('home', 'click', 'next')
  })
})
```

Note: A lot of tests for coverage all the scenarios

----

Mismo con verificación de parámetros y pruebas...

```js
// forced to pass flowId null
sendData('step4', 'submit', 'buying', null, true)

// wrong values
sendData('faq', '123', true, 'not-found')
```

Note: even with tests we can have a lot of issues

---

## Empezar la refactorización

----

Objeto como parámetro

```js
function sendData (data) {
  console.log(data.page)
  console.log(data.action)
  //(...)
}
```

Note: object as argument

----

¡Babel es mi lenguaje!

```js
const sendData = ({page, action, label, flowId, isError}) => {
  console.log(page)
  console.log(action)
  //(...)
}
```

----

La orden no es más importante
```js
sendData({page: 'home', action: 'click', label: 'next'})
sendData({page: 'step4', action: 'submit', label: 'buying', isError: false})
sendData({page: 'faq', action: 'click', label: 'not-found', flowId: '123', isError: true})
```

----

Pero aún tenemos errores

```js
sendData({page: 'step4'})
sendData({page: 'step4', action: 'click'})
sendData()
```

----

Mejora algo

```js
sendData('step4', 'submit', 'buying', null, true) // forced to pass flowId null
sendData({page: 'step4', action: 'submit', label: 'buying'})

sendData('faq', '123', true, 'not-found') // wrong values
sendData({page: 'faq', action: '123', label: true, flowId: 'not-found'})
```

---

<img src="images/flow-logo.png" />

----

- Static Type Checker
- Created by Facebook (React también!)

----

Pequeño ejemplo

```js
const sum = (a, b) => a + b
```

```js
const sum = (a: number, b: number) => a + b
```

```js
sum(2, 3)
```

----

y si...

```js
sum(true, 3)
```

```
src/sum.js:40
 40: sum(true, 2)
         ^^^^ boolean. This type is incompatible with the expected param type of
 38: export const sum = (a: number, b: number) => a + b
                            ^^^^^^ number
```

----

Intentando usar

```js
const sendData = ({page, action, label, flowId, isError}:
  {page: string, action: string, label: string, flowId: string, isError: boolean}) => {
  //(...)
}
```

----

¡alias!

```js
type Event = {
  page: string,
  action: string,
  label: string,
  flowId: string,
  isError: boolean
}

const sendData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

----

Todos los parámetros

```js
sendData({page: 'step4', action: 'submit', label: 'buying', isError: false})
```

----

? para llaves opcionales

```js
type Event = {
  page: string,
  action: string,
  label: string,
  flowId?: string,
  isError?: boolean
}

const sendData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

----

```js
sendData({page: 'faq', action: 'click', label: 'question'})
```

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

```js
describe('sendData', () => {
  //(...)
})
```

---



---

¿The end?

----

## Roberto Soares
<hr />
github @roberto

twitter @bt1

React, Redux, Elm, Functional Programming, Babel, Flow

Note: TODO {||}, // @flow
