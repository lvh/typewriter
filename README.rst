typewriter
==========

`typewriter`_ turns your README into a `github page`_.

.. _`typewriter`: https://github.com/lvh/typewriter 
.. _`github page`: http://pages.github.com/

.. image:: http://project-logos.lvh.cc/typewriter.png
    :align: center

Usage
-----

Run ``typewriter`` from a clean git repository. Pass it a template (a git repository) with the ``-t`` flag::

    typewriter -t [template]

The repository specification is parsed using git, or `hub`_ if it's available.

.. _`hub`: https://github.com/defunkt/hub

Themes
------

Themes are git repositories. They're cloned into a temporary directory and built using a Makefile. The ``gh-pages`` branch is checked out in the current repository. The working tree is cleaned up, assuming it was built by typewriter before (which is checked by the presence of ``.typewriter``). The built theme is copied and committed in, and the result is pushed back to Github.

A theme has a ``src/`` folder. The ``README`` file is copied into that folder. typewriter then runs ``make build``, and expects the contents in the ``build/`` folder.

Take a look at `ttlvh`_ for an example of a theme. It's the one used to render the `github page for typewriter itself`_, amongst other things.

.. _`ttlvh`: https://github.com/lvh/ttlvh
.. _`github page for typewriter itself`: http://lvh.github.com/typewriter
