```js
export function eventTag (category, action, label, flowId, isError) {
  if (!label || !action || !category) {
    throw new Error('Cannot add event: label, action or category with null value')
  }
  //(...)
}
```

```js
export function eventTag (category, action, label, flowId, isError) {
  //(...)
}
```

```js
eventTag('home', 'click', 'next')
eventTag('faq', 'click', 'not-found', '123', true)
eventTag('step4', 'submit', 'buying', null, false)
```

```js
eventTag('step4')
eventTag('step4', 'click')
eventTag()
```

```js
describe('eventTag', () => {
  context('correct arguments', () => {
    //(...)
  })
  context('incorrect arguments', () => {
    it('throws error when receives only category')
    it('throws error when receives only label')
    it('throws error when receives only action')
    it('throws error when receives only category and label')
    it('throws error when receives only category and action')
    it('throws error when receives only label and action')
  })
})
```

```js
eventTag('step4', 'submit', 'buying', null, true) // forced to pass flowId null
eventTag('faq', '123', true, 'not-found') // wrong values
```

old school

```js
export function eventTag (data) {
  console.log(data.category)
  console.log(data.action)
  //(...)
}
```

ES6 - Babel language

```js
export const eventTag = ({category, action, label, flowId, isError}) => {
  console.log(category)
  console.log(action)
  //(...)
}
```

```js
eventTag({category: 'home', action: 'click', label: 'next'})
eventTag({category: 'step4', action: 'submit', label: 'buying', isError: false})
eventTag({category: 'faq', action: 'click', label: 'not-found', flowId: '123', isError: true})
```

```js
eventTag({category: 'step4'})
eventTag({category: 'step4', action: 'click'})
eventTag()
```

TODO testes

```js
eventTag('step4', 'submit', 'buying', null, true) // forced to pass flowId null
eventTag({category: 'step4', action: 'submit', label: 'buying'})

eventTag('faq', '123', true, 'not-found') // wrong values
eventTag({category: 'faq', action: '123', label: true, flowId: 'not-found'})
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
export const eventTag = ({category, action, label, flowId, isError}: {category: string, action: string, label: string, flowId: string, isError: boolean}) => {
  //(...)
}
```

First example

```js
type Event = {
  category: string,
  action: string,
  label: string,
  flowId: string,
  isError: boolean
}

export const eventTag = ({category, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
eventTag({category: 'step4', action: 'submit', label: 'buying', isError: false})
```

Optional values

```js
type Event = {
  category: string,
  action: string,
  label: string,
  flowId?: string,
  isError?: boolean
}

export const eventTag = ({category, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
eventTag({category: 'faq', action: '123', label: true, flowId: 'not-found'})
```

```js
describe('eventTag', () => {
  context('correct arguments', () => {
    //(...)
  })
  context('incorrect arguments', () => {
    it('throws error when receives only category')
    it('throws error when receives only label')
    it('throws error when receives only action')
    it('throws error when receives only category and label')
    it('throws error when receives only category and action')
    it('throws error when receives only label and action')
  })
})
```

```js
describe('eventTag', () => {
  //(...)
})
```
