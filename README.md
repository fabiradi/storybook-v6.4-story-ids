# Using `id` for story conflicts with `storyStoreV7`

This is a demo repository based on [Create React App](https://github.com/facebook/create-react-app) and [Storybook 6.4-RC3](https://github.com/storybookjs/storybook/issues/15355).

## `yarn storybook`

- Stortybook 6.4 (still) works great with an `id` set for a story to have a **permalink** that _will not change_ based on the location of the story (this may be essential for some use cases).

```javascript
// src/stories/Button.stories.tsx
export default {
  title: 'Example/Button',
  component: Button,
  id: 'mybutton', // ⚠️ This is important
} as ComponentMeta<typeof Button>
```

### How the ID is used

```javascript
// Generated story ID based on "title" (no "id" property set)
example-button--primary

// Story ID based on property "id"
mybutton--primary
```

### What is not working?

If you want to use the new [v7 story store](https://github.com/storybookjs/storybook/blob/next/MIGRATION.md#using-the-v7-store), the **manually set IDs** seem to be ignored.

```javascript
// .storybook/main.js
module.exports = {
  // ...
  features: {
    storyStoreV7: true, // ⚠️ activating this casues the conflict
  },
};
```

### What happens?

The `id` is not respected and falls back to something _derived_ from `title`.

```plain
Didn't find 'example-button--primary' in CSF file, this is unexpected
Error: Didn't find 'example-button--primary' in CSF file, this is unexpected
    at StoryStore.storyFromCSFFile (http://localhost:6006/vendors~main.iframe.bundle.js:15439:15)
    at StoryStore._callee2$ (http://localhost:6006/vendors~main.iframe.bundle.js:15410:56)
    at tryCatch (http://localhost:6006/vendors~main.iframe.bundle.js:96208:40)
    at Generator.invoke [as _invoke] (http://localhost:6006/vendors~main.iframe.bundle.js:96439:22)
    at Generator.next (http://localhost:6006/vendors~main.iframe.bundle.js:96264:21)
    at asyncGeneratorStep (http://localhost:6006/vendors~main.iframe.bundle.js:15130:103)
    at _next (http://localhost:6006/vendors~main.iframe.bundle.js:15132:194)
```
