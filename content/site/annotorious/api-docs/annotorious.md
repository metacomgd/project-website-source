---
title: "API Docs | Annotorious"
date: 2020-05-17T14:14:36+02:00
draft: false
layout: "api-doc"
meta_title: "Annotorious API Docs"
meta_description: "API Documentation for the Annotorious image annotation library"
meta_link: "https://recogito.github.io/site/annotorious/api-docs/annotorious"
---

# API Reference: Annotorious

> The __standard version__ of Annotorious works on normal images embedded in websites
> or web applications. The API reference for the Annotorious OpenSeadragon plugin is 
> available [here](/site/annotorious/api-docs/osd-plugin).

## Initializing Annotorious

Initialize an Annotorious instance on an image with 

```javascript
var config = {
  image: document.getElementById('my-image'),
  readOnly: true
};

var anno = Annotorious.init(config);
```

The config object can have the following properties:

| Property    | Type | Value                                                                               | Default |
|-------------|------|-------------------------------------------------------------------------------------|---------|
| `image`     | `Element`, `String` | image element or, alternatively, element ID                          | -       |
| `readOnly`  | `Boolean` | Set to `true` to display all annotations read-only                             | `false` |
| `headless`  | `Boolean` | Completely disables the editor popup. Drawing is still possible, and lifecycle events still fire. Can be used in conjunction with [applyTemplate](#applytemplate). Note that headless mode currently supports only shape creation, not editing. | `false`    |

## Instance Methods

### addAnnotation

```js
anno.addAnnotation(annotation [, readOnly]);
```

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](https://github.com/recogito/annotorious/wiki/Web-Annotation-Model). At the moment, 
only a single `FragmentSelector` with an `xywh=pixel` fragment is supported.

| Argument     | Type    | Value                                                       |
|--------------|---------|-------------------------------------------------------------|
| `annotation` | Object  | the annotation according to the W3C WebAnnotation format    |
| `readOnly`   | Boolean | set the second arg to `true` to display the annotation in read-only mode |

### removeAnnotation

```js
anno.removeAnnotation(arg);
```

Removes an annotation programmatically. 

| Argument     | Type           | Value                                                           |
|--------------|----------------|-----------------------------------------------------------------|
| `arg`        | Object, String | the annotation in W3C WebAnnotation format or the annotation ID |

### setAnnotations

```js
anno.setAnnotations(annotations);
```

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Value                                         |
|---------------|-----------------------------------------------|
| `annotations` | array of annotations in W3C WebAnnotation format |

### loadAnnotations

```js
anno.loadAnnotations(url);
```

Loads annotations from a JSON file. The method returns a promise, in 
case you want to perform an action after the annotations have loaded.

```javascript
anno.loadAnnotations(url).then(function(annotations) {
  // Do something
});
```

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `url`     | the URL to HTTP GET the annotations from |

### getAnnotations

```js
anno.getAnnotations();
```

Returns all annotations according to the current rendered state, in W3C Web Annotation format. 

### setAnnotationsVisible

```js
anno.setAnnotationsVisible(visible);
```

Shows or hides the annotation layer.

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `visible` | if `true` show the annotation layer, otherwise hide it |

### selectAnnotation

```js
anno.selectAnnotations(arg);
```

Selects an annotation programmatically, highlighting its shape, and opening the editor popup. 

- If no argument is provided (or the annotation or ID is unknown), this method will deselect 
  the current selection, if any
- The method will return the selected annotation as a result
- Note that the the `selectAnnotation` event will __not__ fire when using this method

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `arg` | the annotation or the annotation ID |

### applyTemplate

```js
anno.applyTemplate(template);
```

When a new annotation is created, Annotorious will automatically apply the given 
template. The template can be a single annotation body, or an array of bodies.
If `openEditor` is set to true, the editor popup will open, allowing the user to 
edit the annotation normally. Otherwise, Annotorious will __instantly__ create an 
annotation from the template bodies __only__. The editor will not open. 

Use this method if you want to implement batch- or fast annotation modes, where
the same annotation should be applied repeatedly, and the user should only take
care of drawing selections.

| Argument   | Value                                    |
|------------|------------------------------------------|
| `template` | the template body or list of bodies to apply automatically |
| `openEditor` | if `true`, the editor will open after the template is applied |

### destroy

```js
anno.destroy();
```

Destroys this instance of Annotorious, removing the annotation layer on the image.

### on

```js
anno.on(event, callback);
```

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Value                                          |
|------------|------------------------------------------------|
| `event`    | the name of the event                          |
| `callback` | the function to call when the event is emitted |

### off

```js
anno.off(event [, callback]);
```

Unsubscribe from an event. If no callback is provided,
all event handlers for this event will be unsubscribed.

| Argument   | Value                                       |
|------------|---------------------------------------------|
| `event`    | the name of the event                       |
| `callback` | the function used when binding to the event |

## Events

### selectAnnotation

```js
anno.on('selectAnnotation', function(annotation) {
  // 
});
```

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.


| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

### createAnnotation

```js
anno.on('createAnnotation', function(annotation) {
  // 
});
```

Fired when a new annotation is created from a user selection.

| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

### updateAnnotation

```js
anno.on('updateAnnotation', function(annotation, previous) {
  // 
});
```

Fired when an existing annotation was updated.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the updated annotation                 |
| `previous`   | the annotation state before the update |

### deleteAnnotation

```js
anno.on('deleteAnnotation', function(annotation) {
  // 
});
```

Fired when an existing annotation was deleted.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the deleted annotation                 |

### mouseEnterAnnotation

```js
anno.on('mouseEnterAnnotation', function(annotation, event) {
  // 
});
```

Fired when the mouse moves into an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

### mouseLeaveAnnotation

```js
anno.on('mouseLeaveAnnotation', function(annotation, event) {
  // 
});
```

Fired when the mouse moves out of an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

