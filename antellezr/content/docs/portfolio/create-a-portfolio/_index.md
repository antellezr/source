---
title: Create a Portfolio Website
weight: 1
---

# Create a Portfolio Website

## Purpose

This document describes the process for creating a static portfolio website using Hugo and GitHub Pages.

## Audience

This document is intended for individuals interested in using Hugo and GitHub Pages to create and host a portfolio.

## Overview

To create and publish a portfolio website, take the following steps:

- [Create a Portfolio Website](#create-a-portfolio-website)
  - [Purpose](#purpose)
  - [Audience](#audience)
  - [Overview](#overview)
  - [Prerequisites](#prerequisites)
  - [Create the GitHub Repositories](#create-the-github-repositories)
    - [Content Repository](#content-repository)
    - [Site Repository](#site-repository)
  - [Create a Hugo site](#create-a-hugo-site)
  - [Set Up the Site Configuration File](#set-up-the-site-configuration-file)
  - [Add Content to the Site](#add-content-to-the-site)
  - [Clone the GitHub Pages Site Repository](#clone-the-github-pages-site-repository)
  - [Add the GitHub Site Repository as a Submodule](#add-the-github-site-repository-as-a-submodule)
  - [Deploy the Site Using GitHub Pages](#deploy-the-site-using-github-pages)

## Prerequisites

Before you begin this guide you must:

- [Install Hugo](https://gohugo.io/installation/) (version 0.134 or higher)
- [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Create a GitHub account](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github)

## Create the GitHub Repositories

This guide uses two separate repositories linked via Git submodules, following these criteria:

- [**Content Repository**](#content-repository): Stores Markdown content, Hugo configuration files, and themes subdirectories.
- [**Site Repository**](#site-repository): Contains the HTML, CSS, and JavaScript files generated by Hugo.

A [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) is a repository nested inside another repository, enabling you to manage content separately from deployment files while keeping them linked. Using a submodule ensures that updates to your website's source code do not directly affect the published site until changes are explicitly pushed.

### Content Repository

To create the `source` content repository from GitHub's web UI, follow the next steps:

1. Go to your GitHub dashboard.\
    {{<button href="https://github.com/dashboard">}}GitHub Dashboard{{</button>}}
2. In the upper-right corner of the page, select the **New Repository** option from the  **+** drowpdown list, as shown in **Figure 1.**
   <center>

      <img src="new-repository.jpg" alt="New Repository" title="New Repository" class="img-border" style="border: 1px solid lightgrey; border-radius: 5px; padding: 5px;">\
      **Figure 1. Create a New Repository**

   </center>
3. From the **Owner** dropdown list, select the account you want to own the repository.
4. Type the name of your repository, in this example, `source`.
5. Select `Public` as the repository visibility. To learn more about public and private repositories, see [About repository visibility](https://docs.github.com/en/repositories/creating-and-managing-repositories/about-repositories#about-repository-visibility).
6. Click the **Create repository** button at the bottom of the page.\
    **Figure 2** displays the the repository overview page.

   <center>

   <img src="source-repo-overview.jpg" alt="Repository overview page" title="Repository overview page" class="img-border" style="border: 1px solid lightgrey; border-radius: 5px; padding: 5px;">\
   **Figure 2. Repository Overview Page Preview**
   </center>

{{% hint info %}}
**Note**: You can also create a repository using the GitHub CLI. To learn more, see [`gh repo create`](https://cli.github.com/manual/gh_repo_create) in the GitHub CLI documentation.
{{% /hint %}}

### Site Repository

Create the site repository using a similar process:

1. Go to your GitHub dashboard.\
    {{<button href="https://github.com/dashboard">}}GitHub Dashboard{{</button>}}
2. Select the **New Repository** option from the  **+** drowpdown list.
3. From the **Owner** dropdown list, select the account you want to own the repository.
4. Type `{github-username}.github.io` as the name of your repository.
5. Select `Public` as the repository visibility.
6. Click the **Create repository** button.

{{% hint info %}}
**Note**: Use your `{github-username}.github.io` as the repository name for a shorter URL when the site is deployed. Alternatively, you can choose a different repository name, but the site’s URL will then appear as `{github-username}.github.io/{repository-name}`.
{{% /hint %}}

## Create a Hugo site

Hugo is a static site generator that transforms Markdown files into a structured website. This section covers setting up a new Hugo project and configuring it with a theme. To create a new Hugo site in your `source` directory using the [Hugo Book](https://themes.gohugo.io/themes/hugo-book/) theme, follow the next steps:

1. In the location of your choice, clone the `source` repository:

    ```bash
    git clone https://github.com/{github-username}/source.git
    ```

2. Change to your `source` directory:

    ```bash
    cd source
    ```

3. Create a new Hugo site:

    ```bash
    hugo new site {hugo-site-name}
    ```

    The root [directory structure](https://gohugo.io/getting-started/directory-structure/) for your project is created.

4. Change into the site directory:

    ```bash
    cd {hugo-site-name}
    ```

5. Add the Hugo Book theme as a Git submodule:

    ```bash
    git submodule add https://github.com/alex-shpak/hugo-book themes/hugo-book
    ```

6. Set the theme in the `hugo.toml` site configuration file.

    ```bash
    echo "theme = 'hugo-book'" >> hugo.toml
    ```

## Set Up the Site Configuration File

The `hugo.toml` configuration file is the main settings file for a Hugo site. It defines site-wide configuration options that control how Hugo generates and structures the static site, including:

- Site metadata
- Theme configuration
- Content settings
- Custom parameters

Edit the `hugo.toml` configuration file to match your site details:

```toml
baseURL = 'https://{github-username}.github.io/'
languageCode = 'en-us'
title = '{User Name} Portfolio'
theme = 'hugo-book'

# Shortcodes support
[markup]
[markup.goldmark.renderer]
unsafe = true
```

## Add Content to the Site

The Hugo Book theme includes an `exampleSite` directory with sample pages and blog posts that showcase the theme’s features. These examples serve as the starting point for this guide.

To copy the content of the `exampleSite` subdirectory to your root directory, use the following command:

```bash
cp -R themes/hugo-book/exampleSite/content.en/* ./content
```

By default, the theme renders pages from the `/content/docs` section as a menu in a tree structure.
You can set `title` and `weight` in the front matter of pages to adjust the order and titles in the menu, as well as other parameters to hide or alter URLs in the menu.

Alternatively, you can create new content manually using the following command:

```bash
hugo new content content/posts/my-first-post.md
```

{{% hint info %}}
**Note**: For more information about content types in Hugo, see the [Content Management](https://gohugo.io/content-management/types/) section of the Hugo documentation.
{{% /hint %}}

## Clone the GitHub Pages Site Repository

To clone the GitHub Pages Site Repository, follow the next steps:

1. Navigate back to the parent directory:

   ```bash
   cd ..
   ```

2. Clone the GitHub Pages repository:

   ```bash
   git clone https://github.com/{github-username}/{github-username}.github.io.git
   ```

3. Navigate into the repository:

   ```bash
   cd {github-username}.github.io
   ```

4. Create a README file:

   ```bash
   touch README.md
   ```

5. Commit and push the README file to the repository:

   ```bash
    git add .
    git commit -m "Adds README"
    git push origin main
   ```

## Add the GitHub Site Repository as a Submodule

To connect your Hugo project to the GitHub Pages repository, follow these steps.

1. Return to the Hugo root directory:

   ```bash
   cd ../source/{hugo-site-name}
   ```

2. Add the site repository as a submodule:

   ```bash
   git submodule add -b main https://github.com/{github-username}/{github-username}.github.io.git public
   ```

3. Verify the submodule was added:

   ```bash
   ls public
   ```

4. Ensure the `public` folder contains a `README.md` file.

## Deploy the Site Using GitHub Pages

To make your Hugo site publicly accessible, you need to generate the static website files and deploy them to GitHub Pages. Follow these steps to build and publish your site.

1. Start the local development server:

   ```bash
   hugo server
   ```

  {{% hint info %}}
  **Note**: This command starts the local server and enables you to view your site in the browser. To stop the server, press `Ctrl + C`.
  {{% /hint %}}

2. Generate the static website:

   ```bash
   hugo
   ```

3. Navigate to the `public` directory:

   ```bash
   cd public
   ```

4. Commit and push:

   ```bash
   git add .
   git commit -m "Initial site deployment"
   git push origin main
   ```

Once the `public` directory is pushed, your site is live at:

```bash
https://{github-username}.github.io/
```
