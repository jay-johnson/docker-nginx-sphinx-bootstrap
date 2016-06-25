========================================
Your Blog Title
========================================

Welcome
-------

This is a summary statement about this site.

Posts
=====
* **2016-06-25** - `Sample Post`_

.. _Sample Post: ./2016-06-25-sample-post.html

More on the author
==================
* `Contact Information`_
* `About Me`_
* `Work History`_

.. _Work History: ./work-history.html
.. _Contact Information: ./contact.html
.. _About Me: ./about.html

References and Sources
======================
This site runs with two Docker containers deployed and managed via Docker Compose. The first container is nginx_ and the second container is sphinx-bootstrap_. Each container shares a mounted volume hosting the static html and rst files. I built this site using the Sphinx_ theme_ that `Ryan Roemer`_ built that integrates the Bootstrap_ CSS / JavaScript framework with various layout options, hierarchical menu navigation, and mobile-friendly responsive design. It is configurable, extensible and can use any number of different Bootswatch_ CSS themes.

.. _nginx: /sphinx-doc.or://hub.docker.com/r/jayjohnson/nginx/
.. _sphinx-bootstrap: https://hub.docker.com/r/jayjohnson/sphinx-bootstrap/
.. _Sphinx: http://sphinx-doc.org/
.. _theme: http://sphinx-doc.org/theming.html
.. _Ryan Roemer: https://github.com/ryan-roemer/sphinx-bootstrap-theme
.. _Bootstrap: http://getbootstrap.com/
.. _Bootswatch: http://bootswatch.com


Site Contents
-------------

.. toctree::
    :maxdepth: 2

    2016-06-25-sample-post
    python
    redis
    docker
    work-history
    contact
    about

Searching
---------

* :ref:`search`

