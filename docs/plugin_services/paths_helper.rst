Paths helper
############

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use ``Mautic\CoreBundle\Helper\PathsHelper`` to resolve Mautic's system paths - images, Themes, cache, and more - instead of hard-coding directory locations. Inject the helper into your service:

.. code-block:: php

    <?php

    use Mautic\CoreBundle\Helper\PathsHelper;

    final class ExampleService
    {
        public function __construct(private PathsHelper $pathsHelper)
        {
        }

        public function resolveImagePaths(): void
        {
            // Relative path, for example 'media/images'
            $relativeImagesDir = $this->pathsHelper->getSystemPath('images');

            // Absolute path, for example '/var/www/html/media/images'
            $absoluteImagesDir = $this->pathsHelper->getSystemPath('images', true);
        }
    }

Pass ``true`` as the second argument to ``getSystemPath()`` to return an absolute path. ``PathsHelper`` also provides convenience methods such as ``getCachePath()``, ``getRootPath()``, and ``getThemesPath()``.

.. tip::

   Use the ``tmp`` - or ``temporary`` - path to store temporary files. Prefer it over the general ``cache`` location for that purpose.
