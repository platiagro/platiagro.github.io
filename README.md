# Documentation for PlatIAgro

We use [Hugo](https://gohugo.io/) to format and generate our website, the
[Docsy](https://github.com/google/docsy) theme for styling and site structure,
and [GitHub Actions](https://github.com/features/actions) to manage the deployment of the site.

## Previewing your changes on a local website server

If you'd like to preview your doc updates as you work, you can install Hugo
and run a local server to host your website. This section shows you how.

### Install Hugo and other dependencies

You need Hugo version 0.68 or later, and it must be the **extended** version of
Hugo. Hugo version 0.68 and later support thenGoldmark renderer for Markdown.
Goldmark offers improved rendering of some text formatting such as lists.

To get the latest extended version of Hugo:

1.  Go to the [Hugo releases](https://github.com/gohugoio/hugo/releases) page.
1.  In the most recent release, scroll down until you find a list of
    **extended** versions.
1. Download the relevant file for your operating system.
1. Unzip the downloaded file into a location of your choice.

For example, to install Hugo on Linux:

1.  Download `hugo_extended_0.71.1_Linux-64bit.tar.gz`
    (or the latest version) from the
    [Hugo releases](https://github.com/gohugoio/hugo/releases/tag/v0.71.1) page.

1.  Create a new directory:

        mkdir $HOME/hugo

1.  Extract the file you downloaded to `$HOME/hugo`.

        tar -zxvf hugo_extended_0.71.1_Linux-64bit.tar.gz

For more details about installing Hugo, See the
[Hugo installation guide](https://gohugo.io/getting-started/installing/).

If you plan to make changes to the site styling, you need to install some
**CSS libraries** as well. Follow the instructions in the
[Docsy theme's setup
guide](https://www.docsy.dev/docs/getting-started/#install-postcss).

### Fork and clone the website repository and run a local website server

Follow the usual GitHub workflow to fork the repository on GitHub and clone it
to your local machine, then use your local repository as input to your Hugo web
server:

1. Clone the repo locally.

    ```
    git clone https://github.com/platiagro/platiagro.github.io.git
    cd platiagro.github.io/
    ```

1. Get local copies of the project submodules so you can build and run your site locally:

    ```
    git submodule update --init --recursive
    ```

1. Start your website server. Make sure you run this command from the
   `platiagro.github.io/` directory, so that Hugo can find the config files it needs:

    ```
    hugo server -D
    ```

1. You can access your website at
  [http://localhost:1313/](http://localhost:1313/).
