# Jade static compiler for Derby.js

Jade fork for derby.js templates compilation (not the actual Derby.js template engine).
## [Jade documentation](https://github.com/visionmedia/jade)

Supports derby-specific tags that ends with `:` and makes `if, else, else if, unless, with, each` compile into derby View-variables.

## Usage

Right after creating your Derby app, add:

```js
// Add Jade compilation support
app.serverUse(module, 'derby-jade');
```

Make sure this is before any calls to `app.loadViews()`.

## Coffeescript

If you want to use Coffeescript instead of Javascript in templates:

```js
// Add Jade compilation support
app.serverUse(module, 'derby-jade', {coffee: true});
```
Then you can do something like this:
```html
if a and b
  p 
    a(on-click="console.log c or 'log'") {{d or 'Click Me'}}
```

## Derby.js-specific syntax

### Conditionals, `each`, `with`

```jade
if _session.loggedIn
  h1 Hello, {{_session.username}}
else
  a(href='/login') Login
```
compiles to
```html
{{if _session.loggedIn}}
    <h1>Hello, {{_session.username}}</h1>
{{else}}
    <a href="/login">Login</a>
{{/}}
```

Another example:
```jade
if _page.flash as #flash
  if #flash.error
    ul.alert.alert-error
      each #flash.error
        li {{this.error}}
  if #flash.info
    ul.alert.alert-success
      each #flash.info as #info
        li {{#info}}
else
  p No notifications
```
 compiles to
```html
{{if _page.flash as #flash}}
    {{if #flash.error}}
        <ul class="alert alert-error">
            {{each #flash.error}}
                <li>{{this.error}}</li>
            {{/}}
        </ul>
    {{/}}
    {{if #flash.info}}
        <ul class="alert alert-success">
            {{#each #flash.info as #info}}
                <li>{{#info}}</li>
            {{/}}
        </ul>
    {{/}}
{{else}}
    <p>No notifications</p>
{{/}}
```

### `import:` and template declarations

```jade
import:(src='./auth', ns='')
import:(src='./games')

Title:
  | My cool app

Body:
  view(name='welcome', title='Welcome {{_session.username}}')
    p We are glad to see you!

Footer:
  view(name='copyright')

welcome:
  h1 {{@title}}
  | {{@content}}

copyright:
  p Use it however you want {{_session.username}}!
```
