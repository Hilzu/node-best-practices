# Node best practices

Best practices when creating Node backends.

## Node version

You should generally use the latest LTS version (even version number) or the latest stable (odd version number) if you can update quickly. You can check the LTS schedule and process at [nodejs/LTS](https://github.com/nodejs/LTS#lts-schedule1).

Create a `.nvmrc` file to specify what Node version your project is using. Keep the range open to allow updating to newer minor and patch releases. You might also want to add your node version to the `engines` object in `package.json`.

    echo 6 > .nvmrc

## Use yarn and `yarn.lock` to handle dependencies

Use `yarn` to install and add dependecies to your project. You can ensure that your dependencies are up-to-date by just running `yarn` in your project folder without needing to run separate `npm install`, `npm update` and `npm prune` commands.

Check in `yarn.lock` file to your repo to ensure reproducable dependencies across machines. When using locked dependencies you should update the lock file regularly. You really want to have the latest minor and patch versions with bug fixes and security updates. You can use [Snyk](https://snyk.io/) to help you find vulnerable dependencies.

Properly evaluate all new dependecies before adding them. The scoring system of [npms](https://npms.io/) is one way to assess if a package can be depended upon.

Don't depend on global packages and add them to your project. For example you shouldn't expect developers to have a global grunt installation.

## NODE_ENV

Always set `NODE_ENV` environment variable to one of `development`, `test` or `production`. No other values should be used. If you need to differentiate between different deployed environments use a different environment variable like `APP_ENV` for that. All deployed environments should have `NODE_ENV` set to production.

## Use JS features supported by Node

Use Node native CommonJS module system instead of ES2015 modules. Node doesn't have support for them and you shouldn't add a build to your backend just for them. Node has good support for most of other Javascript features and you can check the status of support at [node.green](http://node.green/). Using vanilla node makes debugging and development a lot smoother. You also get better stack traces with no extra effort.

## Use strict mode in all files

The flip-side of not using ES2015 modules is that you don't get strict mode for free. Strict mode needs to be enabled in all files with the `"use strict";` statement at the top. You can enforce strict mode usage with eslint and add strict mode statements using [use-strict-cli](https://github.com/philidem/use-strict-cli). Don't use the `--strict_mode` CLI flag to Node as that enables strict mode also for external dependencies.

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

Use [ESLint](http://eslint.org/) to catch common mistakes. The [`eslint:recommended`](http://eslint.org/docs/rules/) ruleset makes for a good starting point.

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

Use [prettier](https://github.com/prettier/prettier) for consistent code style in your project. Prettier automatically formats code and removes the need for code style guides and bikeshedding related to it. You can enforce usage of prettier style with [`eslint-plugin-prettier`](https://github.com/not-an-aardvark/eslint-plugin-prettier).
