# suppress-eslint-errors

Have you ever tried to turn on a new eslint rule only to be discouraged by hundreds or thousands of violations in an existing codebase?
So have we.

Sometimes, there isn't a great business case for updating all of the existing (working!) code, especially in a larger, legacy codebase.
For those times, `suppress-eslint-errors` has you covered.

## How it works

`suppress-eslint-errors` is a codemod for [jscodeshift](https://github.com/facebook/jscodeshift) that runs eslint against your existing code.
For each violation it finds, it adds a little snippet:

```javascript
// TODO: Fix this the next time the file is edited.
// eslint-disable-next-line cool-new-rule
```

This way, you can get the benefits of the rule in new code without having to immediately work through a huge backlog.

## Usage

`suppress-eslint-errors` comes with a wrapper script so that you can call it directly without installing anything extra:

```bash
npx suppress-eslint-errors [jscodeshift options] PATH...
```

The wrapper will call `jscodeshift` with the transformer and any other arguments that you pass to it.
If it detects a `.gitignore` in the local directory, it will also specify that as the `--ignore-config`.

`suppress-eslint-errors` must be used with a locally installed copy of `eslint`.
If it can't find one, it will bail out early.

In some cases, the code produced by this codemod will make some unfortunate indentation decisions.
Be sure to re-run any code formatting tools you use before committing!

## Options

**--message**: Sets the comment to add above eslint-disable-next-line comments.

**--rules**: Comma-separated list of ESLint rule IDs to disable. When specified, violations of rules not in this set will be left in place.

## Examples

Suppress all rule violations in the `index.js` file, using a custom comment:

```bash
npx suppress-eslint-errors ./index.js --message="TODO: Issue #123"
```

Suppress violations of the `eqeqeq` and `@typescript-eslint/no-explicit-any` rules in .ts and .tsx files in the `src` directory:

```bash
npx suppress-eslint-errors ./src --extensions=ts,tsx --parser=tsx --rules=eqeqeq,@typescript-eslint/no-explicit-any
```

## Is it perfect?

Definitely not. PRs are welcome for any edge cases that you encounter.
