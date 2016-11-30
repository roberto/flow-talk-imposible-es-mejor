```js
export function sendAnalyticsData (page, action, label, flowId, isError) {
  if (!label || !action || !page) {
    throw new Error('Cannot add event: label, action or page with null value')
  }
  //(...)
}
```

```js
export function sendAnalyticsData (page, action, label, flowId, isError) {
  //(...)
}
```

```js
sendAnalyticsData('home', 'click', 'next')
sendAnalyticsData('faq', 'click', 'not-found', '123', true)
sendAnalyticsData('step4', 'submit', 'buying', null, false)
```

```js
sendAnalyticsData('step4')
sendAnalyticsData('step4', 'click')
sendAnalyticsData()
```

```js
describe('sendAnalyticsData', () => {
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
sendAnalyticsData('step4', 'submit', 'buying', null, true) // forced to pass flowId null
sendAnalyticsData('faq', '123', true, 'not-found') // wrong values
```

old school

```js
export function sendAnalyticsData (data) {
  console.log(data.page)
  console.log(data.action)
  //(...)
}
```

ES6 - Babel language

```js
export const sendAnalyticsData = ({page, action, label, flowId, isError}) => {
  console.log(page)
  console.log(action)
  //(...)
}
```

```js
sendAnalyticsData({page: 'home', action: 'click', label: 'next'})
sendAnalyticsData({page: 'step4', action: 'submit', label: 'buying', isError: false})
sendAnalyticsData({page: 'faq', action: 'click', label: 'not-found', flowId: '123', isError: true})
```

```js
sendAnalyticsData({page: 'step4'})
sendAnalyticsData({page: 'step4', action: 'click'})
sendAnalyticsData()
```

TODO testes

```js
sendAnalyticsData('step4', 'submit', 'buying', null, true) // forced to pass flowId null
sendAnalyticsData({page: 'step4', action: 'submit', label: 'buying'})

sendAnalyticsData('faq', '123', true, 'not-found') // wrong values
sendAnalyticsData({page: 'faq', action: '123', label: true, flowId: 'not-found'})
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
export const sendAnalyticsData = ({page, action, label, flowId, isError}: {page: string, action: string, label: string, flowId: string, isError: boolean}) => {
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

export const sendAnalyticsData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
sendAnalyticsData({page: 'step4', action: 'submit', label: 'buying', isError: false})
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

export const sendAnalyticsData = ({page, action, label, flowId, isError}: Event) => {
  //(...)
}
```

```js
sendAnalyticsData({page: 'faq', action: '123', label: true, flowId: 'not-found'})
```

```js
describe('sendAnalyticsData', () => {
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
describe('sendAnalyticsData', () => {
  //(...)
})
```
