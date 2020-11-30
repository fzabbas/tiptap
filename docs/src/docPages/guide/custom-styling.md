# Custom styling

## toc

## Introduction
tiptap is headless, that means there is no styling provided. That also means, you are in full control of how your editor looks. The following methods allow you to apply custom styles to the editor.

## Option 1: Style the plain HTML
The whole editor is rendered inside of a container with the class `.ProseMirror`. You can use that to scope your styling to the editor content:

```css
/* Scoped to the editor */
.ProseMirror p {
  margin: 1em 0;
}
```

If you’re rendering the stored content somewhere, there won’t be a `.ProseMirror` container, so you can just globally add styling to the used HTML tags:

```css
/* Global styling */
p {
  margin: 1em 0;
}
```


## Option 2: Add custom classes
Most extensions have a `class` option, which you can use to add a custom CSS class to the HTML tag.

Most extensions allow you to add attributes to the rendered HTML through the `HTMLAttributes` configuration. You can use that to add a custom class (or any other attribute):

```js
new Editor({
  extensions: [
    Document,
    Paragraph.configure({
      HTMLAttributes: {
        class: 'my-custom-paragraph',
      },
    }),
    Heading.configure({
      HTMLAttributes: {
        class: 'my-custom-heading',
      },
    }),
    Text,
  ]
})
```

The rendered HTML will look like that:

```html
<h1 class="my-custom-heading">Example Text</p>
<p class="my-custom-paragraph">Wow, that’s really custom.</p>
```

If there are already classes defined by the extensions, your classes will be added.

## Option 3: Customize the HTML
You can even customize the markup for every extension. This will make a custom bold extension that doesn’t render a `<strong>` tag, but a `<b>` tag:

```js
import Bold from '@tiptap/extension-bold'

const CustomBold = Bold.extend({
  renderHTML({ HTMLAttributes }) {
    // Original:
    // return ['strong', HTMLAttributes, 0]
    return ['b', HTMLAttributes, 0]
  },
})

new Editor({
  extensions: [
      // …
      CustomBold(),
  ]
})
```

You should put your custom extensions in separate files though, but I think you got the idea.