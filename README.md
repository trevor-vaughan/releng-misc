# releng-misc

<!-- vim-markdown-toc GFM -->

* [Overview](#overview)
* [Bolt Project](#bolt-project)
  * [Setup](#setup)
    * [What the `releng::` project affects](#what-the-releng-project-affects)
      * [For Github Organizations](#for-github-organizations)
      * [For GitLab Groups](#for-gitlab-groups)
    * [Setup Requirements](#setup-requirements)
    * [Beginning with the releng:: Bolt project](#beginning-with-the-releng-bolt-project)
  * [Usage](#usage)
    * [](#)
* [Contributing](#contributing)

<!-- vim-markdown-toc -->

# Overview

This project collects the various tools (script, config, notes, etc.) we've
been using to assist with RELENG-related activities. The purpose is to
establish **awareness** of these tools, and give everyone a change to
inspect/improve/use them.

**WARNING** Things collected here may be broken, full of bugs, hard to use, and
out of date.  Don't assume that anything here is suitable to use in
production without inspecting and testing it first.


# Bolt Project

This repository includes the `releng::` [Bolt] project, including plans to
help us inspect and administer our GitHub organization and GitLab group.

## Setup

### What the `releng::` project affects


#### For Github Organizations

* Clone all repos into a single directory (**`github_inventory::clone_git_repos`**)
* Report the highest SemVer tag in each repo (**`github_inventory::latest_semver_tags`**)
* List and/or set which PR checks are required on each repo
  (**`github_inventory::required_checks`**)
* Report repos with GitHub Actions workflows (**`github_inventory::workflows`**)


#### For GitLab Groups

* Mirror all projects from their corresponding GitHub repo,
  using a dedicated API token/account.
  (**`releng::gitlab_project_repo_mirror`**)
* Identify/erase all Gitlab CI job logs between two dates
  (**`releng::gitlab_project_ci_logs`**)


### Setup Requirements

* [Puppet Bolt 3.0+][bolt], installed from an [OS package][bolt-install]
  (don't run from a RubyGem or use rvm)
*  GitHub + GitLab API auth tokens with sufficient scope
* Environment variables:
  * **`GITHUB_API_TOKEN`**
  * **`GITLAB_API_PRIVATE_TOKEN`** - usually needs `api read+write` scope for updating mirrors
* The [`octokit`][octokit-rb] & [`gitlab`][gitlab-rb] RubyGems

### Beginning with the releng:: Bolt project

1. If you are using [rvm], you **must disable it** before running bolt (We need
   to use the `puppet-bolt` package's ruby interpreter):

   ```sh
   rvm use system
   ```

2. Install the RubyGem dependencies using Bolt's `gem` command:

   On most platforms:

   ```sh
   /opt/puppetlabs/bolt/bin/gem install --user-install -g gem.deps.rb
   ```

   On Windows:

   ```pwsh
   "C:/Program Files/Puppet Labs/Bolt/bin/gem.bat" install --user-install -g gem.deps.rb
   ```

3. Install the Puppet modules:

   ```sh
   bolt module install
   ```
   
4. Set the environment variables **`GITHUB_API_TOKEN`** and 
   **`GITLAB_API_PRIVATE_TOKEN`**, as required.

5. Verify that you can see the project's Bolt plans:

   ```sh
   bolt plan show
   ```

## Usage

This repo contains RELENG-related [Puppet Bolt] orchestration (in the
[Boltdir/](Boltdir/) directory). For information on the available tasks and
plans, run:

```sh
bolt plan show [plan_name]

bolt task show [task_name]

```

### 

# Contributing

* If you'd like to contribute something that you've been using, drop it in a
  new folder (preferably with a small `README.md` to let others know what it
  is). Don't let polishing things hold you up from contributing!

[bolt]: https://puppet.com/docs/bolt/latest/bolt.html
[gitlab-rb]: https://rubygems.org/gems/gitlab
[bolt-install]: https://puppet.com/docs/bolt/latest/bolt_installing.html
[inventory file]: https://puppet.com/docs/bolt/latest/inventory_file_v2.html
[inventory reference plugin]: https://puppet.com/docs/bolt/latest/using_plugins.html#reference-plugins
[`local` transport]: https://puppet.com/docs/bolt/latest/bolt_transports_reference.html#local
[octokit-rb]: https://github.com/octokit/octokit.rb
[Puppet Bolt]: https://puppet.com/docs/bolt/latest/bolt.html
[rvm]: https://rvm.io
