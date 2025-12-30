# fhdaisy

`fhdaisy` is a Python wrapper for [DaisyUI](https://daisyui.com/) that
brings its component classes to [FastHTML](https://www.fastht.ml/)
applications. Instead of manually writing HTML elements with DaisyUI
classes like `<button class="btn btn-primary">`, you use Python
components like `Btn('Click me', cls='-primary')`. The library follows a
consistent pattern: every DaisyUI CSS class becomes a title-case Python
component (e.g., `card` → `Card`, `alert` → `Alert`), and modifiers can
be shortened by dropping the redundant prefix (e.g., `btn-primary` →
`-primary`). Beyond the standard components, `fhdaisy.xtras` provides
helper functions for common patterns like accordions and forms, reducing
repetitive code while maintaining the flexibility to create custom
helpers for your specific needs.

## Usage

### Installation

Install latest from [pypi](https://pypi.org/project/fhdaisy/)

``` sh
$ pip install fhdaisy
```

### Docs

To learn to use this library and see examples, please refer to the [documentation](AnswerDotAI.github.io/fhdaisy).

A brief overview of the project is available in this readme, but it is not a replacement for the docs.

## Brief overview

### DaisyUI

DaisyUI is a component library built on top of Tailwind CSS that
provides semantic class names for common UI patterns. While Tailwind
offers utility classes like `bg-blue-500` or `p-4`, DaisyUI adds
higher-level component classes like `btn`, `card`, and `modal` that
encapsulate the many utility classes typically needed for these
elements. This means you can create a styled button with
`class="btn btn-primary"` instead of combining dozens of Tailwind
utilities.

For FastHTML developers, DaisyUI is particularly useful because it
provides a complete design system out of the box without requiring
JavaScript frameworks. The components are purely CSS-based and work
perfectly with FastHTML’s server-side rendering approach. You get
professional-looking components with built-in themes, responsive design,
and accessibility features, while maintaining the simplicity of working
with HTML elements.

### Daisy basics

``` python
from fasthtml.common import *
from fasthtml.jupyter import *
from fhdaisy import *
```
**Important note**: Make sure you import `fhdaisy` after `fasthtml`. Otherwise `fasthtml` will overwrite the components and `Link()` will render to `<link></link>` in HTML.

Let’s look at a specific example of a DaisyUI component - the `btn`.

DaisyUI creates buttons by using a `btn` class, along with other special
classes prefixed by `btn-`, eg:

``` python
c = Button('Hey there', cls='btn btn-primary')
print(c)
```

### fhdaisy basics

Now let’s see how fhdaisy makes this simpler and more Pythonic. Instead
of using generic HTML elements with DaisyUI classes, fhdaisy provides
Python components that correspond directly to DaisyUI’s component
classes. The key insight is that DaisyUI’s CSS class names already tell
us what kind of component we’re creating - so why not use them as Python
component names?

Here’s how it works: fhdaisy takes each DaisyUI base class (like `btn`,
`card`, `alert`) and turns it into a Python component with a title-case
name (`Btn`, `Card`, `Alert`). When you use `Btn()` in your code,
fhdaisy automatically creates a button element with the `btn` class
already applied.

While the component names in fhdaisy come from DaisyUI’s CSS classes,
the underlying HTML elements are automatically chosen to match what
DaisyUI requires. Each component knows its correct HTML tag - `Btn`
creates a `<button>`, `Alert` creates a `<div>`, `Input` creates an
`<input>`, and so on. This mapping follows DaisyUI’s own documentation
and ensures your components work correctly with all of DaisyUI’s styling
and behavior.

Additional convenience comes with modifiers. In DaisyUI, you style a
button by adding classes like `btn-primary`, `btn-outline`, or `btn-sm`.
Notice how they all repeat the `btn-` prefix? With fhdaisy, you can use
a shorthand notation in the `cls` parameter: just write `-primary`,
`-outline`, or `-sm`. The component knows it’s a `Btn`, so it
automatically adds the `btn-` prefix back when generating the HTML. This
means less typing, less repetition, and cleaner code that’s easier to
read and maintain.

Let’s see this in action with the same `btn` as before:

``` python
c = Btn('Hey there', cls='-primary')
print(c)
```

    <button class="btn btn-primary ">Hey there</button>

This renders identically to the previous manual version.

With fhdaisy, the pattern is consistent: use the title-case version of
the DaisyUI class name as your component. So `input` becomes `Input`;
fhdaisy automatically adds the base `input` class when you use the
`Input` component. Any modifiers like `input-bordered` can be shortened
to just `-bordered` in the `cls` parameter.

