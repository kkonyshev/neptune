** This project is still under development but the editor is working **

> Neptune is ScalaJS port of [pell](https://github.com/jaredreich/pell), the simplest and smallest WYSIWYG text editor for web, with no dependencies.

Even the CSS is written in ScalaJS using [ScalaCSS](https://github.com/japgolly/scalacss)

Live demo: TBD

Live demo gif TBD

## Comparisons

| library       | size (min+gzip) | size (min) | jquery | bootstrap |
|---------------|-----------------|------------|--------|-----------|
| neptune       | 73kB            | 291kB      |        |           |
| pell          | 1.11kB          | 2.85kB     |        |           |
| medium-editor | 27kB            | 105kB      |        |           |
| quill         | 43kB            | 205kB      |        |           |
| ckeditor      | 163kB           | 551kB      |        |           |
| summernote    | 26kB            | 93kB       | x      | x         |
| froala        | 52kB            | 186kB      | x      |           |
| tinymce       | 157kB           | 491kB      | x      |           |

## Features

* Pure Scala, no dependencies
* Easily customizable with the Scala CSS DSL
* No separate CSS file, it's merged inside a single JS file and will be added to document automatically

Current actions:
- Bold
- Italic
- Underline

TBA:
- Strike-through
- Heading 1
- Heading 2
- Paragraph
- Quote
- Ordered List
- Unordered List
- Code
- Horizontal Rule
- Link
- Image

## Browser Support

* IE 9+
* Chrome 5+
* Firefox 4+
* Safari 5+
* Opera 11.6+

## Installation

#### SBT:

Install SBT and then

```bash
sbt fullOptJS
```
Find the generated optimized JS file at: `target/scala-2.11/neptune-opt.js`

#### HTML:

```html
<head>
  ...
  <style>
    /* override styles here */
    .pell-content {
      background-color: pink;
    }
  </style>
</head>
<body>
  ...
  <!-- Bottom of body -->
  <script src=".../neptune.js"></script>
</body>
```

## Usage

#### API

TODO

```js
// ES6
import pell from 'pell'
// or
import { exec, init } from 'pell'
```

```js
// Browser
pell
// or
window.pell
```

```js
// Initialize pell on an HTMLElement
pell.init({
  // <HTMLElement>, required
  element: document.getElementById('some-id'),

  // <Function>, required
  // Use the output html, triggered by element's `oninput` event
  onChange: html => console.log(html),

  // <boolean>, optional, default = false
  // Outputs <span style="font-weight: bold;"></span> instead of <b></b>
  styleWithCSS: false,

  // <Array[string | Object]>, string if overwriting, object if customizing/creating
  // action.name<string> (only required if overwriting)
  // action.icon<string> (optional if overwriting, required if custom action)
  // action.title<string> (optional)
  // action.result<Function> (required)
  // Specify the actions you specifically want (in order)
  actions: [
    'bold',
    {
      name: 'custom',
      icon: 'C',
      title: 'Custom Action',
      result: () => console.log('YOLO')
    },
    'underline'
  ],

  // classes<Array[string]> (optional)
  // Choose your custom class names
  classes: {
    actionbar: 'pell-actionbar',
    button: 'pell-button',
    content: 'pell-content'
  }
})

// Execute a document command, see reference:
// https://developer.mozilla.org/en/docs/Web/API/Document/execCommand
// this is just `document.execCommand(command, false, value)`
pell.exec(command<string>, value<string>)
```

TODO: 

#### List of overwriteable action names
- bold
- italic
- underline
- strikethrough
- heading1
- heading2
- paragraph
- quote
- olist
- ulist
- code
- line
- link
- image

#### Example

```html
<div id="pell"></div>
<div>
  Text output:
  <div id="text-output"></div>
  HTML output:
  <pre id="html-output"></pre>
</div>
```

```js
const editor = pell.init({
  element: document.getElementById('pell'),
  onChange: html => {
    document.getElementById('text-output').innerHTML = html
    document.getElementById('html-output').textContent = html
  },
  styleWithCSS: true,
  actions: [
    'bold',
    'underline',
    {
      name: 'italic',
      result: () => window.pell.exec('italic')
    },
    {
      name: 'custom',
      icon: '<b><u><i>C</i></u></b>',
      title: 'Custom Action',
      result: () => console.log('YOLO')
    },
    {
      name: 'image',
      result: () => {
        const url = window.prompt('Enter the image URL')
        if (url) window.pell.exec('insertImage', ensureHTTP(url))
      }
    },
    {
      name: 'link',
      result: () => {
        const url = window.prompt('Enter the link URL')
        if (url) window.pell.exec('createLink', ensureHTTP(url))
      }
    }
  ],
  classes: {
    actionbar: 'pell-actionbar-custom-name',
    button: 'pell-button-custom-name',
    content: 'pell-content-custom-name'
  }
})

// editor.content<HTMLElement>
// To change the editor's content:
editor.content.innerHTML = '<b><u><i>Initial content!</i></u></b>'
```

## License

MIT
