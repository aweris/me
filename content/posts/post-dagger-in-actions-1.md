---
title: "Dagger in Actions - Run GitHub Actions in Dagger"
image: "images/post/post-dagger-in-actions-1.png"
author: "Ali AKCA"
date: 2023-01-08
description: "Dummy Blog Post to show pagination styling." 
categories: ["Github Actions", "Dagger", "Gale"]
tags: ["github", "dagger", "gale"]
featured: false
---

## Introduction

Since its debut in 2018, GitHub Actions has emerged as a cornerstone in GitHub's software development lifecycle. More than just a CI/CD solution, it facilitates automation via event-driven triggers. This unique feature offers unparalleled flexibility, allowing for endless automation scenarios, all while remaining tightly integrated with the rest of the GitHub ecosystem.

GitHub Actions is a powerful tool that can significantly enhance a developer's workflow. It automates mundane tasks such as building, linting, testing, etc., and it enables the deployment of complex applications. Its seamless integration into GitHub means that developers don't have to juggle multiple tools, streamlining their workflow. Additionally, the platform has strong community support, with a wide range of pre-built actions available in its marketplace. This makes it easy to automate tasks and integrate CI/CD into projects. According to Github's [the state of the Octoverse report 2023](https://github.blog/2023-11-08-the-state-of-open-source-and-ai/#developer-activity-as-a-bellwether-of-new-tech-adoption), on average, developers used more than 20 million GitHub Actions minutes a day in public projects and the community keeps growing with the number of GitHub Actions in the GitHub Marketplace passing the 20,000 mark in 2023.

## The Problem

### The Limitations of Remote-Only Workflow Execution in GitHub Actions

While GitHub Actions brings automation and CI/CD seamlessly into the GitHub platform, the primary drawback is the inability to execute workflows locally. This limitation has implications:

1. **Marketplace Dilemma - Local Scripts or GitHub Actions**: Developers are confronted with the decision to either use GitHub Marketplace actions, which requires additional effort to run those steps locally, or to use local scripts in Github Actions Workflows, thereby foregoing the benefits of the Marketplace. This situation often results in a compromise between maintaining consistent workflows across environments and taking full advantage of the distinct features each offers.

2. **Cost of Time and Remote Validation:**: Running workflows on GitHub consumes Actions minutes and requires pushing changes to remote repository. Testing some triggers can be quite challenging due to the specific triggering conditions required for the events to occur. This process also extends the feedback loop, as developers must commit, push, and await remote execution to assess the success of their changes, leading to significant development time costs.

3. **Debugging Difficulties**: Investigating workflow failures becomes a daunting task since remote debugging requires more steps.

### Push and Pray

As someone who has navigated the labyrinthine complexities of GitHub Actions, debugging a failing workflow often feels like a descent into madness. We find ourselves pushing change after change to a remote repository, racking up action minutes, and crossing your fingers each time. Over the years, most of us have either seen or been part of a series of commits that look like this:

```shell
git commit -m "fix: initial attempt to debug this maze of a workflow"
git commit -m "fix: YAML more like WhyML, am I right?"
git commit -m "fix: removed that pesky tab, should be spaced now, or so I pray"
git commit -m "fix: reverted because the improvement was a trainwreck"
git commit -m "fix: action minutes are the new Bitcoin, and I'm broke"
git commit -m "fix: if this doesn’t work, I’m becoming a goat farmer"
```

## Transitioning to Local Execution with Dagger & Gale

Imagine moving from the elusively cloud-based operations of GitHub Actions to the convenience of execution these workflows right on your local machine. Gale facilitates this shift seamlessly. Using Gale feels like an intuitive extension of your regular development workflow, now enhanced with the power and functionality of Github Actions.

##### Sample Shell Command

Suppose you want to execute a Github Action workflow locally using Gale. your command might look like this:

```shell!
dagger -m github.com/aweris/gale@v0.0.10 call run --source . --workflow CI --job golangci --token $GITHUB_TOKEN sync
```

This command intructs Gale to execute the `golangci` job from the workflow named `CI` within the current directory's repository context.

##### Expected Output

