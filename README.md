# pre-commit-with-lint

It's a fork from pre-commit. uses as pre-commit to run scripts in package.json.

and add lint to lint files only changed in this commit when pre-commit, without lint all your css or js files.so you can speed up when you commit.

# how to use?

you need do nothing bug install this package.

```
  npm install pre-commit-with-lint --save-dev
```

default this package will use stylelint to lint changed file as ".(s?css)|(less)$", use eslint to lint changed file as ".jsx?$".

default setting like this.
```
{
  "pre-commit": {
    "lint": {
      "stylelint": ".(s?css)|(less)$",
      "eslint": ".jsx?$"
    }
  }
}
```

you can add other linter, the key is linter name, value is reg string.
```
{
  "pre-commit": {
    "tslint": ".ts$"
  }
}
```
you can disable lint by set to "false".

disable all linter
```
{
  "pre-commit": {
    "lint": "false"
  }
}
```
disable one linter
```
{
  "pre-commit": {
    "lint": {
      "eslint": "false"
    }
  }
}
```

if default linter no found, case warn.
but if the linter set in package.json not found, it case error, and prevent this commit.

disable run test
```
{
  "pre-commit": {
    "run": []
  }
}
```

# pre-commit

[![Version npm][version]](http://browsenpm.org/package/pre-commit)[![Build Status][build]](https://travis-ci.org/observing/pre-commit)[![Dependencies][david]](https://david-dm.org/observing/pre-commit)[![Coverage Status][cover]](https://coveralls.io/r/observing/pre-commit?branch=master)

[version]: http://img.shields.io/npm/v/pre-commit.svg?style=flat-square
[build]: http://img.shields.io/travis/observing/pre-commit/master.svg?style=flat-square
[david]: https://img.shields.io/david/observing/pre-commit.svg?style=flat-square
[cover]: http://img.shields.io/coveralls/observing/pre-commit/master.svg?style=flat-square

**pre-commit** is a pre-commit hook installer for `git`. It will ensure that
your `npm test` (or other specified scripts) passes before you can commit your
changes. This all conveniently configured in your `package.json`.

But don't worry, you can still force a commit by telling `git` to skip the
`pre-commit` hooks by simply committing using `--no-verify`.

### Installation

It's advised to install the **pre-commit** module as a `devDependencies` in your
`package.json` as you only need this for development purposes. To install the
module simply run:

```
npm install --save-dev pre-commit
```

To install it as `devDependency`. When this module is installed it will override
the existing `pre-commit` file in your `.git/hooks` folder. Existing
`pre-commit` hooks will be backed up as `pre-commit.old` in the same repository.

### Configuration

`pre-commit` will try to run your `npm test` command in the root of the git
repository by default unless it's the default value that is set by the `npm
init` script. 

But `pre-commit` is not limited to just running your `npm test`'s during the
commit hook. It's also capable of running every other script that you've
specified in your `package.json` "scripts" field. So before people commit you
could ensure that:

- You have 100% coverage
- All styling passes.
- JSHint passes.
- Contribution licenses signed etc.

The only thing you need to do is add a `pre-commit` array to your `package.json`
that specifies which scripts you want to have ran and in which order:

```js
{
  "name": "437464d0899504fb6b7b",
  "version": "0.0.0",
  "description": "ERROR: No README.md file found!",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: I SHOULD FAIL LOLOLOLOLOL \" && exit 1",
    "foo": "echo \"fooo\" && exit 0",
    "bar": "echo \"bar\" && exit 0"
  },
  "pre-commit": [
    "foo",
    "bar",
    "test"
  ]
}
```

In the example above, it will first run: `npm run foo` then `npm run bar` and
finally `npm run test` which will make the commit fail as it returns the error
code `1`.  If you prefer strings over arrays or `precommit` without a middle
dash, that also works:

```js
{
  "precommit": "foo, bar, test"
  "pre-commit": "foo, bar, test"
  "pre-commit": ["foo", "bar", "test"]
  "precommit": ["foo", "bar", "test"],
  "precommit": {
    "run": "foo, bar, test",
  },
  "pre-commit": {
    "run": ["foo", "bar", "test"],
  },
  "precommit": {
    "run": ["foo", "bar", "test"],
  },
  "pre-commit": {
    "run": "foo, bar, test",
  }
}
```

The examples above are all the same. In addition to configuring which scripts
should be ran you can also configure the following options:

- **silent** Don't output the prefixed `pre-commit:` messages when things fail
  or when we have nothing to run. Should be a boolean.
- **colors** Don't output colors when we write messages. Should be a boolean.
- **template** Path to a file who's content should be used as template for the
  git commit body.

These options can either be added in the `pre-commit`/`precommit` object as keys
or as `"pre-commit.{key}` key properties in the `package.json`:

```js
{
  "precommit.silent": true,
  "pre-commit": {
    "silent": true
  }
}
```

It's all the same. Different styles so use what matches your project. To learn
more about the scripts, please read the official `npm` documentation:

https://npmjs.org/doc/scripts.html

And to learn more about git hooks read:

http://githooks.com

### License

MIT
