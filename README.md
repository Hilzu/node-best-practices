# Node best practices

Best practices when creating Node backends

## NODE_ENV

Always set `NODE_ENV` environment variable to some value. Acceptable values are `development`, `test` and `production`. No other values should be used. If you need to differentiate between different deployed environments use a different environment variable like `APP_ENV` for that. All deployed environments should have `NODE_ENV` set to production.

## .nvmrc

Create a `.nvmrc` file to specify what Node version your project is using. Keep the range open to allow updating to newer minor and patch releases. You might want to add your node version to the `engines` object in `package.json`.

    echo 6 > .nvmrc
    
## Use yarn and `yarn.lock` to handle dependencies

Use `yarn` to install and add dependecies to your project. You can ensure that your dependencies are up-to-date by just running `yarn` in your project folder without needing to run separate `npm install`, `npm update` and `npm prune` commands.

Check in `yarn.lock` file to your repo to ensure reproducable dependencies across machines. When using locked dependencies you should update the lock file regularly. You really want to have the latest minor and patch versions with bug fixes and security updates. You can use snyk to help you find vulnerable dependencies.

## Use JS features supported by Node

Use Node native CommonJS module system instead of ES2015 modules. Node doesn't have support for them and you shouldn't add a build to your backend just for them. Node has good support for most of other Javascript features and you can check the status of support at [node.green](http://node.green/). Using vanilla node makes debugging and development a lot smoother. You also get better stack traces with no extra effort.

## Use strict mode in all files

The flip-side of not using ES2015 modules is that you don't get strict mode for free. Strict mode needs to be enabled in all files with the `"use strict";` statement at the top. You can enforce strict mode usage with eslint and add strict mode statements using `strict-mode-cli`.

## EditorConfig

[EditorConfig](http://editorconfig.org/)

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

## Use eslint

## Use prettier

## npm scripts

## Configuration in package.json

