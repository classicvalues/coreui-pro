---
layout: docs
title: Bootstrap alerts
description: Bootstrap alerts give contextual feedback information for common user operations. The alert component is delivered with a bunch of usable and adjustable alert messages.
group: components
aliases:
  - "/components/alerts/"
toc: true
---

## Examples

Bootstrap alert is prepared for any length of text, as well as an optional close button. For a styling, use one of the **required** contextual classes (e.g., `.alert-success`). For inline dismissal, use the (e.g., `.alert-success`). For inline dismissal, use the [alerts JavaScript plugin](#dismissing).

{{< example >}}
{{< alerts.inline >}}
{{- range (index $.Site.Data "theme-colors") }}
<div class="alert alert-{{ .name }}" role="alert">
  A simple {{ .name }} alert—check it out!
</div>{{- end -}}
{{< /alerts.inline >}}
{{< /example >}}

{{< callout info >}}
{{< partial "callout-warning-color-assistive-technologies.md" >}}
{{< /callout >}}

### Link color

Use the `.alert-link` utility class to immediately give matching colored links inside any alert.

{{< example >}}
{{< alerts.inline >}}
{{- range (index $.Site.Data "theme-colors") }}
<div class="alert alert-{{ .name }}" role="alert">
  A simple {{ .name }} alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>{{ end -}}
{{< /alerts.inline >}}
{{< /example >}}

### Additional content

Alert can also incorporate supplementary HTML elements like heading, paragraph, and divider.

{{< example >}}
<div class="alert alert-success" role="alert">
  <h4 class="alert-heading">Well done!</h4>
  <p>Aww yeah, you successfully read this important alert message. This example text is going to run a bit longer so that you can see how spacing within an alert works with this kind of content.</p>
  <hr>
  <p class="mb-0">Whenever you need to, be sure to use margin utilities to keep things nice and tidy.</p>
</div>
{{< /example >}}

### Dismissing

Using the JavaScript plugin, it's possible to remove any alert.

- Be sure you've loaded the bootstrap alert plugin or the compiled CoreUI JavaScript.
- Add a close button and the `.alert-dismissible` class, which adds some extra padding to the right of the alert component and positions the `.close` button.
- On the close button, add the `data-coreui-dismiss="alert"` attribute, which triggers the JavaScript functionality. You have to use the `<button>` element with it for proper behavior across all devices.
- To animate alerts when dismissing them, you have to add the `.fade` and `.show` classes.

You can see this in action with a live demo:

{{< example >}}
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-coreui-dismiss="alert" aria-label="Close"></button>
</div>
{{< /example >}}

{{< callout warning >}}
When an alert is dismissed, the element is completely removed from the page structure. If a keyboard user dismisses the alert using the close button, their focus will suddenly be lost and, depending on the browser, reset to the start of the page/document. For this reason, we recommend including additional JavaScript that listens for the `closed.coreui.alert` event and programmatically sets `focus()` to the most appropriate location in the page. If you're planning to move focus to a non-interactive element that normally does not receive focus, make sure to add `tabindex="-1"` to the element.
{{< /callout >}}

## JavaScript behavior

### Triggers

Enable dismissal of an alert via JavaScript:

```js
var alertList = document.querySelectorAll('.alert')
alertList.forEach(function (alert) {
  new coreui.Alert(alert)
})
```

Or with `data` attributes on a button **within the alert**, as demonstrated above:

```html
<button type="button" class="btn-close" data-coreui-dismiss="alert" aria-label="Close"></button>
```

Note that closing an alert will remove it from the DOM.

### Methods

You can create an alert instance with the alert constructor, for example:

```js
var myAlert = document.getElementById('myAlert')
var cuiAlert = new coreui.Alert(myAlert)
```

This makes an alert listen for click events on descendant elements which have the `data-coreui-dismiss="alert"` attribute. (Not necessary when using the data-api's auto-initialization.)

<table class="table">
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code>close</code>
      </td>
      <td>
        Closes an alert by removing it from the DOM. If the <code>.fade</code> and <code>.show</code> classes are present on the element, the alert will fade out before it is removed.
      </td>
    </tr>
    <tr>
      <td>
        <code>dispose</code>
      </td>
      <td>
        Destroys an element's alert. (Removes stored data on the DOM element)
      </td>
    </tr>
    <tr>
      <td>
        <code>getInstance</code>
      </td>
      <td>
        Static method which allows you to get the alert instance associated to a DOM element, you can use it like this: <code>coreui.Alert.getInstance(alert)</code>
      </td>
    </tr>
  </tbody>
</table>

```js
var alertNode = document.querySelector('.alert')
var alert = coreui.Alert.getInstance(alertNode)
alert.close()
```

### Events

Bootstrap's alert plugin exposes a few events for hooking into alert functionality.

<table class="table">
  <thead>
    <tr>
      <th>Event</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>close.coreui.alert</code></td>
      <td>
        Fires immediately when the <code>close</code> instance method is called.
      </td>
    </tr>
    <tr>
      <td><code>closed.coreui.alert</code></td>
      <td>
        Fired when the alert has been closed and CSS transitions have completed.
      </td>
    </tr>
  </tbody>
</table>

```js
var myAlert = document.getElementById('myAlert')
myAlert.addEventListener('closed.coreui.alert', function () {
  // do something, for instance, explicitly move focus to the most appropriate element,
  // so it doesn't get lost/reset to the start of the page
  // document.getElementById('...').focus()
})
```

## Customizing

### SASS
{{< scss-docs name="alert-variables" file="scss/_variables.scss" >}}

#### Variants

CoreUI allows defining variant colors in two ways.

Check out [our Sass maps and loops docs]({{< docsref "/customize/sass#maps-and-loops" >}}) for how to customize these loops and extend CoreUI's base-modifier approach to your own code.

##### Manual

You can define each color manually and keep full control of the component appearance.

{{< highlight scss >}}
$alert-variants: (
  "primary": (
    "background": $your-bg-color,
    "border": $your-border-color,
    "color": $your-color,
    "link-color": $your-link-color
  )
  ...
);
{{< /highlight >}}

##### Color function

The color set can be generated automatically thanks to our `alert-color-map` function.

{{< scss-docs name="alert-color-functions" file="scss/_functions.scss" >}}

{{< highlight scss >}}
$alert-variants: (
  "primary": alert-color-map($primary),
  ...
);
{{< /highlight >}}

#### Modifiers

CoreUI's alert component is built with a base-modifier class approach. This means the bulk of the styling is contained to a base class `.alert` while style variations are confined to modifier classes (e.g., `.alert-danger`). These modifier classes are built from the `$alert-variants` map to make customizing the number and name of our modifier classes.

{{< scss-docs name="alert-modifiers" file="scss/_alert.scss" >}}

### CSS Vars
{{< css-vars-docs file="scss/_alert.scss" >}}
