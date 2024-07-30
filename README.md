# Flutter Bump Version

It automatically increases the version property of your project's pubspec.yaml on every merged Pull Request.

## How it works

1. When a Pull Request is merged, it triggers the action. It won't triggered on closed Pull Requests.
2. It will read the pubspec.yaml file and fetch your build number (the one after the '+').
3. It will increase the build number by 1.
4. It will update your version text (the one before the '+'), based on the new build number.
5. It will commit and push the changes to the specified branch.

## Examples

- If your version number is `1`, your version property will be updated to `0.0.02+2`.
- If your version number is `9`, your version property will be updated to `0.0.10+10`.
- If your version number is `10`, your version property will be updated to `0.0.11+11`.
- If your version number is `99`, your version property will be updated to `0.1.00+100`.
- If your version number is `100`, your version property will be updated to `0.1.01+101`.
- If your version number is `999`, your version property will be updated to `1.0.00+1000`.
- If your version number is `1000`, your version property will be updated to `1.0.01+1001`.
- If your version number is `9999`, your version property will be updated to `10.0.01+10001`.

## Usage

In your workflow file, add the following step:

```
- name: Bump Version
  uses: RodXander/flutter-bump-version@v0.0.02
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

Also, to set  when the action should be triggered, you can use the following example for when a PR is merged or manually triggered:

```
on:
    workflow_dispatch: #Trigger manually
    pull_request:
        types: [closed] #Trigger only on closed PRs
    
jobs:
    bump_version:
        if: github.event.pull_request.merged == true #Skip if the PR was not merged
        runs-on: ubuntu-latest
        
        steps:
            ...
```

You can use whatever condition works best for you.

## Inputs

- `branch`: Optional. The branch where the changes will be pushed. Default: `main`.
- `token`: Required. The GitHub token used to checkout and push. You should use `${{ secrets.GITHUB_TOKEN }}`, but in protected branches, you may need to create a Personal Access Token as shown [here](https://github.com/ad-m/github-push-action/blob/master/docs/personal-acces-token.md#creation-of-a-personal-access-token), set that token as a secret in your repository and pass it as input, like `${{ secrets.PAT_TOKEN }}`.