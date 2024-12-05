# Technical Overview

[JupyterLite](https://github.com/jupyterlite/jupyterlite) is a WebAssembly (WASM) compiled distribution of JupyterLab, Jupyter Notebooks and a Jupyter REPL.

JupyterLite runs solely in the browser — no backend server or installation required. Kernels for Python, JavaScript and R are all available.

JupyterLite distributions can be served as static web pages using a simple web browser. JupyterLite provides the same, comprehensive interactive user environment as "traditional" Jupyter server published environments.

Notebooks can be create and edited and changes saved to browser storage, allowing modifications to be persisted over multiple sessions as long as the same browser is used to access the site.

Files including notebooks can be shipped as part of the distribution. Updates made to such notebooks are saved to browser storage; deleting the notebook then resets its state to the originally shipped version.

JupyterLite can be "installed" into a browser as a progressive web app, supporting offline use.

A JupyerLite distribution can also be bundled with a simple webserver using `pyinstaller` and then distributed as a 