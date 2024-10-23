# Deployment Notes

JupyterLite environments can be deployed via the VLE in two main ways:

- link to a URL of the JupyterLite site running on a public URL;
- upload the modded JupyterLite site as an `HTML5.zip` activity bundle;

## BUNDLING FOR OU VLE

Currently, we need some hacks to the jupyterlite distribtuion when bundling it for OU VLE / HTML5.zip:

- filenames only allow [a-zA-Z-_]; so e.g. spaces in e.g. notebook filenames throw an error;
- the `index.html` pages in `lab/`, `tree/` and `notebooks/` need updating: `<link rel="manifest" href="/app.webmanifest" /> ` needs to be set to `<link rel="manifest" href="/app.webmanifest" crossorigin="use-credentials" />` to handle the auth; NOTE: in more recent JupyterLite builds, the path seems to be to `../app.webmanifest`; the following should replace it: `sed -i '.bak' 's|<link rel="manifest" href="\([^"]*\)" />|<link rel="manifest" href="\1" crossorigin="use-credentials" />|' ./static/lab/index.html` (on Mac, we need `sed -i '.bak' ...` (requires `''` (no backup) or a backup filename suffix, such as `.bak`));
- the html5zip is referenced in an iframe with a load of URL paramters added to it by the vle'; this prevents the automatic redirect to eg `lab/index.html`; solution (h/t Claude.ai) is to strip the URL of unwanted params (note that this currently breaks the history/back button for the corresponding browser tab). We need to clean the URL of unwanted parameters. One fix is to modify `./config-utils.js` as follows (a copy of the required updated file can be found [here](https://github.com/OpenComputingLab/jupyterlite_in_moodle_vle/tree/main/resources)) :

```javascript
// Add this function
function stripUriParameters() {
  const url = new URL(window.location.href);
  const path = url.searchParams.get('path');
  url.search = '';
  // The path parameter at least is valid
  if (path) {
    url.searchParams.set('path', path);
  }
  window.history.replaceState({}, '', url);
  //That actually trashes the page history.
  // May be better to use: window.history.pushState({}, '', url.toString());
}

// Then as the first line of function main(), add:
//  stripUriParameters();
```

- to create the zip, we need an `index.html` at the root of the zip folder;  where a jupyterlite distribution asset is created by a GitHub action, unzip and untar the Jupyterlite asset uploaded by the action, `cd` into it and then (after any mods e.g. to `index.html` files), zip  the directory as root into a `.zip` archive file, e.g. `zip -r ../jupyterlite-dist-tm112.zip .`. If zipping a git tracked dir, add `--exclude "*.git*"` to the zip command to ignore git files.
- to get the html5 bundle URL, upload the html5.zip as an HTML5.zip resource type; view the associated page; the JupyterLite environment is in the embedded iframe but gets stuck on the `./index.html` page; right click on the iframe (which will be blank ) and view it in a separate window to get the URL of the embedded html5 content; or, in browser dev tools, inspect page in to find the associated iframe and its `src` value. Take this path and extend it to point to eg `lab/index.html` rather than `./index.html`; you should now be able to use this link as an embedded URL link to display the lab environment in a Moodle page.

## Example JupyterLite Xeus Python distribution


Example `build_environment.yml`:

```yaml
name: xeus-kernels
channels:
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
dependencies:
  - python
  - pip
  - jupyter_server
  - jupyterlite-core >=0.3.0,<0.4.0
  - pip:
    - jupyterlite-xeus>=0.2.0a0,<0.3
```

Example `environment.yml`:

```yaml
name: xeus-python-kernel
channels:
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
dependencies:
  - pip
  - xeus-python
  - pandas
  - matplotlib
  - ipywidgets
  - requests
  - folium
  - pip:
    - ipython-magic-folium
```

We can also add a separate `requirements.txt` eg for extensions to the JupyterLite environment.

```text
# Core modules (mandatory)
jupyterlite-core
jupyterlab
notebook

jupyterlite-xeus

# Python kernel (optional)
#jupyterlite-pyodide-kernel

# JavaScript kernel (optional)
#jupyterlite-javascript-kernel==0.2.3

# Language support (optional)
jupyterlab-language-pack-fr-FR
jupyterlab-language-pack-zh-CN

# Jupyer environment extensions
jupyterlab-filesystem-access

jupyterlab-empinken-extension
jupyterlab-cell-status-extension
jupyterlab-ou-brand-extension

```

