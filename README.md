<div align="center">
<h1>s40-scripts 🛠📦</h1>

<p>CLI toolbox for common scripts used by the s40 project.</p>
</div>

---

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Installation](#installation)
- [Usage](#usage)
  - [Overriding Config](#overriding-config)
  - [TypeScript Support](#typescript-support)
- [Inspiration](#inspiration)
- [Other Solutions](#other-solutions)
- [Issues](#issues)
  - [🐛 Bugs](#-bugs)
  - [💡 Feature Requests](#-feature-requests)
- [Contributors ✨](#contributors-)
- [LICENSE](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and
should be installed as one of your project's `devDependencies`:

```
npm install --save-dev s40-scripts
```

## Usage

This is a CLI and exposes a bin called `s40-scripts`. I don't really plan on
documenting or testing it super duper well because it's really specific to my
needs. You'll find all available scripts in `src/scripts`.

This project actually dogfoods itself. If you look in the `package.json`, you'll
find scripts with `node src {scriptName}`. This serves as an example of some of
the things you can do with `s40-scripts`.

### Overriding Config

Unlike `react-scripts`, `s40-scripts` allows you to specify your own
configuration for things and have that plug directly into the way things work
with `s40-scripts`. There are various ways that it works, but basically if you
want to have your own config for something, just add the configuration and
`s40-scripts` will use that instead of it's own internal config. In addition,
`s40-scripts` exposes its configuration so you can use it and override only the
parts of the config you need to.

This can be a very helpful way to make editor integration work for tools like
ESLint which require project-based ESLint configuration to be present to work.

So, if we were to do this for ESLint, you could create an `.eslintrc` with the
contents of:

```
{"extends": "./node_modules/s40-scripts/eslint.js"}
```

> Note: for now, you'll have to include an `.eslintignore` in your project until
> [this eslint issue is resolved](https://github.com/eslint/eslint/issues/9227).

Or, for `babel`, a `.babelrc` with:

```
{"presets": ["s40-scripts/babel"]}
```

Or, for `jest`:

```javascript
const {jest: jestConfig} = require('s40-scripts/config')
module.exports = Object.assign(jestConfig, {
  // your overrides here

  // for test written in Typescript, add:
  transform: {
    '\\.(ts|tsx)$': '<rootDir>/node_modules/ts-jest/preprocessor.js',
  },
})
```

> Note: `s40-scripts` intentionally does not merge things for you when you start
> configuring things to make it less magical and more straightforward. Extending
> can take place on your terms. I think this is actually a great way to do this.

### TypeScript Support

If the `tsconfig.json`-file is present in the project root directory and
`typescript` is a dependency the `@babel/preset-typescript` will automatically
get loaded when you use the default babel config that comes with `s40-scripts`.
If you customized your `.babelrc`-file you might need to manually add
`@babel/preset-typescript` to the `presets`-section.

`s40-scripts` will automatically load any `.ts` and `.tsx` files, including the
default entry point, so you don't have to worry about any rollup configuration.

If you have a `typecheck` script (normally set to `s40-scripts typecheck`) that
will be run as part of the `validate` script (which is run as part of the
`pre-commit` script as well).

TypeScript definition files will also automatically be generated during the
`build` script.

## Inspiration

This is inspired by `react-scripts`.

## Other Solutions

If you are aware of any please [make a pull request][prs] and add it here!
Again, this is a very specific-to-me solution.

- [Rollpkg](https://github.com/rafgraph/rollpkg) - convention over config build
  tool to create packages with TypeScript and Rollup.

### 🐛 Bugs

Please file an issue for bugs, missing documentation, or unexpected behavior.

[**See Bugs**][bugs]

## LICENSE

MIT
