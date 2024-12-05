# Embedding JupyterLab and Jupyter Notebook in Moodle VLE Pages

A browser based JupyterLab enviroment can be embedded into a Moodle VLE page as an embedded URL, or bundled into an `HTML5.zip` activity that can either be rendered in an `HTML5` activity page, or embedded in a page rendered from OU-XML.

## Uploading the HTML5.zip asset

In a Moodle VLE course home page, enable editing settings:

![Enable edits in Moodle course](images/moodle_enable_edit.png)

In a section, click to `Add an activity or resource`:

![Click to "Add an activity or resource"](images/moodle_click_add_activity.png)

Select the `HTML activity` type:

![Panel of new activity options - select "HTML activity"](images/moodle-add-html5-resource.png)

Give the page a sensible name and ID:

![Creating an HTML5 package - initial settings](images/html5upload.png)

For the settings, set the `height` and `width` to `*`.

![Creating an HTML5 package - height and width set *, availability "show on course page" ](images/html5settings.png)

*It might make sense to also tick the `Full window` checkbox?*

Where notebooks are bundled into the JupyterLite distribution, the JupyterLab or Jupyter notebook environment can be launched with a pre-opened notebook by setting `lab/index.html?path=NOTEBOOK.ipynb` in the URL path. If the [`jupyterlab-open-url-parameter`](https://github.com/jupyterlab-contrib/jupyterlab-open-url-parameter) extension is installed, notebooks can also be opened from a URL by setting one or more `&fromURL=NOTEBOOKURL` URL parameters.

![JupyterLab UI in embedded page](images/example_embedded_jupyterlab.png)
