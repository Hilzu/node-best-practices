# Node best practices

Best practices for creating Node backends.

## Node version

You should generally use the latest LTS version (even version numbers) of Node,
or the latest stable version (odd version numbers) if you can update quickly.

You can check the LTS schedule and process at [nodejs/LTS](https://github.com/nodejs/LTS#lts-schedule1).

Create a `.nvmrc` file to specify what Node version your project is using.
Keep the range open to allow updating to newer minor and patch releases.
You might also want to add your Node version to the `engines` object in `package.json`.

    echo 6 > .nvmrc

## Adhere to twelve-factor principles

To create scalable services you should adhere to the [twelve-factor principles](https://12factor.net/) when possible.
This is doubly more important with Node because scaling mostly happens by adding more instances.

One of the 12-factor principles is keeping configuration in environment variables.
Locally you should use [`dotenv`](https://github.com/motdotla/dotenv) to manage your environment variables.

## Use yarn and `yarn.lock` to handle dependencies

Use [`yarn`](https://yarnpkg.com/en/) to manage your project's dependencies.
You can ensure that your dependencies are up-to-date by just running `yarn` in your project folder without needing to run separate `npm install`, `npm update`, `npm dedupe` and `npm prune` commands.

Check the `yarn.lock` file in to your repo to ensure reproducable dependencies across machines.
When doing that you should update the lock file regularly.
You really want to have the latest minor and patch versions with bug fixes and security updates.
You can use [Snyk](https://snyk.io/) to help you find vulnerable dependencies.

Properly evaluate all new dependecies before adding them.
The scoring system of [npms](https://npms.io/) is one way to assess if a package can be depended upon.
The score is based on quality, popularity, maintenance and personalities.
See the [npms documentation](https://github.com/npms-io/npms-analyzer/blob/master/docs/architecture.md#evaluators) for more information and to learn how to evaluate packages yourself.

## NODE_ENV

Always set the `NODE_ENV` environment variable to one of `development`, `test` or `production`. No other values should be used.
If you need to differentiate between different deployed environments use a different environment variable like `APP_ENV`.
All deployed environments should have `NODE_ENV` set to `production`.

## Only use JavaScript features supported by Node

Use the CommonJS module system native to Node instead of ES2015 modules.
Node doesn't have support for ES2015 modules and you shouldn't add a build step to your backend just for module support.
Node 6+ has good support for most of other Javascript features – you can check the status of support at [node.green](http://node.green/).
Using vanilla Node makes debugging and development a lot smoother. You also get better stack traces with no extra effort.

## Use strict mode in all files

The flip-side of not using ES2015 modules is that you don't get strict mode for free.
Strict mode needs to be enabled in all files with the `"use strict";` statement at the top.

You can enforce strict mode usage with ESLint and add strict mode statements using [use-strict-cli](https://github.com/philidem/use-strict-cli).

Don't use Node's `--strict_mode` flag, as that enables strict mode also for external dependencies (which may not be `use strict` compatible).

## EditorConfig

Use [EditorConfig](http://editorconfig.org/) for editor independent configuration.

```ini
# .editorconfig
root = true

[*]
# Change these settings to your own preference
indent_style = space
indent_size = 2

# We recommend you to keep these unchanged
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

## Use ESLint

Use [ESLint](http://eslint.org/) to catch common mistakes.
The [`eslint:recommended`](http://eslint.org/docs/rules/) ruleset makes for a good starting point.

```js
{
  "parserOptions": {
    "ecmaVersion": 2015,
    "sourceType": "script"
  },
  "env": {
    "node": true
  },
  "extends": "eslint:recommended",
  "rules": {
    "strict": "error"
  }
}
```

## Use prettier

Use [prettier](https://github.com/prettier/prettier) for consistent code style in your project.
Prettier automatically formats code and removes the need for code style guides and bikeshedding related to it.
You can enforce usage of prettier style with [`eslint-plugin-prettier`](https://github.com/not-an-aardvark/eslint-plugin-prettier).
