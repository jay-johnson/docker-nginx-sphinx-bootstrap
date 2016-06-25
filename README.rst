===================================================================
Host Your Own Technical Blog with Docker + nginx + Sphinx Bootstrap
===================================================================

I built this repository_ for hosting my own technical blog + resume + work portfolio. It uses `docker compose`_ to deploy my nginx_ and sphinx-bootstrap_ containers and shares a mounted volume across the two containers. 

I use this repository for hosting:

http://jaypjohnson.com

Docker Hub Image(s): `jayjohnson/nginx`_ and `jayjohnson/sphinx-bootstrap`_

Date: **2016-06-24**

.. role:: bash(code)
      :language: bash

Overview
--------

I built this composition for hosting a nice-to-read blog that made it easy to generate content instead of battling formatting. Now I can write a post using `reStructuredText Markup`_ and the `python Sphinx bootstrap`_ documentation tooling converts each ``rst`` file into readable, static html which is hosted using the nginx_ container for HTTP traffic on port 80 (or 443) to the static html files. Once I containerized the sphinx-bootstrap-theme_ I added automatic integration with `Google Analytics`_ + `Google Search Console`_ for others looking to do the same thing. I like that out-of-the-box the sphinx-bootstrap-theme_ comes with support for `multiple bootswatch themes`_ and there are even more themes available from the `bootswatch repository`_ and `bootswatch website`_. Additionally it is nice to know that this blog is already mobile-ready because it is built using `bootstrap`_.

Integrating with Google Analytics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Set your `Google Analytics Tracking Code`_ to the ENV_GOOGLE_ANALYTICS_CODE_ environment variable during container creation

During container startup the environment variable ``ENV_GOOGLE_ANALYTICS_CODE`` will be `automatically installed into the default html layout`_ on every page across your site

Integrating with Google Search Console
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Automatic **sitemap.xml** creation

When the container starts or you `manually rebuild the html content`_ it will `automatically build`_ a ``sitemap.xml`` from any files ending with a ``.rst`` extension in the repository's root directory. This file is stored in the environment varaible ``ENV_DOC_OUTPUT_DIR`` directory. This is handy when you want to integrate your site into the `Google Search Console`_ and it should look similar to: http://jaypjohnson.com/sitemap.xml

What can I use it for?
~~~~~~~~~~~~~~~~~~~~~~

After working with the `Levvel team`_ over the past year, I realized the importance of having a blog to demonstrate technical expertise. After watching wordpress lose my work, I knew there had to be a better way. Recently, my friend `Alex`_ recommended I check out `Sphinx`_ because it made documentation even easier than traditional markdown. After finding the `python Sphinx bootstrap`_ repository I knew I wanted to drop this into a docker container so I could deploy content while keeping the nginx services up and running.

I now use this repository as `my blog`_ for `technical posts`_, hosting my `projects and stack discussions`_, `work history`_, resume_, and `contact information`_. I find it so much easier to write an ``rst`` file and let the framework translate it into fomatted, stylized html. Now I can focus on content instead of the presentation (which is handy because I am not a web developer or artist). 

Other interesting out-of-the-box features are:

* A native ``Search`` bar on each page
* Each page has a ``Source`` button in the navigation bar to quickly inspect the original ``rst`` markup data 

