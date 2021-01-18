# Conditional testing

Conditional testing is [strongly discouraged](https://on.cypress.io/conditional-testing) in Cypress. But if you _must_ do something conditionally in Cypress depending on the page, here are few examples.

## Toggle checkbox

Let's say that you have a checkbox when the page loads. Sometimes the checkbox is checked, sometimes not. How do you toggle the checkbox?

<!-- fiddle Toggle first solution-->

```html
<div>
  <input type="checkbox" id="done" />
  <label for="done">Toggle me!</label>
</div>
<script>
  // sometimes the checkbox is checked
  const dice = Math.random()
  if (dice < 0.5) {
    document.querySelector('input').checked = true
  }
</script>
```

```js
// first solution using jQuery $.prop
cy.get('input').then(($checkbox) => {
  const initial = Boolean($checkbox.prop('checked'))
  // let's toggle
  cy.log(`Initial checkbox: **${initial}**`)
  // toggle the property "checked"
  $checkbox.prop('checked', !initial)
})
```

<!-- fiddle-end -->

Alternative solution

<!-- fiddle Toggle second solution -->

```html
<div>
  <input type="checkbox" id="done" />
  <label for="done">Toggle me!</label>
</div>
<script>
  // sometimes the checkbox is checked
  const dice = Math.random()
  if (dice < 0.5) {
    document.querySelector('input').checked = true
  }
</script>
```

```js
cy.get('input')
  .as('checkbox')
  .invoke('is', ':checked') // use jQuery $.is(...)
  .then((initial) => {
    cy.log(`Initial checkbox: **${initial}**`)
    if (initial) {
      cy.get('@checkbox').uncheck()
    } else {
      cy.get('@checkbox').check()
    }
  })
```

<!-- fiddle-end -->