OMERO.omero_vitessce
=======================

OMERO Vitessce multimodal data viewer.

The app was made from the https://github.com/will-moore/omero-vitessce, but it is looking into expanding: 
https://github.com/will-moore/omero-vitessce

Installation
============

Install `omero_vitessce` in development mode as follows:

    # within your python venv:
    $ cd omero-vitessce
    $ pip install -e .

Add the app to the `omero.web.apps` setting:

N.B. Here we use single quotes around double quotes:

    $ omero config append omero.web.apps '"omero_vitessce"'

Optionally, add a link "OMERO Vitessce" at the top of the webclient to
open the index page of this app:

    $ omero config append omero.web.ui.top_links '["OMERO Vitessce", "omero_vitessce_index", {"title": "Open OMERO Vitessce in new tab", "target": "_blank"}]'


Now restart your `omero-web` server and go to
<http://localhost:4080/omero_vitessce/> in your browser.

