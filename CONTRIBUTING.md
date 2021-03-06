# Contributing to rust-lpc82x

Thank you for considering to work on rust-lpc82x. This document will give you some pointers and explain the guidelines that you need to be follow.

## Opening issues

If you found a problem, please open an issue on the [GitHub repository]. If you're not sure whether you found a problem or not, just open an issue anyway. We'd rather close a few invalid issues than miss any real problems.

## Contributing changes

If you want to contribute a change to rust-lpc82x, please open a pull request on the [GitHub repository]. The best way to open a pull request is usually to just push a branch to your fork, then click the button that should appear near the top of your fork's GitHub page.

If you're having any problems with completing your change, feel free to open a pull request anyway and ask any questions there. We're happy to help with getting changes across the finish line.

Please note that all Rust code is automatically generated from an [SVD file] using [svd2rust]. Any manual changes to Rust code will not be accepted. If there's something wrong with the generated Rust code, that means either the SVD file or svd2rust are doing something wrong.

### Patching the SVD file

There's existing infrastructure for patching the SVD file on the fly in the [update script]. To fix an issue in the SVD file, create another patch (see the patches at the root of the repository for examples) and add it to the update script.

## Commit guidelines

We use [clog] to generate a changelog for each release. This is done automatically, using the commit messages as a data source. Therefore it is very imporant to write clear commit messages and tag them in a way that the tool can understand.

The rest of this section explains the rules for commit messages. Please don't be put off, if this seems overwhelming. As always, if you're unsure about anything, just send a pull request. [GitCop] and the reviewer will happily point out any problems.

Before we go into the rules, here's an example of a commit message:
```
feat: Implement a feature

This is the commit message body. It is optional and might consist of
multiple paragraphs.

Here's the message body's second paragraph. The next paragraph is going
to automatically close an issue, once the commit is merged into the
repository.

Closes #123456.
```

First, let's start with the first line, the header. It's the most important part of the commit, as it's used by [clog] to generate the changelog. For that reason, it's the most heavily regulated part:
- The header's purpose is to concisely summarize the changes made.
- It must be **at most 50 characters** long.
- It should be written in the **imperative voice**, as if you're commanding someone. Write "Add something", as opposed to "Adding something" or "Added something".
- It must begin with the type of commit, followed by a colon (e.g. "feat:" or "fix:"). The following types can be used:
  - **feat**: New functionality, or changes (not bug fixes) to existing functionality.
  - **fix**: Bug fixes
  - **docs**: Improvements to documentation
  - **style**: Code formatting, indentation, etc.
  - **refactor**: Changes to code that don't change what it does. Cleaning up, moving stuff around, etc.
  - **perf**: Performance improvements
  - **test**: Changes to test code
  - **chore**: Custodial work that isn't directly related to the code. Changes to the build system, etc.

The following rules apply to the message body:
- The messages body is optional, but should be added if the header and the commit diff by themselves don't explain why the commit is necessary.
- It should **provide context** for the commit and **explain its reasoning**. It doesn't need to restate things that are already obvious from the commit diff.
- Please be mindful of explanations on how the code works. Often, it makes more sense to add such explanations to the code itself, in the form of comments.
- The length limit for lines in the commit body is **72 characters**.
- If any issues should be closed once the commit is merged, this can be done automatically by adding something like "Closes #123456" to the commit. Be careful about not doing this accidentally.

That's it! If anything about this document is unclear, feel free to open an issue. If you have questions regarding a pull request that you're working on, just open the pull request and ask your questions there.

## Release Procedure

This section is intended for project maintainers only. It assumes that you can push to the repository (here called `upstream`, but primarily work on your own fork (`origin`),

1. Check out feature branch for the release (replace x.y.z with actual version)
```
$ git checkout -b release-x.y.z
```

2. Push feature branch to your fork (required for the next steps to work)
```
$ git push -u origin release-x.y.z
```

3. Update changelog (make manual changes as required)
```
$ clog --from-latest-tag -o CHANGELOG.md --major|--minor|--patch
```

4. Update versions in README.md, if version bump is major or minor

5. Do cargo-release dry run, review its actions
```
$ cargo release --level major|minor|patch --dry-run
```

6. Run cargo-release
```
$ cargo release --level major|minor|patch
```

7. Open pull request, review it, merge it

8. Push the tag that cargo-release created to `upstream`
```
git checkout master
git pull upstream master
git push --tag upstream master
```


[GitHub repository]: https://github.com/braun-robotics/rust-lpc82x
[SVD file]: http://ds.arm.com/media/resources/db/chip/nxp/lpc824m201jdh20/LPC82x.svd
[svd2rust]: https://crates.io/crates/svd2rust
[update script]: https://github.com/braun-robotics/rust-lpc82x/blob/master/scripts/update
[clog]: https://crates.io/crates/clog-cli
[GitCop]: https://gitcop.com/
