<img width="1971" alt="Screnshot of `html` with syntax highlighting in VSCode" src="https://user-images.githubusercontent.com/1316441/219116113-2981f5f5-a95b-47d9-83ee-7b9f1bc6eb7d.png">


# `html`: JSX-Like Syntax for Tagged Template Literals

A couple blog posts detailing the rationale for an using tagged template literals:

- [Templating in JavaScript, From Zero Dependencies on Up](https://blog.jim-nielsen.com/2021/javascript-templating/)
- [JSX-Like Syntax for Tagged Template Literals in JavaScript](https://blog.jim-nielsen.com/2019/jsx-like-syntax-for-tagged-template-literals/)
- [Static Site Templating: Switching from React & JSX to JavaScript & Template Literals](https://blog.jim-nielsen.com/2020/switching-from-react-to-js-for-templating/)

## Usage

Copy/paste the contents of `html.js` into your own codebase.

Or, install it in your project (this isn't published on NPM, but you _can_ install directly from GitHub or pull it in from a service like jsdeliver):

- Node: `npm i jimniels/html`
- Browser: `import html from "https://cdn.jsdelivr.net/gh/jimniels/html@0.2.0/html.js"`

## Why?

Sometimes you just don’t need a bespoke templating languages with its quirks and dependneices. Just Use JavaScript™️.

Example trade-offs of using template literals over JSX:

- No more trying to remember which custom attributes require their own spcial syntax. Just use the platform standard names. For example:
  - `class=""` instead of `className=""`
  - `for=""` instead of `htmlFor=""`
  - `style="font-weight: 600"` isntead of `style={{ fontWeight: 600 }}`
  - Use standard SVG attributes too without worry! e.g. `xlink:href` instead of `xlinkHref`
- Render more than just HTML without a problem: SVG, JSON, whatever! It’s all just text. No need to try and do [special fragment workarounds](https://github.com/facebook/react/issues/12014#issuecomment-454869517).
- No `dangerouslySetInnerHTML` (in a sense, everything in template literals is `dangerouslySetInnerHTML`, you just escape the things you know are unsafe).
- Do templating in the script tags in your HTML, e.g. `<script>var t = "${myVar}";</script>`

### Examples

Everything in template literals is an expression. The tagged `html` template literal will handle coercing JSX-like expressions into the correct strings for you.

Render a string or number:

```js
const props = { title: "Hello World" };

const Component = (props) => html`
  <h1>${props.title}</h1>
`;

Component(props);
// -> "<h1>Hello World</h1>"
```

**Note**: line breaks and indents will appear in multi-line strings. There are [libraries to help with this](https://www.npmjs.com/package/dedent) if you need them.

“Components” are merely functions that take arguments and return strings:

```js
const H1 = ({ text }) => html`
  <h1>${text}</h1>
`;

const H2 = ({ text }) => html`
  <h2>${text}</h2>
`;

const out = `<!doctype html>
  <html>
    <body>
      ${H1({ text: "Hello" })}
      ${H1({ text: "World" })}
    </body>
  </html>
`;

// -> "<!doctype html><html><body><h1>Hello</h1><h2>World</h2></body></html>"
```

Use conditionals and arrays just like in JSX too!

```js
const props = {
  activePath: "/about/",
  navItems: [
    { path: "/", title: "Home" },
    { path: "/about/", title: "About" },
    { path: "/tags/", title: "Tags" }
  ]
};

const Navigation = ({ activePath, navItems }) => html`
  <ul class="navigation">
    ${navItems.map(({ path, title }) => html`
      <li>
        <a
          class="${activePath === path && 'active'}"
          href="${path}">
          ${title}
        </a>
      </li>
    `)}
  </ul>
`;

Navigation(props);
// -> <ul class="navigation"><li><a class="" href="/">Home</a></li>...</ul>
```

And make sure you [get syntax highlighting](https://github.com/mjbvz/vscode-lit-html) for the HTML embedded in template literals.

[Read the original blog post for more details](https://blog.jim-nielsen.com/2019/jsx-like-syntax-for-tagged-template-literals/).