```shell!
✔ dagger call run sync [1m22.0s]
┃ Workflow: CI
┃ ┏
┃ ┃ Job: golangci-lint
┃ ┃ ┏
┃ ┃ ┃ [...]
┃ ┃ ┃ 
┃ ┃ ┃ Lint
┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ run golangci-lint
┃ ┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ ┃ [...]
┃ ┃ ┃ ┃ ┃
┃ ┃ ┃ ┃ ┃ level=info msg="Execution took 875.738375ms"
┃ ┃ ┃ ┃ ┃
┃ ┃ ┃ ┃ ┃ golangci-lint found no issues
┃ ┃ ┃ ┃ ┃ Ran golangci-lint in 956ms
┃ ┃ ┃ ┃ ┗
┃ ┃ ┃ ┗
┃ ┃ ┃
┃ ┃ ┃ Complete job
┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ Complete job="golangci-lint" conclusion=success
┃ ┃ ┃ ┗
┃ ┃ ┗
┃ ┗
┗
• Engine: 1ca7bdc4f279 (version v0.9.3)
⧗ 1m24.7s ✔ 330 ∅ 53
```

This output, while streamlined for clarity, accurately demonstrates the the of detailed response Gale provides. It shows the step by step progression of the workflow concluding with a clear status message.

## The Solution

Faced with the challenges of remote-only execution in GitHub Actions, such as duplicated efforts, resource-intensive operations, prolonged feedback cycles, and cumbersome debugging, the need for a more integrated and flexible approach is evident.

