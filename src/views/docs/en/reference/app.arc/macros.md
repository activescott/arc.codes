---
title: '@macro'
description: Extend Architect app functionality
---

Extend the functionality of your Architect app with standard CloudFormation. The `@macro` primitive allows developers to add any resources or modify existing ones extending Architect into the entire AWS ecosystem supported by CloudFormation.

## Getting started

These example configuration files declare a macro saved to `src/macros/my-custom-macro.js`

<arc-viewer default-tab=arc>
<div slot=contents class=bg-g4>

<arc-tab label=arc>
<h5>arc</h5>
<div slot=content>

```arc
@app
testapp

@macros
my-custom-macro
```
</div>
</arc-tab>

<arc-tab label=json>
<h5>json</h5>
<div slot=content>

```json
{
  "app": "testapp",
  "macros": [
    "my-custom-macro"
  ]
}
```
</div>
</arc-tab>

<arc-tab label=toml>
<h5>toml</h5>
<div slot=content>

```toml
app="testapp"

macros=[
 "my-custom-macro"
]

```
</div>
</arc-tab>

<arc-tab label=yaml>
<h5>yaml</h5>
<div slot=content>

```yaml
---
app: testapp

macros:
- my-custom-macro
```
</div>
</arc-tab>

</div>
</arc-viewer>

### Deploy

When running `arc deploy` Architect looks for macros to run in:

- `src/macros/filename`
- `node_modules/macro-module-name`

You deploy a macro by using this syntax:

- `arc deploy` to deploy with CloudFormation to staging
- `arc deploy production` to run a full CloudFormation production deployment

## Examples

Macros receive the parsed `app.arc` file so custom pragmas and config can be defined. The second argument is the current CloudFormation template.

```js
/**
 * @param {object} arc - the parsed app.arc file currently executing
 * @param {object} cloudformation - the current AWS::Serverless CloudFormation template
 * @param {object} stage - the application stage (one of `staging` or `production`)
 **/
module.exports = function myCustomMacro(arc, cloudformation, stage) {
  // modify cloudformation.Resources here
  return cloudformation
}
```
> Note: macros are a new feature and only JavaScript macros are supported at this time; however Python and Ruby are planned