*TO DO / TO CHECK — what actually gets bundled and what gets added into the kernel as a preinstalled package? Are all packages listed in `environment.yaml` and `requirements.txt` bundled in the distribution? [From a quick look, it looks like they are all in the distribution; and perhaps the packages in the environment.yaml are installed into the kernel?]*

Example action for bundling a xeus Pyhton jupyterlite distribution:

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - '*'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install mamba
        uses: mamba-org/setup-micromamba@v1
        with:
          micromamba-version: '1.5.8-0'
          environment-file: build_environment.yml
          cache-environment: true
    
      - name: Install the dependencies
        run: |
          python -m pip install -r requirements.txt
      - name: Build the JupyterLite site
        run: |
          cp README*.md content
          jupyter lite build --contents content --output-dir dist

    # We can use the uploaded artifact 
    # as the basis for an HTML5.zip upload
    # BUT see elsewhere for required hacks

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          name: jupyterlite_dist
          path: ./dist

```

### Example JupyterLite R distribution

Example requirements:

```text
jupyterlab-widgets==3.0.10
jupyterlab~=4.1.8
jupyterlite-core>=0.3.0
notebook~=7.1.3

# Kernel
git+https://github.com/r-wasm/jupyterlite-webr-kernel.git

# Branding and OU extensions
jupyterlab-ou-brand-extension==0.2.3

# Notebook cell tools
jupyterlab-cell-status-extension==0.2.2
jupyterlab-empinken-extension==0.5.2
jupyterlab-myst==2.4.0
jupyterlab-spellchecker==0.8.4

# File browsing and handling
jupyterlab-unfold==0.3.0
jupyter-archive==3.4.0
jupyterlab-filesystem-access==0.6.0
```

Example action:

```yaml
name: Deploy

on:
  workflow_dispatch:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  release:
    types: [published]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Upgrade pip
        run: python -m pip install --upgrade pip

      - name: Build jupyterlite
        run: |
          python -m pip install -r requirements.txt
          jupyter lite build --contents content --output-dir dist

      - name: Build the JupyterLite site
        run: |
          cp README.md content
          jupyter lite build --contents content --output-dir dist
        
    # We can use the uploaded artifact 
    # as the basis for an HTML5.zip upload
    # BUT see elsewhere for required hacks

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

    # Optionally publish the site to GitHub pages
      - name: GitHub Pages action
        uses: peaceiris/actions-gh-pages@v3.6.1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## Running as a standalone / offline application

To run the application offline, or as a standalone application, we need to ensure that all files are bundled as part of the JupyterLite distribution.

If any packages outside the core distribution are required in order to run particular notebooks, these packages will need to be shipped as part of the distribution and made available to the appropriate kernel.

By default, the `jupyterlite-webr` kernel will look to a remote URL to install and load packages on demand. The target URL can be set via an option within a notebook, or baked into the kernel as a default at run time.

## Customisations for webR (e.g. M348)

For standalone operation in M348, which uses the `jupyterlite-webr` kernel, we need to bundle required packages, as well as the webR base package, into the JupyterLite distribution:

- example of creating custom built webR packages: https://github.com/ouseful-testing/webr-package-repo-demo/ ; note that we need to match the R version with the R kernel version; in JupyterLite R kernel, this currently lags the main webR R version; the built packages can be downloaded as workload assets the placed where there need to be;

- we can set the package download path in an R notebook code cell: `options(webr_pkg_repos= "https://ouseful-demos.github.io/jupyterlite-m348-demo/repo/")`;

- also also need to fix the path to the `webr` wasm packages; this can be done from a [custom build](https://github.com/ouseful-testing/jupyterlite-webr-kernel/tree/th-custom-path) of the `jupyterlite-webr-kernel` by setting the path in build config files as per [here](https://github.com/ouseful-testing/jupyterlite-webr-kernel/issues/1).

## BORKEDNESS

Issues in notebooks:

- JupyterLite now seems to drop explicit reference to `index.html` from the notebook homepage toc links; which breaks the VLE handler and means we can't open notebooks from the notebook index / ToC homepage.

Issues in JupyterLab:

- N/A

## OPEN ISSUES

For distribution into secure enviromments, *everything* will need to be bundled into an `html5.zip` bundle.

To explore:

- use something like `esbuild` to bundle all assets into a js bundle *(I have no experience as yet with bundling web resources...)*; this will include the need to bundle third party packages and their dependencies as well as wasm packages. (Serving wasm packages may require mime-type settings updates on the VLE..)

*If anyone wants to team up exploring any of the above, please email me: `tony.hirst@open.ac.uk`.*
