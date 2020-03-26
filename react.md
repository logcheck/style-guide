React
-----

Use the spread operator to pass in props from Ruby
==================================================

```erb
<%= content_tag :div, nil,
  id: 'some-component',
  data: {
    initialValueForComponent: 1
  }.to_json
%>
```
```jsx
const element = document.getElementById('some-component')
if (element) {
  const data = JSON.parse(element.getAttribute('data'))
  ReactDOM.render(<SomeComponent {...data} />, element)
}
```

Prefer functional components over classes
=======================================

Further reading: https://reactjs.org/docs/hooks-intro.html