.. _docker compose: https://docs.docker.com/compose/
.. _Google Analytics: https://analytics.google.com/
.. _Google Search Console: https://www.google.com/webmasters/tools/
.. _Levvel team: http://levvel.io
.. _Alex: https://github.com/ajsmith
.. _Sphinx: http://www.sphinx-doc.org/en/stable/
.. _reStructuredText Markup: http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
.. _python Sphinx bootstrap: https://github.com/ryan-roemer/sphinx-bootstrap-theme
.. _sphinx-bootstrap-theme: https://github.com/ryan-roemer/sphinx-bootstrap-theme
.. _multiple bootswatch themes: https://github.com/ryan-roemer/sphinx-bootstrap-theme/blob/bfb28af310ad5082fae01dc1ff08dab6ab3fa410/demo/source/conf.py#L146-L150
.. _bootswatch website: http://bootswatch.com/
.. _bootswatch repository: https://github.com/thomaspark/bootswatch
.. _bootstrap: http://getbootstrap.com/
.. _Google Analytics Tracking Code: https://support.google.com/analytics/answer/1008080?hl=en
.. _ENV_GOOGLE_ANALYTICS_CODE : https://github.com/jay-johnson/docker-sphinx/blob/4c5cddf0b9edc4bee0ff3d673f5d7dd16a8336c5/docker/sphinx-bootstrap/Dockerfile#L47
.. _automatically installed into the default html layout: https://github.com/jay-johnson/docker-sphinx/4c5cddf0b9edc4bee0ff3d673f5d7dd16a8336c5/docker/sphinx-bootstrap/containerfiles/start-container.sh#L13-L14
.. _manually rebuild the html content: https://github.com/jay-johnson/docker-sphinx-bootstrap/blob/master/containerfiles/start-container.sh#L16-17
.. _automatically build: https://github.com/jay-johnson/docker-sphinx-bootstrap/blob/master/containerfiles/start-container.sh#L22-L42
.. _my blog: http://jaypjohnson.com
.. _technical posts : http://jaypjohnson.com/2016-06-24-configurable-docker-nginx.html
.. _projects and stack discussions: http://jaypjohnson.com/redis.html
.. _resume: http://jaypjohnson.com/_downloads/JayJohnson-Resume.pdf
.. _work history : http://jaypjohnson.com/work_history.html
.. _contact information: http://jaypjohnson.com/contact.html
.. _repository: https://github.com/jay-johnson/docker-nginx-sphinx-bootstrap
.. _nginx : https://hub.docker.com/r/jayjohnson/nginx/
.. _sphinx-bootstrap : https://hub.docker.com/r/jayjohnson/sphinx-bootstrap
.. _jayjohnson/nginx : https://hub.docker.com/r/jayjohnson/nginx/
.. _jayjohnson/sphinx-bootstrap : https://hub.docker.com/r/jayjohnson/sphinx-bootstrap
.. _start.sh: https://github.com/jay-johnson/docker-nginx/blob/master/containerfiles/start.sh
.. _start_container.sh: https://github.com/jay-johnson/docker-nginx/blob/master/containerfiles/start-container.sh
.. _base nginx.conf : https://github.com/jay-johnson/docker-nginx/blob/master/containerfiles/base_nginx.conf
.. _derived nginx.conf : https://github.com/jay-johnson/docker-nginx/blob/master/containerfiles/derived_nginx.conf
.. _properties.sh : https://github.com/jay-johnson/docker-nginx/blob/master/properties.sh
.. _docker-compose.yml: https://github.com/jay-johnson/docker-nginx-sphinx-bootstrap/blob/master/docker-compose.yml


Install and Setup
-----------------

I am running the docker containers on an Amazon EC2 t1.micro with a Route 53 dns alias record set to route ``jaypjohnson.com`` traffic to the micro.

On the EC2 micro I ran these commands to setup and deploy the site:

1. Create the ``/opt/blog`` directory

::

    $ mkdir -p /opt/blog/ && chmod 777 /opt/blog

2. Clone this repo

::

    $ cd /opt/blog
    $ git clone https://github.com/jay-johnson/docker-nginx-sphinx-bootstrap.git repo 
    $ cd repo
    $

3. Start the composition

::

    $ ./start_composition.sh
    Creating websphinx
    Creating webnginx
    Done
    $

4. Test the blog

::

    $ curl -s http://localhost:80 | grep Welcome | grep h2
    <h2>Welcome<a class="headerlink" href="#welcome" title="Permalink to this headline">¶</a></h2>
    $

Compose Environment Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use the following environment variables inside the docker-compose.yml_ file to configure the container startup behaviors:

