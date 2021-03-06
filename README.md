# grunt-bump

> Bump package version, create tag, commit, push ...

## Getting Started
This plugin requires Grunt.

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-bump --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-bump');
```

### Configuration
In your project's Gruntfile, add a section named `bump` to the data object passed into `grunt.initConfig()`. The options (and defaults) are:

```js
grunt.initConfig({
  bump: {
    options: {
      files: ['package.json'],
      updateConfigs: [],
      commit: true,
      commitMessage: 'Release v%VERSION%',
      commitFiles: ['package.json'],
      createTag: true,
      tagName: 'v%VERSION%',
      tagMessage: 'Version %VERSION%',
      push: true,
      pushTo: 'upstream',
      gitDescribeOptions: '--tags --always --abbrev=1 --dirty=-d',
      globalReplace: false
    }
  },
})
```

### Options

#### options.files
Type: `Array`
Default value: `['package.json']`

Maybe you wanna bump 'component.json' instead? Or maybe both: `['package.json', 'component.json']`? Can be either a list of files to bump (an array of files) or a grunt glob (e.g., `['*.json']`).

Grunt-bump will scan these files searching for instances of the version-\<separator\>-\<value\> pattern where _version_ is case-insensitive, _separator_ can be either colon or equals sign and _value_ is a string following the [semantic versioning](http://semver.org) convention.  The fact the equals sign is also supported means you are not limited only to JSON files but you can use other file formats to bump versions as well.

#### options.updateConfigs
Type: `Array`
Default value: `[]`

Sometimes you load the content of `package.json` into a grunt config. This will update the config property, so that even tasks running in the same grunt process see the updated value.

```js
bump: {
  files:         ['package.json', 'component.json'],
  updateConfigs: ['pkg',          'component']
}
```

#### options.commit
Type: `Boolean`
Default value: `true`

Should the changes be committed? False if you want to do additional things.

#### options.commitMessage
Type: `String`
Default value: `Release v%VERSION%`

If so, what is the commit message ? You can use `%VERSION%` which will get replaced with the new version.

#### options.commitFiles
Type: `Array`
Default value: `['package.json']`

An array of files that you want to commit. You can use `['-a']` to commit all files.

#### options.createTag
Type: `Boolean`
Default value: `true`

Create a Git tag?

#### options.tagName
Type: `String`
Default value: `v%VERSION%`

If `options.createTag` is set to true, then this is the name of that tag (`%VERSION%` placeholder is available).

#### options.tagMessage
Type: `String`
Default value: `Version %VERSION%`

If `options.createTag` is set to true, then yep, you guessed right, it's the message of that tag - description (`%VERSION%` placeholder is available).

#### options.push
Type: `Boolean`
Default value: `true`

Push the changes to a remote repo?

#### options.pushTo
Type: `String`
Default value: `upstream`

If `options.push` is set to true, which remote repo should it go to?

#### options.gitDescribeOptions
Type: `String`
Default value: `--tags --always --abbrev=1 --dirty=-d`

Options to use with `$ git describe`

#### options.globalReplace
Type: `Boolean`
Default value: `false`

Replace all occurrences of the version in the file. When set to `false`, only the first occurrence will be replaced.

### Usage Examples

Let's say current version is `0.0.1`.

```bash
$ grunt bump
>> Version bumped to 0.0.2
>> Committed as "Release v0.0.2"
>> Tagged as "v0.0.2"
>> Pushed to origin

$ grunt bump:patch
>> Version bumped to 0.0.3
>> Committed as "Release v0.0.3"
>> Tagged as "v0.0.3"
>> Pushed to origin

$ grunt bump:minor
>> Version bumped to 0.1.0
>> Committed as "Release v0.1.0"
>> Tagged as "v0.1.0"
>> Pushed to origin

$ grunt bump:major
>> Version bumped to 1.0.0
>> Committed as "Release v1.0.0"
>> Tagged as "v1.0.0"
>> Pushed to origin

$ grunt bump:prerelease
>> Version bumped to 1.0.0-1
>> Committed as "Release v1.0.0-1"
>> Tagged as "v1.0.0-1"
>> Pushed to origin

$ grunt bump:patch
>> Version bumped to 1.0.1
>> Committed as "Release v1.0.1"
>> Tagged as "v1.0.1"
>> Pushed to origin

$ grunt bump:git
>> Version bumped to 1.0.1-ge96c
>> Committed as "Release v1.0.1-ge96c"
>> Tagged as "v1.0.1-ge96c"
>> Pushed to origin
````

If you want to jump to an exact version, you can use the ```setversion``` tag in the command line.

```bash
$ grunt bump --setversion=2.0.1
>> Version bumped to 2.0.1
>> Committed as "Release v2.0.1"
>> Tagged as "v2.0.1"
>> Pushed to origin
```

Sometimes you want to run another task between bumping the version and committing, for instance generate changelog. You can use `bump-only` and `bump-commit` to achieve that:

```bash
$ grunt bump-only:minor
$ grunt changelog
$ grunt bump-commit
```

## Contributing
See the [contributing guide](https://github.com/vojtajina/grunt-bump/blob/master/CONTRIBUTING.md) for more information. In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/): `grunt test jshint`.

## License
Copyright (c) 2014 Vojta Jína. Licensed under the MIT license.
