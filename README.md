![Badge_OMERO](https://github.com/MicheleBortol/omero-vitessce/actions/workflows/omero_plugin.yml/badge.svg)
![Badge_PyPI](https://img.shields.io/pypi/v/omero-vitessce?label=pypi%20package)


OMERO.omero_vitessce
=======================

OMERO Vitessce multimodal data viewer.

Installation
============

This section assumes that an OMERO.web is already installed.

Install the app using [pip](https://pip.pypa.io/en/stable/):

NB: You need to ensure that you are running `pip` from the python environment
where `omero-web` is installed.

    $ pip install -U omero-vitessce

Add the app to the `omero.web.apps` setting:

    $ omero config append omero.web.apps '"omero_vitessce"'

Add `omero_vitessce` as a tab in the right-hand-side panel:

    $ omero config append omero.web.ui.right_plugins '["Vitessce", "omero_vitessce/webclient_plugins/right_plugin.vitessce.js.html", "vitessce_tab"]'

Add `omero_vitessce` as an open-with option:

    omero config append omero.web.open_with '["omero_vitessce", "open_vitessce", {"supported_objects": ["project", "dataset", "image"], "label": "Vitessce"}]'

Add the omero web address (replace ´'"http://localhost:4080"'´ with your address):

    $ omero config set omero.web.omero_vitessce.serveraddress '"http://localhost:4080"'
    
Now restart OMERO.web as normal for the configuration above to take effect.

### omero-web-zarr
While [omero-web-zarr](https://github.com/ome/omero-web-zarr) is not a requirement, but it is strongly recommended to have it installed and enabled for using this plugin
omero-web-zarr is necessary to take advantage fully of the autogenerated config files for the Vitessce viewer,
as they rely on omero-web-zarr to serve the images to the viewer.

Please see https://pypi.org/project/omero-web-zarr/ for instructions on installing and enabling the omero-web-zarr plugin.

Usage
============

### Selecting the viewer
The plugin can be used with:
- `Vitessce` Tab on the right hand side panel.
- `Open with` `->` `Vitessce` on the left side panel. 

#### Right-hand-side panel:
- **Prexisting config files**:  
![Right-Config](https://github.com/user-attachments/assets/b8fea5f0-1d8f-4578-a577-8744d32dcaa5)   
Clicking the link will open a new tab with the viewer displaying the selected configuration.
- **Autogenerating config file**:  
![Right-Auto](https://github.com/user-attachments/assets/8dfa22cc-dbb5-493d-b837-d609a11a1346)  
Clicking the button will autogenerate a config file and attach it to the dataset/image, and the viewer displaying the autogenerated configuration.  
  
#### Open-with:
Right click on a `dataset` or an `images` in the left-panel and select `open-with` -> `Vitessce`.  
This will open the vitessce viewer in a new tab using the first configuration file attachement.
![Openwith-image](https://github.com/user-attachments/assets/f75cc27c-4cd3-4a19-ae20-7d8f0a47aa87)

### Viewer window
After using a The Vitessce viewer is opened in a new tab:
![image](https://github.com/user-attachments/assets/77de517e-67e4-42d6-a2b6-2a3a6a11dc7e)

### Config files
The omero-vitessce plugin considers all files with the `.json.txt` extension attached to the image or dataset as config files for the viewer.
The config files should follow the vitessce view-config-json format: http://vitessce.io/docs/view-config-json/

#### Autogenerated config files
Config files can be autogenerated for:
- `images`: A spatial view and a layer controller view are setup for the image.
- `datasets`: All images in the dataset are bundled togheter and a spatial view and a layer controller are setup.

Assumes that the images are in the OME-NGFF format v0.4 and that [omero-web-zarr](https://github.com/ome/omero-web-zarr) is
available to serve the images.

#### Custom config files
Custom config files should have a `.json.txt` extension and added as attachements to a dataset or an image.
The configuration files does not need to refer to the dataset / image it is attached to and can refer to other images.

For how to create a custom config file see:
- [VItessce Docs](http://vitessce.io/docs/view-config-json/)
- [Vitessce Examples](http://vitessce.io/examples/)

#### Serving the images / data /metadata
Images and data can be served through:
- [omero-web-zarr](https://pypi.org/project/omero-web-zarr/):  OME-NGFF images only.
- [omero-openlink](https://github.com/sukunis/OMERO.openlink): all images and file attachements.
- `webclient/annotation/` endpoint: only for Annotations (useful for file attachments).

Development
=======================

## Sources

The main sources are:
- cookiecutter-omero-webapp: https://github.com/ome/cookiecutter-omero-webapp
- `react_webapp` from omero-web-apps-examples: https://github.com/ome/omero-web-apps-examples/tree/master/react-webapp
- Vitessce python package used for generating config files http://python-docs.vitessce.io/
- omero-web-zarr to serve OME-NGFF images: https://github.com/ome/omero-web-zarr
- omero-openlink to serve images and other files: https://github.com/sukunis/OMERO.openlink

## React web app

### omero_vitessce web app

We serve a custom version of the vitesce app: http://vitessce.io/

The app sets up a vitessce view configured through a text file in json format: http://vitessce.io/docs/view-config-json/

The configuration file is taken from the `?config` url parameter.

### Installation

This project was created with [Create React App](https://github.com/facebook/create-react-app).

You can run this project in development mode or as an OMERO.web Django app.

To get started:

    $ cd vitessce_app
    $ npm install

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br>
You will also see any lint errors in the console.


### `npm run build`

Builds the app for production to the `build` folder and copies the
html and static files to the Django app in `vitessce app`. See the [deploy_build.sh]() script.<br>

You also need to install the app into your `omero-web` environment:

    # cd to the root of the repo
    $ pip install -e .

You will need to have the app configured in your OMERO.web install:

    $ omero config append omero.web.apps '"omero_vitessce"'
    $ omero config append omero.web.ui.right_plugins '["Vitessce", "omero_vitessce/webclient_plugins/right_plugin.vitessce.js.html", "vitessce_tab"]'
    $ omero config set omero.web.omero_vitessce.serveraddress '"http://localhost:4080"'

It bundles React in production mode and optimizes the build for the best performance.

See the Create React App section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