+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| Variable Name                          | Purpose                                                            | Default Value                                               | 
+========================================+====================================================================+=============================================================+ 
| **ENV_BASE_NGINX_CONFIG**              | Provide a path to a `base nginx.conf`_                             | /root/containerfiles/base_nginx.conf                        | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_DERIVED_NGINX_CONFIG**           | Provide a path to a `derived nginx.conf`_                          | /root/containerfiles/derived_nginx.conf                     | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_DEFAULT_ROOT_VOLUME**            | Path to shared volume for static html, js, css, images, and assets | /opt/blog                                                   | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_DOC_SOURCE_DIR**                 | Input directory where Sphinx processes ``rst`` files               | /opt/blog/repo/source                                       | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_DOC_OUTPUT_DIR**                 | Output directory where Sphinx will output the ``html`` files       | /opt/blog/repo/release                                      | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_BASE_DOMAIN**                    | Your web domain like: ``http://jayjohnson.com``                    | http://jaypjohnson.com                                      | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 
| **ENV_GOOGLE_ANALYTICS_CODE**          | Your Google Analytics Tracking Code like: ``UA-79840762-99``       | UA-79840762-99                                              | 
+----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------------+ 

.. warning:: Please make sure the **nginx** and **sphinx-bootstrap** containers use the **same base** ``ENV_DEFAULT_ROOT_VOLUME`` directory and that the ``rst`` files are stored inside the ``ENV_DOC_SOURCE_DIR`` and the html output files can be written to the ``ENV_DOC_OUTPUT_DIR`` directory


Here is how my EC2 host has the shared directory set up

::

   $ ls /opt/blog/repo/
   docker-compose.yml  Makefile  nginxssh.sh  README.rst  source  sphinxssh.sh  start_composition.sh  stop_composition.sh
   $

.. note:: The **release** directory will not be present until you start the composition the first time


Want to add a new blog post?
----------------------------

1. Open a new ``new-post.rst`` file in the ``source`` directory

2. Add the following lines to the new ``new-post.rst`` file:

::

    ==================
    This is a New Post
    ==================

    My first blog post


3. Edit the ``index.rst`` file and find the ``Site Contents`` section

4. Add a new line to ``Site Contents`` **toctree** section containing: ``new-post`` 

Here is how mine looks after adding it to the ``index.rst``

::

    Site Contents
    -------------

    .. toctree::
        :maxdepth: 2

        new-post
        python
        work-history
        contact
        about


.. note:: One nice feature of the sphinx framework is it will automatically label the link with the first **Title** inside the file.

5. Save the ``index.rst`` file

6. Deploy and Rebuild the html files

Inside the ``websphinx`` container I included a deploy + rebuild script you can run from outside the container with:

::

    $ docker exec -it websphinx /root/containerfiles/deploy-new-content.sh

7. Test the new post shows up in the site

::

    $ curl -s http://localhost:80/ | grep href | grep toctree | grep "New Post"
    <li class="toctree-l1"><a class="reference internal" href="new-post.html">This is a New Post</a></li>
    <li class="toctree-l1"><a class="reference internal" href="new-post.html">This is a New Post</a></li>
    $

Stopping the site
~~~~~~~~~~~~~~~~~

To stop the site run:

::

    $ ./stop_composition.sh 
    Stopping the Composition
    Stopping webnginx ... done
    Stopping websphinx ... done
    Done
    $



Cleanup the site containers
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to stop and cleanup the site and docker containers run these commands:

1. Check the site containers are running

::

    $ docker ps -a
    CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                                      NAMES
    f095da56839f        jayjohnson/nginx              "/root/containerfiles"   About an hour ago   Up About an hour    0.0.0.0:82->80/tcp, 0.0.0.0:444->443/tcp   webnginx
    b2f9d5dd915a        jayjohnson/sphinx-bootstrap   "/root/containerfiles"   About an hour ago   Up About an hour                                               websphinx
    $

2. Stop the composition

::

    $ ./stop_composition.sh 
    Stopping the Composition
    Stopping webnginx ... done
    Stopping websphinx ... done
    Done
    $

3. Remove the containers

::

    $ docker rm webnginx websphinx
    webnginx
    websphinx
    $

4. Remove the container images

::

    $ docker rmi jayjohnson/nginx jayjohnson/sphinx-bootstrap


5. Remove the blog directory

:: 

    $ rm -rf /opt/blog/repo

Licenses
--------

This repository is licensed under the MIT license.

The nginx license: http://nginx.org/LICENSE

Sphinx Bootstrap Theme is licensed under the MIT license.

Bootstrap v2 is licensed under the Apache license 2.0.

Bootstrap v3.1.0+ is licensed under the MIT license.
