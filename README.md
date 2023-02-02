# Go coverage report

A GitHub Action to add a coverage report and badge to your Go repo.

Apply it to your repo by adding this step to one of your workflows:

```yaml
    - name: Update coverage report
      uses: ncruces/go-coverage-report@main
```

Your repo needs to have a Wiki for the action to work,
and workflows need to have read _and_ write permissions to the repo.

Also, consider:
- running this step _after_ your tests run
  - the step fails if any tests fail, so you might as well skip it they do
- running it only once
  - use a condition to avoid repeated matrix runs
- allowing it to fail without failing the entire job
  - PRs lack permission to update the badge, nor do you want them to


```yaml
    - name: Test
      run: go test -v ./...

    - name: Update coverage report
      uses: ncruces/go-coverage-report@main
      if: matrix.os == 'ubuntu-latest'
      continue-on-error: true
```

This action will generate an HTML report and SVG badge,
and save them as "hidden" files in your Wiki.

You can then apply them to your `README.md` with this Markdown snippet:

```markdown
[![Go Coverage](https://github.com/USER/REPO/wiki/coverage.svg)](https://raw.githack.com/wiki/USER/REPO/coverage.html)
```

## Use manually or as a pre-commit hook

The [script](coverage.sh) can also be run manually, or as a [pre-commit hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).

## Credits

- [@vieux](https://github.com/vieux/) for [gocover.io](https://github.com/vieux/gocover.io) which I've used for years before creating this
- [@Prounckk](https://github.com/Prounckk) for the [blog](https://eremeev.ca/posts/golang-test-coverage-github-action/) that prompted this solution
- [raw.githack.com](https://raw.githack.com/) for proxying the HTML report
- [shields.io](https://shields.io/) for SVG badge generation
