# 🗑️ Neon Delete Branch by Name Action

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./docs/logos/neon-logo-dark.svg">
    <img alt="Neon logo" src="./docs/logos/neon-logo-light.svg">
  </picture>
</p>

This action deletes a specified Neon branch by its name within your Neon project.

It simplifies deleting Neon branches when you only know the branch name and not its ID. It internally retrieves the branch ID using the provided name and then utilizes the [Neon Delete Branch Action](https://github.com/neondatabase/delete-branch-action) to perform the deletion.

## Setup

Using the action requires adding a Neon API key to your GitHub Secrets. There are two ways you can perform this setup:

- **Using the Neon GitHub Integration** (recommended 👍) — this integration connects your Neon project to your GitHub repository, creates an API key, and sets the API key in your GitHub repository for you. See [Neon GitHub Integration](https://neon.tech/docs/guides/neon-github-integration) for instructions.
- **Manual setup** — this method requires obtaining a Neon API key and configuring it manually in your GitHub repository.

  1. **Obtain a Neon API key.** See [Create an API key](https://neon.tech/docs/manage/api-keys#create-an-api-key) for instructions on the Neon documentation.
  2. In your GitHub repository, go to **Settings** and locate **Secrets and variables** at the bottom of the left sidebar.
  3. Click **Actions** > **New repository secret**.
  4. Name the secret `NEON_API_KEY` and paste your API key in the **Value** field.
  5. Click **Add secret**.

## Usage

The following fields are required to run the Delete Branch by Name action:

- `project_id` — The Neon project ID. If you have the Neon GitHub Integration installed, you can specify `${{ vars.NEON_PROJECT_ID }}`. You can find the project ID of your Neon project on the Settings page of your Neon console.
- `api_key` — The Neon API key for your Neon project or organization. If you have the GitHub integration installed, specify `${{ secrets.NEON_API_KEY }}`.
- `branch_name` — The name of the Neon branch you want to delete.

Setup the action in your workflow:

```yml
steps:
  - uses: neondatabase/delete-branch-by-name-action@main
    with:
      project_id: your_neon_project_id
      branch_name: my-old-feature-branch # Specify branch name to delete
      api_key: ${{ secrets.NEON_API_KEY }}
```

Alternatively, you can use `${{ vars.NEON_PROJECT_ID }}` to get your `project_id`. If you have set up the [Neon GitHub Integration](https://neon.tech/docs/guides/neon-github-integration), the `NEON_PROJECT_ID` variable will be defined as a variable in your GitHub repository.

This action internally performs the following steps:

1.  **Retrieves Branch ID:** It uses the Neon API to find the branch ID associated with the provided `branch_name`.
2.  **Deletes Branch:** It then utilizes the [Neon Delete Branch Action](https://github.com/neondatabase/delete-branch-action) to delete the branch using the retrieved `branch_id`.

## Outputs

This action does not provide any outputs. The primary function is to delete a Neon branch by its name.

## Example Workflow

Here's an example of a complete GitHub Actions workflow that deletes a Neon branch by name:

```yml
name: Neon Github Actions Delete Branch by Name

on:
  # You can modify the following line to trigger the workflow on a different event, such as `push` or `pull_request`, as per your requirements. We have used `workflow_dispatch` for triggering the action in this example.
  workflow_dispatch:

jobs:
  Delete-Neon-Branch-By-Name:
    runs-on: ubuntu-24.04
    steps:
      - uses: neondatabase/delete-branch-by-name-action@main
        with:
          project_id: ${{ vars.NEON_PROJECT_ID }}
          branch_name: my-old-feature-branch # Replace with the branch name you want to delete
          api_key: ${{ secrets.NEON_API_KEY }}
      - run: echo "Branch 'my-old-feature-branch' deleted successfully"
```

---

## Other Actions

Check out other Neon GitHub Actions:

- [Create Branch Action](https://github.com/neondatabase/create-branch-action)
- [Delete Branch Action](https://github.com/neondatabase/delete-branch-action)
- [Reset Branch Action](https://github.com/neondatabase/reset-branch-action)
- [Schema Diff Action](https://github.com/neondatabase/schema-diff-action)
