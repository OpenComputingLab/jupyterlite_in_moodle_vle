# Building Standalone Distributions

The JupyterLite application can be distributed simply as a set of standalone static website files and published as if it were a simple static website. If all the files required to run the JupyterLite environment are bundled as part of the JupyterLite distribution in the nornal JupyerLite build process, the applcation can be served from a single domain, without the need to make http requests to any other website.

A JuptyerLite site can also be bundled with a simple webserver and distributed as a simple self-contained desktop application (for example, a single Windows `.exe` file distributed via DVD-ROM or USB memory stick). This application can run on a desktop without the need for any other software to be installed, and without any requirement for a network connection.

*For the OU module M348 (24J presentation), we are currently exploring the use of a simple .exe file distribution for use by students in secure environments.*

## Building Self-Contained JupyterLite Kernels

The `pyodide` Python kernel that is shipped by default in many JuoyterLite distributions provides a core Python with a wide range of additional scientific programming Python packages preinstalled.

The Pyodide kernel will thus be able to run a wide variety of Jupyter notebooks without the need to install any additional packages from remote third party package repositories. However, if the Pyodide kernel *does not* include a required package, installation of the package from a repository is required. To ship one or more additional packages inside the Pyodide kernel requires a custom build of pyodide that includes those packages.

The Xeus-Python kernel *cannot* currently install additional packages at run time. However, additional packages may be installed for use by the Xues-Python at the JupyterLite site build time. If Pyhton package requirments are identified in advance of the publication of the JupyterLite site, the required packages can be built into the deployed Xeus-Python kernel.

The WebR kernel loads all its packages from a WebR repository on demand. However, if we publish a package repository wihtin the JupyterLite website (that is, rooted on the path we publish JupyteLite to), we can configure the WebR kernel to load the packages from there and make a *de facto* self-contained distribution.

## Packaging a Standalone Desktop Distribution

Using a tool such as `pyinstaller`, we can easily create standalone, cross-platform executables that bundle a JupyterLite site with a simple webserver such as `flask`. This executable then contains all it needs in order to serve JupyterLite locally. Load the local URL into your browser, and then use the site as required, saving notebooks into local storage, or mounting them into the browser using the [`jupyterlab-filesystem-access](https://jupyterlite.readthedocs.io/en/latest/howto/content/filesystem-access.html) extension.

If our local webserver serves files from a local directory, we can also add notebooks to that directory and then read them into JupyterLite from the local URL used to serve the notebook contents of the local directory.