[Dagger](https://docs.dagger.io/) stands out in this regard. Its modular structure, particularly through Dagger modules, addresses these challenges head-on. It simplifies the process of converting complex tasks into reusable components, aligning perfectly with the demands for speed and flexibility in modern software development. Dagger's approach is pivotal in creating more effective and efficient CI/CD solutions.

[Gale](https://github.com/aweris/gale), one of the many Dagger modules available, offers a tailored solution for locally executing GitHub Actions workflows. Gale effectively reduces the need for maintaining separate workflows, conserves valuable action minutes, and quickens the feedback loop for developers. Additionally, Gale brings the debugging process to the local environment, streamlining troubleshooting and enhancing overall workflow efficiency. This shift from cloud-centric operations to a more localized, hands-on methodology epitomizes the practicality and efficiency needed in today's CI/CD practices, positioning Gale as a key solution to addressing the limitations of GitHub Actions.

## Understanding Dagger and Dagger Modules

Before diving deeper into [Gale](https://github.com/aweris/gale), it's essential to grasp the foundations of Dagger and its modular system. Dagger stands as a revolutionary CI/CD platform, redefining the automation of software development workflows. Central to its innovation are Dagger modules, which are designed to encapsulate specific functions and tasks, thus promoting reusability across various projects and workflows.

Dagger lets you encapsulate all your project's tasks and workflows into simple modules, written in your preferred programming language. These modules are not just versatile and powerful but also allow for the creation of functions that can handle both basic and complex types. This versatility is key to addressing a broad spectrum of development needs.

The interconnectivity of modules is a core feature of Dagger. You can seamlessly add dependencies to your modules, enabling the use of existing solutions and fostering collaboration. This interconnected system is crucial for building upon the work of others and contributing to a community-driven ecosystem.

For a more comprehensive understanding of Dagger and its capabilities, interested readers are encouraged to explore the official [Dagger Documentation](https://docs.dagger.io/zenith/) or visit [Daggerverse](https://daggerverse.dev/), a dynamic registry featuring a diverse array of Dagger modules. These modules are collaboratively developed and maintained by the Dagger team and the vibrant Dagger community, offering a rich repository of tools and solutions for various CI/CD needs.

## Getting Started with Gale

### Introduction to Gale

[Gale](https://github.com/aweris/gale) significantly enhances the GitHub Actions workflow by enabling local execution of workflows. This innovative tool addresses the challenges of remote-only operations in GitHub Actions, streamlining debugging and testing for a more efficient development process.

#### Key Features of Gale:

- **Local Execution**: Run GitHub Actions workflows directly on your machine. This feature allows for quicker testing and debugging, bypassing the need for remote repository pushes.
- **Seamless Compatibility**: Gale is built to easily integrate with your existing GitHub Actions setups, ensuring a smooth transition to local execution without extensive workflow modifications.
- **Part of the Dagger Ecosystem**: Gale, as a module within Dagger, leverages the platform's ecosystem to enhance workflow efficiency. This integration facilitates smoother and quicker execution compared to standard remote GitHub Actions processes.

Gale simplifies and speeds up the CI/CD process, offering a practical solution to one of the most pressing limitations of GitHub Actions.

### Prerequisites

#### Dagger CLI
To use Gale, the Dagger CLI is a must-have. Here are the quick install commands for Linux and macOS:

- Quickly install Dagger by running:

```shell!
curl https://dl.dagger.io/dagger/install.sh | sudo BIN_DIR=/usr/local/bin DAGGER_VERSION=0.9.5 sh
```

These command provides a straightforward way to get Dagger up and running on your system. For more detailed instructions, alternative installation methods, or troubleshooting, visit the [Dagger CLI Installation Guide](https://docs.dagger.io/cli/465058/install).

#### Setup Dagger Module

To avoid adding `-m github.com/aweris/gale@v0.0.10` to every command, you can run the following command to
set the `DAGGER_MODULE` environment variable:

```shell
export DAGGER_MODULE=github.com/aweris/gale@v0.0.10
```

With the Dagger CLI installed and Dagger module set you're ready to start using Gale to enhance your GitHub Actions workflows.

## Using Gale

Gale enhances the GitHub Actions experience by enabling local execution of workflows. This section will guide you through utilizing Gale's capabilities, starting with selecting a repository and then diving into specific commands.

### Command Structure

Dagger uses a flexible command system that lets you link different commands together. Gale makes full use of this feature, offering a variety of commands that you can combine to fit your needs.

#### General Syntax

Here’s a quick look at how Gale commands are structured:

```shell!
dagger <Dagger Command> [Global Options] <Command> [Command Options] ...
```

- **Dagger Command:** A main Dagger action like call, export, or shell.
- **Global Options:** Settings for all global `gale` commands like `--source` or `--repo`
- **Command:** A specific Gale command, such as `list` or `run`.
- **Command Options:** Extra settings for each Gale command.
- **Command Chaining:** You can link commands based on what the previous command does.


For instance:

```shell!
dagger export --repo goreleaser/goreleaser --branch main run --workflow golangci-lint --job golangci --token $GITHUB_TOKEN data --output .gale/run
```

This command sequence demonstrates a chained `gale` command for executing a workflow and returning result directory. Here's a breakdown:

- `dagger export`: This part of the command prepares to export data from the workflow.
- `--repo goreleaser/goreleaser --branch main`: These global options specify the GitHub repository and branch to work with.
- `run --workflow golangci-lint --job golangci --token $GITHUB_TOKEN`: The run command executes the specified workflow and job in the repository. It uses the provided GitHub token for authentication.
- `data`: This is a chained command following run. It retrieves data generated by the workflow run, such as logs or artifacts.
- `--output .gale/run`: Finally, the export command saves the retrieved workflow run data to a local directory on your machine, .gale/run.

Each part of the command builds upon the previous, showcasing how Gale enables complex workflows through command chaining.

Command Examples:

- Listing Workflows:

```shell!
dagger --repo [owner/name] --branch [branch name] call list --workflows-dir [path to workflows]
```

- Running a Workflow:
```shell!
dagger call --source [path] run --workflow [workflow name] --job [job name] sync
```


- Exporting Workflow Data:

```shell!
dagger export --source [path] run  --workflow [workflow name] directory --output <local-path>
```

- Interactive Debugging:

```shell!
dagger shell --repo [owner/name] --branch [branch name] run --workflow [workflow name] sync
```

### Global Options

- `--branch`: Specify a branch name.
- `--repo`: Define the repository in the format owner/name.
- `--source`: Set the directory containing the repository source.
- `--tag`: Use a specific tag name.
- `--workflows-dir`: Path to the workflows directory (default is .github/workflows).

### Listing Workflows

Gale's `list` command is a practical tool for viewing GitHub Actions workflows and jobs. It offers flexibility to accommodate different project setups, whether you're working with local or remote repositories.

#### Command Options and Usage

- **Basic Local Usage**: To list workflows in given path:
  ```shell
  dagger call --source [path] list
  ```

- **For Remote Repositories**: Specify the repository name and branch/tag:
  ```shell
  dagger call --repo [owner/name] --branch [branch name] list
  ```

- **Using an Alternative Workflows Directory**: This is useful for scenarios where you want to use GitHub Actions workflows in Gale but not execute them as GitHub Actions themselves.
  ```shell
  dagger call --source [path] --workflows-dir [path to workflows] list
  ```

#### Understanding the Output

The command outputs the names of workflows and their respective jobs, providing a clear snapshot of your CI/CD pipeline.

**Example Output for `kubernetes/minikube`**:
  ```shell
  dagger call --repo kubernetes/minikube --branch master list
  ```
Output:
  ```
  ...
  Workflow: build (path: .github/workflows/build.yml)
  Jobs:
  - build_minikube
  - lint
  - unit_test
  ...
  ```

### Running Workflows

Gale's `run` command offers extensive flexibility for executing GitHub Actions workflows locally. This command is versatile, allowing for various customizations and usage scenarios.


#### Customizing Workflow Execution

Gale provides several options for tailoring workflow execution:

- **Workflow Selection**: The `--workflow` flag selects a specific workflow by name or path.
- **Job Filtering**: Use `--job` to run a specific job within the workflow. Without this, Gale executes all jobs by default.
- **Custom Runner Image**: The `--image` flag allows specifying a custom image for the workflow runner.
- **Event Simulation**: `--event` and `--event-file` enables the simulation of specific GitHub events, like a push event, with `--event push` or a detailed event payload using `--event-file /path/to/event.json`.
- **Debug Mode**: Enable runner debug mode with `--runner-debug`.

#### Executing and Interacting with Workflows

Gale offers subcommands to execute and interact with workflows:

- **Sync**: The `sync` subcommand runs the workflow and outputs the container logs.

Example:

```shell
 dagger call ... run ... sync
```

- **Directory Export**: Use a `directory` to export workflow run information to a specified local path. It includes options to include artifacts, event files, repository source, and secrets in the export.

```shell
dagger export ... run ... directory --output <some-local-path>
```

- **Interactive Debugging**: Combining `dagger shell` with `sync` allows you to connect to a container post-execution, useful for debugging workflow issues. For example:

```shell
dagger shell ... run ... sync
```

### Examples

To better understand how Gale can be utilized in real-world scenarios, let's explore some examples. These examples demonstrate Gale's capabilities in listing and running workflows, providing insights into its practical applications and benefits.

####  Listing Workflows with Custom Workflow Directory

This example demonstrates how to use Gale's `list` command to view workflows from a specific repository with a custom workflow directory. This is particularly useful when you want to list workflows that are not in the default `.github/workflows` directory.

**Command:**

```shell
dagger call --repo aweris/gale --branch main --workflows-dir examples/workflows list
```

**Expected Output:**

```shell
✔ dagger call list [3.63s]
┃ Workflow: examples/workflows/artifact-cache.yaml
┃ Jobs:
┃  - test
┃
┃ Workflow: matrices (path: examples/workflows/job-matrices.yaml)
┃ Jobs:
┃  - matrix
┃
┃ Workflow: outputs (path: examples/workflows/job-outputs.yaml)
┃ Jobs:
┃  - gen-outputs
┃  - use-outputs
┃
┃ Workflow: examples/workflows/lint.yaml
┃ Jobs:
┃  - golangci-lint
┃
┃ Workflow: examples/workflows/secret-printer.yaml
┃ Jobs:
┃  - test
┃
┃ Workflow: step (path: examples/workflows/step.yaml)
┃ Jobs:
┃  - action
┃  - run
┃  - docker
• Engine: 1ca7bdc4f279 (version v0.9.3)
⧗ 6.93s ✔ 110 ∅ 27
``` 


This output shows the various workflows and their respective jobs located in the `examples/workflows` directory of the `aweris/gale` repository.

####  Running a Specific Job within a Workflow

This example demonstrates running the `golangci-lint` job from the `CI` workflow in the `prometheus/prometheus` repository.

**Command:**

```shell
dagger call --repo prometheus/prometheus --branch main run --workflow CI --job golangci --token $GITHUB_TOKEN sync
```

**Expected Output:**

```shell
✔ dagger call run sync [1m22.0s]
┃ Workflow: CI
┃ ┏
┃ ┃ Job: golangci-lint
┃ ┃ ┏
┃ ┃ ┃ [...]
┃ ┃ ┃ 
┃ ┃ ┃ Lint
┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ run golangci-lint
┃ ┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ ┃ [...]
┃ ┃ ┃ ┃ ┃
┃ ┃ ┃ ┃ ┃ level=info msg="Execution took 875.738375ms"
┃ ┃ ┃ ┃ ┃
┃ ┃ ┃ ┃ ┃ golangci-lint found no issues
┃ ┃ ┃ ┃ ┃ Ran golangci-lint in 956ms
┃ ┃ ┃ ┃ ┗
┃ ┃ ┃ ┗
┃ ┃ ┃
┃ ┃ ┃ Complete job
┃ ┃ ┃ ┏
┃ ┃ ┃ ┃ Complete job="golangci-lint" conclusion=success
┃ ┃ ┃ ┗
┃ ┃ ┗
┃ ┗
┗
• Engine: 1ca7bdc4f279 (version v0.9.3)
⧗ 1m24.7s ✔ 330 ∅ 53
```

#### Exporting Workflow Run Directory

This example demonstrates how to use Gale to export workflow run information including uploaded artifacts and repository sources after workflow execution.

**Command:**

```shell
dagger export --repo kubernetes/minikube --branch master run --workflow build --job build_minikube --token $GITHUB_TOKEN data --output .gale/exports
```

**Expected Output:**

```shell
✔ dagger download run directory [4m39.0s]
┃ Asset exported to ".gale/exports"
• Engine: 1ca7bdc4f279 (version v0.9.3)
⧗ 4m42.3s ✔ 364 ∅ 55
```

**Export Directory Structure:**

```shell
.gale/exports/
├── artifacts
│   └── minikube_binaries
│       ├── [List of binaries]
│
├── repo
│   ├── [List of repository files]
│
└── run
    ├── event.json
    ├── jobs
    │   └── build_minikube
    │       ├── job_run.json
    │       └── steps
    │           └── [Step details]
    ├── workflow.yaml
    └── workflow_run.json
```

The resulting artifacts, repository source, along with details of the run, are exported to the `.gale/exports` directory. This approach significantly reduces the time to obtain build artifacts compared to running the entire CI process locally.

####  Interactive Shell Access to Workflow Container

This example demonstrates how to connect to workflow containers after execution, particularly useful for debugging files and repositories in failing workflow runs.

**Command:**

```shell
dagger shell --repo prometheus/prometheus --branch main run --workflow CI --job golangci --token $GITHUB_TOKEN sync
```

**Accessing the Interactive Shell:**

Once workflow execution finishes dagger connects you'll be in work directory of the container

```shell
root@df9puffur8j16:/home/runner/work/prometheus/prometheus#
```

In this directory you can find contents of your repository after workflow execution

```shell
root@df9puffur8j16:/home/runner/work/prometheus/prometheus# ls -al
total 508
drwxr-xr-x  1 root root   4096 Nov 16 15:09 .
drwxr-xr-x  3 root root   4096 Nov 16 15:09 ..
drwxr-xr-x  2 root root   4096 Nov 16 15:08 .circleci
-rw-r--r--  1 root root    133 Nov 16 15:08 .dockerignore
drwxr-xr-x  1 root root   4096 Nov 16 15:09 .git
drwxr-xr-x  4 root root   4096 Nov 16 15:08 .github
-rw-r--r--  1 root root    504 Nov 16 15:08 .gitignore
-rw-r--r--  1 root root    601 Nov 16 15:08 .gitpod.Dockerfile
-rw-r--r--  1 root root    429 Nov 16 15:08 .gitpod.yml
-rw-r--r--  1 root root   2508 Nov 16 15:08 .golangci.yml
-rw-r--r--  1 root root   1339 Nov 16 15:08 .promu.yml
-rw-r--r--  1 root root    435 Nov 16 15:08 .yamllint
-rw-r--r--  1 root root 151806 Nov 16 15:08 CHANGELOG.md
-rw-r--r--  1 root root    152 Nov 16 15:08 CODE_OF_CONDUCT.md
-rw-r--r--  1 root root   5069 Nov 16 15:08 CONTRIBUTING.md
-rw-r--r--  1 root root   1270 Nov 16 15:08 Dockerfile
-rw-r--r--  1 root root  11357 Nov 16 15:08 LICENSE
-rw-r--r--  1 root root   1589 Nov 16 15:08 MAINTAINERS.md
-rw-r--r--  1 root root   4576 Nov 16 15:08 Makefile
-rw-r--r--  1 root root   9133 Nov 16 15:08 Makefile.common
-rw-r--r--  1 root root   3773 Nov 16 15:08 NOTICE
-rw-r--r--  1 root root   7838 Nov 16 15:08 README.md
-rw-r--r--  1 root root  15679 Nov 16 15:08 RELEASE.md
-rw-r--r--  1 root root    172 Nov 16 15:08 SECURITY.md
-rw-r--r--  1 root root     12 Nov 16 15:08 VERSION
drwxr-xr-x  4 root root   4096 Nov 16 15:08 cmd
drwxr-xr-x  3 root root   4096 Nov 16 15:08 config
drwxr-xr-x  2 root root   4096 Nov 16 15:08 console_libraries
drwxr-xr-x  2 root root   4096 Nov 16 15:08 consoles
drwxr-xr-x 31 root root   4096 Nov 16 15:08 discovery
drwxr-xr-x  6 root root   4096 Nov 16 15:08 docs
drwxr-xr-x  5 root root   4096 Nov 16 15:08 documentation
-rw-r--r--  1 root root   9378 Nov 16 15:08 go.mod
-rw-r--r--  1 root root 121270 Nov 16 15:08 go.sum
drwxr-xr-x 11 root root   4096 Nov 16 15:08 model
drwxr-xr-x  2 root root   4096 Nov 16 15:08 notifier
drwxr-xr-x  2 root root   4096 Nov 16 15:08 plugins
-rw-r--r--  1 root root   1151 Nov 16 15:08 plugins.yml
drwxr-xr-x  3 root root   4096 Nov 16 15:08 prompb
drwxr-xr-x  5 root root   4096 Nov 16 15:08 promql
drwxr-xr-x  3 root root   4096 Nov 16 15:08 rules
drwxr-xr-x  3 root root   4096 Nov 16 15:08 scrape
drwxr-xr-x  2 root root   4096 Nov 16 15:08 scripts
drwxr-xr-x  3 root root   4096 Nov 16 15:08 storage
drwxr-xr-x  2 root root   4096 Nov 16 15:08 template
drwxr-xr-x  3 root root   4096 Nov 16 15:08 tracing
drwxr-xr-x 16 root root   4096 Nov 16 15:08 tsdb
drwxr-xr-x 18 root root   4096 Nov 16 15:08 util
drwxr-xr-x  4 root root   4096 Nov 16 15:08 web
```

From this directory if you switch over `/home/runner/_temp/_gale/runs/<run_id>` directory you can access directory contains workflow information:

```shell
> cd /home/runner/_temp/_gale/runs/186b5824-a7b7-4c85-b8e1-c35d1cb76d6a/

root@df9puffur8j16:/home/runner/_temp/_gale/runs/186b5824-a7b7-4c85-b8e1-c35d1cb76d6a# ls -al

total 16
drwxr-xr-x 4 root root 4096 Nov 16 15:08 .
drwxr-xr-x 3 root root 4096 Nov 16 15:09 ..
drwxr-xr-x 3 root root 4096 Nov 16 15:08 run
drwxr-xr-x 2 root root 4096 Nov 16 15:08 secrets
```

View details of the workflow run:

```shell
cat run/workflow_run.json | jq .
```

Expected output

```json
{
  "ran": true,
  "duration": "20.45445351s",
  "name": "CI",
  ...
  "conclusion": "success",
  "jobs": {
    "golangci": "success"
  }
}
```

## Future Directions

As Gale continues to mature, a key focus will be on enhancing its compatibility with GitHub Actions and the broader Dagger module ecosystem. This includes developing more sophisticated event simulation capabilities and advanced debugging tools, which are essential for a more seamless and intuitive user experience.

The integration with Dagger's ecosystem is particularly promising, as it opens up possibilities for more customization options, allowing users to tailor their CI/CD workflows to their specific needs.

## Next Steps

### Exploring Further and Giving Feedback

As Gale continues to evolve, there are several ways you can engage with and contribute to its development:

- **Deep Dive into Gale**: Begin integrating Gale into your current GitHub Actions workflows. Experiment with its features to fully understand its capabilities and how it can benefit your projects.

- **Join the Community**: Participate in the Dagger and Gale communities. Share your experiences, learn from others, and gain valuable insights. You can join the [Dagger Discord server](https://discord.gg/ufnyBtc8uY) to connect with fellow users and the development team.

- **Provide Feedback**: Your experiences and suggestions are crucial for Gale's improvement. Share your feedback through the Dagger community channels or directly on the [Gale GitHub repository](https://github.com/aweris/gale).

- **Stay Updated**: Keep up with the latest developments in Gale and Dagger. New features and improvements are continuously being added, expanding the tools' capabilities.

- **Contribute**: If you're interested, consider contributing to Gale's development. Your contributions can range from code to documentation and are vital for the tool's growth.
