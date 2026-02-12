# run-gemini-cli

## overview

`run-gemini-cli` is a github action that integrates [gemini] into your development workflow via the [gemini cli]. it acts both as an autonomous agent for critical routine coding tasks, and an on-demand collaborator you can quickly delegate work to.

use it to perform github pull request reviews, triage issues, perform code analysis and modification, and more using [gemini] conversationally (e.g., `@gemini-cli fix this issue`) directly inside your github repositories.

- [run-gemini-cli](#run-gemini-cli)
  - [overview](#overview)
  - [features](#features)
  - [quick start](#quick-start)
    - [1. get a gemini api key](#1-get-a-gemini-api-key)
    - [2. add it as a github secret](#2-add-it-as-a-github-secret)
    - [3. update your .gitignore](#3-update-your-gitignore)
    - [4. choose a workflow](#4-choose-a-workflow)
    - [5. try it out!](#5-try-it-out)
  - [workflows](#workflows)
    - [gemini dispatch](#gemini-dispatch)
    - [issue triage](#issue-triage)
    - [pull request review](#pull-request-review)
    - [gemini cli assistant](#gemini-cli-assistant)
  - [configuration](#configuration)
    - [inputs](#inputs)
    - [outputs](#outputs)
    - [repository variables](#repository-variables)
    - [secrets](#secrets)
  - [authentication](#authentication)
    - [google authentication](#google-authentication)
    - [github authentication](#github-authentication)
  - [observability](#observability)
  - [extensions](#extensions)
  - [best practices](#best-practices)
  - [customization](#customization)
  - [contributing](#contributing)

## features

- **automation**: trigger workflows based on events (e.g. issue opening) or schedules (e.g. nightly).
- **on-demand collaboration**: trigger workflows in issue and pull request
  comments by mentioning the [gemini cli] (e.g., `@gemini-cli /review`).
- **extensible with tools**: leverage [gemini] models' tool-calling capabilities to
  interact with other clis like the [github cli] (`gh`).
- **customizable**: use a `gemini.md` file in your repository to provide
  project-specific instructions and context to [gemini cli].

## quick start

get started with gemini cli in your repository in just a few minutes:

### 1. get a gemini api key

obtain your api key from [google ai studio] with generous free-of-charge quotas

### 2. add it as a github secret

store your api key as a secret named `gemini_api_key` in your repository:

- go to your repository's **settings > secrets and variables > actions**
- click **new repository secret**
- name: `gemini_api_key`, value: your api key

### 3. update your .gitignore

add the following entries to your `.gitignore` file:

```gitignore
# gemini-cli settings
.gemini/

# github app credentials
gha-creds-*.json
```

### 4. choose a workflow

you have two options to set up a workflow:

**option a: use setup command (recommended)**

1. start the gemini cli in your terminal:

   ```shell
   gemini
   ```

2. in gemini cli in your terminal, type:

   ```
   /setup-github
   ```

**option b: manually copy workflows**

1. copy the pre-built workflows from the [`examples/workflows`](./examples/workflows) directory to your repository's `.github/workflows` directory. note: the `gemini-dispatch.yml` workflow must also be copied, which triggers the workflows to run.

### 5. try it out!

**pull request review:**

- open a pull request in your repository and wait for automatic review
- comment `@gemini-cli /review` on an existing pull request to manually trigger a review

**issue triage:**

- open an issue and wait for automatic triage
- comment `@gemini-cli /triage` on existing issues to manually trigger triaging

**general ai assistance:**

- in any issue or pull request, mention `@gemini-cli` followed by your request
- examples:
  - `@gemini-cli explain this code change`
  - `@gemini-cli suggest improvements for this function`
  - `@gemini-cli help me debug this error`
  - `@gemini-cli write unit tests for this component`

## workflows

this action provides several pre-built workflows for different use cases. each workflow is designed to be copied into your repository's `.github/workflows` directory and customized as needed.

### gemini dispatch

this workflow acts as a central dispatcher for gemini cli, routing requests to
the appropriate workflow based on the triggering event and the command provided
in the comment. for a detailed guide on how to set up the dispatch workflow, go
to the
[gemini dispatch workflow documentation](./examples/workflows/gemini-dispatch).

### issue triage

this action can be used to triage github issues automatically or on a schedule.
for a detailed guide on how to set up the issue triage system, go to the
[github issue triage workflow documentation](./examples/workflows/issue-triage).

### pull request review

this action can be used to automatically review pull requests when they are
opened. for a detailed guide on how to set up the pull request review system,
go to the [github pr review workflow documentation](./examples/workflows/pr-review).

### gemini cli assistant

this type of action can be used to invoke a general-purpose, conversational gemini
ai assistant within the pull requests and issues to perform a wide range of
tasks. for a detailed guide on how to set up the general-purpose gemini cli workflow,
go to the [gemini assistant workflow documentation](./examples/workflows/gemini-assistant).

## configuration

### inputs

<!-- prettier-ignore-start -->
<!-- begin_autogen_inputs -->

-   <a name="__input_gcp_location"></a><a href="#user-content-__input_gcp_location"><code>gcp_location</code></a>: _(optional)_ the google cloud location.

-   <a name="__input_gcp_project_id"></a><a href="#user-content-__input_gcp_project_id"><code>gcp_project_id</code></a>: _(optional)_ the google cloud project id.

-   <a name="__input_gcp_service_account"></a><a href="#user-content-__input_gcp_service_account"><code>gcp_service_account</code></a>: _(optional)_ the google cloud service account email.

-   <a name="__input_gcp_workload_identity_provider"></a><a href="#user-content-__input_gcp_workload_identity_provider"><code>gcp_workload_identity_provider</code></a>: _(optional)_ the google cloud workload identity provider.

-   <a name="__input_gcp_token_format"></a><a href="#user-content-__input_gcp_token_format"><code>gcp_token_format</code></a>: _(optional, default: `access_token`)_ the token format for authentication. set to "access_token" to generate access tokens (requires service account), or set to empty string for direct wif. can be "access_token" or "id_token".

-   <a name="__input_gcp_access_token_scopes"></a><a href="#user-content-__input_gcp_access_token_scopes"><code>gcp_access_token_scopes</code></a>: _(optional, default: `https://www.googleapis.com/auth/cloud-platform,https://www.googleapis.com/auth/userinfo.email,https://www.googleapis.com/auth/userinfo.profile`)_ the access token scopes when using token_format "access_token". comma-separated list of oauth 2.0 scopes.

-   <a name="__input_gemini_api_key"></a><a href="#user-content-__input_gemini_api_key"><code>gemini_api_key</code></a>: _(optional)_ the api key for the gemini api.

-   <a name="__input_gemini_cli_version"></a><a href="#user-content-__input_gemini_cli_version"><code>gemini_cli_version</code></a>: _(optional, default: `latest`)_ the version of the gemini cli to install. can be "latest", "preview", "nightly", a specific version number, or a git branch, tag, or commit. for more information, see [gemini cli releases](https://github.com/google-gemini/gemini-cli/blob/main/docs/releases.md).

-   <a name="__input_gemini_debug"></a><a href="#user-content-__input_gemini_debug"><code>gemini_debug</code></a>: _(optional)_ enable debug logging and output streaming.

-   <a name="__input_gemini_model"></a><a href="#user-content-__input_gemini_model"><code>gemini_model</code></a>: _(optional)_ the model to use with gemini.

-   <a name="__input_google_api_key"></a><a href="#user-content-__input_google_api_key"><code>google_api_key</code></a>: _(optional)_ the vertex ai api key to use with gemini.

-   <a name="__input_prompt"></a><a href="#user-content-__input_prompt"><code>prompt</code></a>: _(optional, default: `you are a helpful assistant.`)_ a string passed to the gemini cli's [`--prompt` argument](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/configuration.md#command-line-arguments).

-   <a name="__input_settings"></a><a href="#user-content-__input_settings"><code>settings</code></a>: _(optional)_ a json string written to `.gemini/settings.json` to configure the cli's _project_ settings.
    for more details, see the documentation on [settings files](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/configuration.md#settings-files).

-   <a name="__input_use_gemini_code_assist"></a><a href="#user-content-__input_use_gemini_code_assist"><code>use_gemini_code_assist</code></a>: _(optional, default: `false`)_ whether to use code assist for gemini model access instead of the default gemini api key.
    for more information, see the [gemini cli documentation](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/authentication.md).

-   <a name="__input_use_vertex_ai"></a><a href="#user-content-__input_use_vertex_ai"><code>use_vertex_ai</code></a>: _(optional, default: `false`)_ whether to use vertex ai for gemini model access instead of the default gemini api key.
    for more information, see the [gemini cli documentation](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/authentication.md).

-   <a name="__input_extensions"></a><a href="#user-content-__input_extensions"><code>extensions</code></a>: _(optional)_ a list of gemini cli extensions to install.

-   <a name="__input_upload_artifacts"></a><a href="#user-content-__input_upload_artifacts"><code>upload_artifacts</code></a>: _(optional, default: `false`)_ whether to upload artifacts to the github action.

-   <a name="__input_use_pnpm"></a><a href="#user-content-__input_use_pnpm"><code>use_pnpm</code></a>: _(optional, default: `false`)_ whether or not to use pnpm instead of npm to install gemini-cli

-   <a name="__input_workflow_name"></a><a href="#user-content-__input_workflow_name"><code>workflow_name</code></a>: _(optional, default: `${{ github.workflow }}`)_ the github workflow name, used for telemetry purposes.


<!-- end_autogen_inputs -->
<!-- prettier-ignore-end -->

### outputs

<!-- prettier-ignore-start -->
<!-- begin_autogen_outputs -->

-   <a name="__output_summary"></a><a href="#user-content-__output_summary"><code>summary</code></a>: the summarized output from the gemini cli execution.

-   <a name="__output_error"></a><a href="#user-content-__output_error"><code>error</code></a>: the error output from the gemini cli execution, if any.


<!-- end_autogen_outputs -->
<!-- prettier-ignore-end -->

### repository variables

we recommend setting the following values as repository variables so they can be reused across all workflows. alternatively, you can set them inline as action inputs in individual workflows or to override repository-level values.

| name                        | description                                                                                                                    | type     | required | when required                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------- | -------- | ------------------------------ |
| `gemini_debug`              | enables debug logging for the gemini cli.                                                                                      | variable | no       | never                          |
| `gemini_cli_version`        | controls which version of the gemini cli is installed.                                                                         | variable | no       | pinning the cli version        |
| `gcp_wif_provider`          | full resource name of the workload identity provider.                                                                          | variable | no       | using google cloud             |
| `google_cloud_project`      | google cloud project for inference and observability.                                                                          | variable | no       | using google cloud             |
| `service_account_email`     | google cloud service account email address. optional - only needed for wif with service account (not required for direct wif). | variable | no       | using wif with service account |
| `google_cloud_location`     | region of the google cloud project.                                                                                            | variable | no       | using google cloud             |
| `google_genai_use_vertexai` | set to `true` to use vertex ai                                                                                                 | variable | no       | using vertex ai                |
| `google_genai_use_gca`      | set to `true` to use gemini code assist                                                                                        | variable | no       | using gemini code assist       |
| `app_id`                    | github app id for custom authentication.                                                                                       | variable | no       | using a custom github app      |

> [!warning]
> do not use the `debug` environment variable as it causes the gemini cli to hang waiting for node debugger to attach.

to add a repository variable:

1. go to your repository's **settings > secrets and variables > actions > new variable**.
2. enter the variable name and value.
3. save.

for details about repository variables, refer to the [github documentation on variables][variables].

### secrets

you can set the following secrets in your repository:

| name              | description                                   | required | when required                         |
| ----------------- | --------------------------------------------- | -------- | ------------------------------------- |
| `gemini_api_key`  | your gemini api key from google ai studio.    | no       | you don't have a gcp project.         |
| `app_private_key` | private key for your github app (pem format). | no       | using a custom github app.            |
| `google_api_key`  | your google api key to use with vertex ai.    | no       | you have a express vertex ai account. |

to add a secret:

1. go to your repository's **settings > secrets and variables >actions > new repository secret**.
2. enter the secret name and value.
3. save.

for more information, refer to the
[official github documentation on creating and using encrypted secrets][secrets].

## authentication

this action requires authentication to both google services (for gemini ai) and the github api.

### google authentication

choose the authentication method that best fits your use case:

1. **gemini api key:** the simplest method for projects that don't require google cloud integration
2. **workload identity federation:** the most secure method for authenticating to google cloud services

### github authentication

you can authenticate with github in two ways:

1.  **default `github_token`:** for simpler use cases, the action can use the
    default `github_token` provided by the workflow.
2.  **custom github app (recommended):** for the most secure and flexible
    authentication, we recommend creating a custom github app.

for detailed setup instructions for both google and github authentication, go to the
[**authentication documentation**](./docs/authentication.md).

## observability

this action can be configured to send telemetry data (traces, metrics, and logs)
to your own google cloud project. this allows you to monitor the performance and
behavior of the [gemini cli] within your workflows, providing valuable insights
for debugging and optimization.

for detailed instructions on how to set up and configure observability, go to
the [observability documentation](./docs/observability.md).

## extensions

the gemini cli can be extended with additional functionality through extensions.
these extensions are installed from source from their github repositories.

for detailed instructions on how to set up and configure extensions, go to the
[extensions documentation](./docs/extensions.md).

## best practices

to ensure the security, reliability, and efficiency of your automated workflows, we strongly recommend following our best practices. these guidelines cover key areas such as repository security, workflow configuration, and monitoring.

key recommendations include:

- **securing your repository:** implementing branch and tag protection, and restricting pull request approvers.
- **workflow configuration:** using workload identity federation for secure authentication to google cloud, managing secrets effectively, and pinning action versions to prevent unexpected changes.
- **monitoring and auditing:** regularly reviewing action logs and enabling opentelemetry for deeper insights into performance and behavior.

for a comprehensive guide on securing your repository and workflows, please refer to our [**best practices documentation**](./docs/best-practices.md).

## customization

create a [gemini.md] file in the root of your repository to provide
project-specific context and instructions to [gemini cli]. this is useful for defining
coding conventions, architectural patterns, or other guidelines the model should
follow for a given repository.

## contributing

contributions are welcome! check out the gemini cli
[**contributing guide**](./contributing.md) for more details on how to get
started.

[secrets]: https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions
[settings_json]: https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/configuration.md#settings-files
[gemini]: https://deepmind.google/models/gemini/
[google ai studio]: https://aistudio.google.com/apikey
[gemini cli]: https://github.com/google-gemini/gemini-cli/
[google cloud support]: https://cloud.google.com/support
[variables]: https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-variables#creating-configuration-variables-for-a-repository
[github cli]: https://docs.github.com/en/github-cli/github-cli
[gemini.md]: https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/configuration.md#context-files-hierarchical-instructional-context
