# tangojs-web-components

tangojs-web-components is a collection of controls for
[tangojs](https://tangojs.github.io). The controls are
designed to work with any framework (or vanilla JS), and
offer standard semantics of HTML*Element.

## Installation

It's available in npmjs registry, just get it:
```
$ npm install tangojs-web-components
```

and drop desired components onto your page:
```html
<link rel="import"
      href="node_modules/tangojs-web-components/dist/components/label.html">
```

### Configuration
`tangojs-web-components` requires `tangojs-core` to be configured before any
components are created. Example:
```html
<!-- load scripts -->

<script type="text/javascript">
  (function () {
    var model = demoModel.createModel()
    var conn = new tangojsConnectorLocal.LocalConnector(model)
    tangojs.setConnector(conn)
  })()
</script>

<!-- import components -->
<!-- use components -->
```

### Requirements
* Mozilla Firefox 45+
  * enable [`dom.webcomponents.enabled`](about:config)
  * enable [`layout.css.grid.enabled`](about:config)
  * use
    [HTMLImports polyfill](http://webcomponents.org/polyfills/html-imports/)
  * apply [this patch](https://github.com/mliszcz/html-imports-firefox-patch)
    just before the polyfill is loaded
* Google Chrome 49+
  * enable [experimental-web-platform-features](
    chrome://flags/#enable-experimental-web-platform-features)

## Components

All components are derived from
[`HTMLElement`](https://developer.mozilla.org/en/docs/Web/API/HTMLElement).
The behavior and layout depends on information received from underlying
model (e.g. `AttributeInfo` object).

* [tangojs-label](#label)
* [tangojs-line-edit](#lineedit)
* [tangojs-command-button](#commandbutton)
* [tangojs-state-led](#led)
* [tangojs-plot](#plot)
* [tangojs-trend](#trend)
* [tangojs-chart](#chart)
* [tangojs-image](#image)
* [tangojs-form](#form)
* [tangojs-device-tree](#devicetree)
* [tangojs-group](#group)

### Label

Displays value of an read-only attribute. The attribute is polled at
constant rate.

#### Examples

```html
<tangojs-label
  model="my/dev/01/attr01"
  poll-period="1000"
  show-name
  show-unit
  precision="3">
</tangojs-label>
```

#### Attributes

Property    | Type    | Attribute    | Remarks
----------- | ------- | ------------ | -------
model       | string  | model        | Full attribute name.
pollPeriod  | number  | poll-period  | Poll period in milliseconds.
showName    | boolean | show-name    | Should display name.
showUnit    | boolean | show-unit    | Should display unit.
showQuality | boolean | show-quality | Should display quality led.
precision   | number  | precision    | Number of decimal digits.

### LineEdit

Displays value of an writable attribute. The attribute is polled at
constant rate.

#### Examples

```html
<tangojs-line-edit
  model="my/dev/01/attr01"
  poll-period="1000"
  show-name
  show-unit>
</tangojs-line-edit>
```

#### Attributes

Property    | Type    | Attribute    | Remarks
----------- | ------- | ------------ | -------
model       | string  | model        | Full attribute name.
pollPeriod  | number  | poll-period  | Poll period in milliseconds.
showName    | boolean | show-name    | Should display name.
showUnit    | boolean | show-unit    | Should display unit.
showQuality | boolean | show-quality | Should display quality led.

### CommandButton

Executes command on the device. Takes arbitrary HTML nodes as children.

#### Examples

```html
<tangojs-command-button
  model="my/dev/01/cmd01"
  parameters="6">
  Click Me!
</tangojs-command-button>
```

#### Attributes

Property   | Type     | Attribute   | Remarks
---------- | -------- | ----------- | -------
model      | string   | model       | Full command name.
parameters | object   | parameters  | Parameters passed to the command.

#### Events

* `tangojs-command-result` - fired when command result is available
  * `event.detail.deviceData: DeviceData` - result

### Led

Displays device state.

#### Examples

```html
<tangojs-state-led
  model="my/dev/01"
  poll-period="1000"
  show-name
  show-led
  show-value
  only-name>
</tangojs-state-led>
```

#### Attributes

Property   | Type    | Attribute   | Remarks
---------- | ------- | ----------- | -------
model      | string  | model       | Full device name.
pollPeriod | number  | poll-period | Poll period in milliseconds.
showName   | boolean | show-name   | Should display name.
showLed    | boolean | show-led    | Should display led.
showValue  | boolean | show-value  | Should display value as string.
onlyName   | boolean | only-name   | Show only device name, instead of full path.

### Plot

*TODO*

### Trend

Plots multiple attributes over time.

#### Examples

```html
<tangojs-trend
  model="tangojs/test/dev1/sine_trend,tangojs/test/dev1/scalar"
  poll-period="1000"
  data-limit="5">
</tangojs-trend>
```

#### Attributes

Property   | Type     | Attribute   | Remarks
---------- | -------- | ----------- | -------
model      | string[] | model       | Array of attribute names.
pollPeriod | number   | poll-period | Poll period in milliseconds.
dataLimit  | number   | data-limit  | Max no. of entries per dataset.

#### Remarks

`tangojs-trend` widget is built
on top of [Chart.js](http://www.chartjs.org/). You have to include
dependencies manually:

```
<script src="node_modules/moment/min/moment.min.js"></script>
<script src="node_modules/chart.js/Chart.min.js"></script>
```

### Chart

Visualizes spectrum attributes.

#### Examples

```html
<tangojs-chart
  model="sys/tg_test/1/long_spectrum,sys/tg_test/1/double_spectrum"
  poll-period="1000">
</tangojs-chart>
```

#### Attributes

Property   | Type     | Attribute   | Remarks
---------- | -------- | ----------- | -------
model      | string[] | model       | Array of attribute names.
pollPeriod | number   | poll-period | Poll period in milliseconds.

#### Remarks

`tangojs-spectrum` widget is built
on top of [Chart.js](http://www.chartjs.org/). You have to include
dependencies manually:

```
<script src="node_modules/moment/min/moment.min.js"></script>
<script src="node_modules/chart.js/Chart.min.js"></script>
```

### Image

[10-02-2017] @GregViguier and his team is currently working on this widget.

### Form

Displays widgets for multiple attributes. Widgets are selected according
to the attribute type.

#### Examples

```html
<tangojs-form
  model="tangojs/test/dev1/sine_trend,tangojs/test/dev1/scalar"
  poll-period="1000">
</tangojs-form>
```

#### Attributes

Property   | Type     | Attribute   | Remarks
---------- | -------- | ----------- | -------
model      | string[] | model       | Array of attribute names.
pollPeriod | number   | poll-period | Poll period in milliseconds.

### DeviceTree

Displays devices, attributes and commands stored in database.

#### Examples

```html
<tangojs-device-tree></tangojs-device-tree>
```

#### Attributes

None.

#### Events

* `selected` - fires when element is selected (checked)
  * `event.detail.selections`
  * `event.detail.selectionsAdded`
  * `event.detail.selectionsRemoved`

#### API

* `getSelections(): Array<T>`, where `T` is
  ```
  {
    key: string,
    path: Array<string>,
    value: { model: string, info: (DeviceInfo|AttributeInfo|CommandInfo) }
  }
  ```
* `clearSelections(): undefined`,
* `collapse(): undefined`,
* `collapseAt(level: Number)`,
* `expand(): undefined`.

### Group

Groups several attributes into one widget. Shows device name, state,
attribute value and unit.

#### Examples

```html
<tangojs-group
  model="sys/tg_test/1/long_scalar,sys/tg_test/1/double_scalar"
  name="Test group">
</tangojs-group>
```

#### Attributes

Property   | Type     | Attribute   | Remarks
---------- | -------- | ----------- | -------
model      | string[] | model       | Array of attribute names.
name       | string   | name        | Group title.