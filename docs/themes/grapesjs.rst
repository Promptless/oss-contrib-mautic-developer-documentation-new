GrapesJS Builder
################

MJML
****

The GrapesJS Builder doesn't require any special HTML syntax to edit content in the Builder. However, for Emails, it supports the :xref:`MJML email framework` to create responsive emails.

.. code-block:: html

    <mjml>
      <mj-body>
        <mj-raw>
          <!-- Company Header -->
        </mj-raw>
        <mj-section background-color="#f0f0f0">
          <mj-column>
            <mj-text font-style="bold" font-size="24px" color="#6f6f6f">My Company</mj-text>
          </mj-column>
        </mj-section>
        <mj-raw>
          <!-- Confirm  text -->
        </mj-raw>
        <mj-section background-color="#fafafa">
          <mj-column width="400px">
            <mj-text font-style="bold" font-size="22px" font-family="Helvetica Neue" color="#626262">Please confirm your subscription!</mj-text>
            <mj-button background-color="#F45E43" font-style="bold" href="#">Yes, subscribe me to the list</mj-button>
            <mj-text color="#525252" font-size="16" line-height="1.5">If you received this email by mistake, simply delete it. You won't be subscribed if you don't click the confirmation link above.<br/><br/>For questions about this list, please contact:
    email@example.com</mj-text>
          </mj-column>
        </mj-section>
            <mj-raw>
          <!-- Confirm  text -->
        </mj-raw>
            <mj-section background-color="#fafafa">
          <mj-column width="400px">
            <mj-text color="#525252" line-height="1.2">
              <p>Company Name<br/>111 Amazing Street<br/>
                Beautiful City</p></mj-text>

          </mj-column>
        </mj-section>
      </mj-body>
    </mjml>

.. _grapesjs-theme-tokens:

MJML theme tokens
*****************

.. versionadded:: 5.2
   Theme token inheritance for MJML templates.

Mautic supports a token-based theming system for MJML Email templates. This allows you to create reusable, consistent styles across your Email themes while enabling the GrapesJS Builder to apply theme defaults to newly dropped blocks.

Understanding theme tokens
==========================

Theme tokens use MJML's native ``<mj-attributes>`` and ``<mj-class>`` system to define reusable styles. When you drag a block from the Builder panel, Mautic automatically applies the appropriate theme tokens, ensuring visual consistency.

Token naming conventions
------------------------

Follow these rules when naming theme tokens:

- Prefix all tokens with ``t-`` to indicate they're theme tokens
- Use kebab-case for multi-word names (for example, ``t-btn-primary``, not ``tBtnPrimary``)
- Keep token names semantic and portable between themes (for example, ``t-surface-1`` instead of ``t-brand-blue``)

Required tokens
---------------

Your theme must define these tokens for the Builder to work correctly:

``t-body``
    Base text styling for body content (font-size, line-height)

``t-btn``
    Base button shape and padding (inner-padding, border-radius)

``t-btn-primary``
    Primary button colors (background-color, color)

``t-section``
    Section layout padding

``t-surface-1``
    Primary surface background color

If your theme omits any required token, the Builder falls back to GrapesJS/MJML defaults, which may not match your theme.

Recommended optional tokens
---------------------------

These tokens provide additional styling options:

- ``t-lead`` - Lead paragraph styling (larger font-size, line-height)
- ``t-h1``, ``t-h2``, ``t-h3``, ``t-h4`` - Heading styles
- ``t-btn-secondary`` - Secondary button colors
- ``t-surface-2`` - Secondary surface background color

Theme skeleton template
=======================

Use this MJML template as a starting point for new themes:

.. code-block:: html

    <mjml>
      <mj-head>
        <mj-attributes>
          <!-- Global defaults (required) -->
          <mj-all font-family="Arial, sans-serif" color="#111827"></mj-all>
          <mj-text font-size="16px" line-height="24px" padding="0"></mj-text>
          <mj-button font-weight="600" border-radius="6px" padding="0"></mj-button>
          <mj-section padding="24px 0"></mj-section>
          <mj-column padding="0"></mj-column>

          <!-- REQUIRED TOKENS -->
          <mj-class name="t-body" font-size="16px" line-height="24px"></mj-class>

          <mj-class name="t-btn" inner-padding="12px 18px"></mj-class>
          <mj-class name="t-btn-primary" background-color="#2563EB" color="#FFFFFF"></mj-class>

          <mj-class name="t-section" padding="32px 24px"></mj-class>
          <mj-class name="t-surface-1" background-color="#FFFFFF"></mj-class>

          <!-- RECOMMENDED OPTIONAL TOKENS -->
          <mj-class name="t-lead" font-size="18px" line-height="28px"></mj-class>
          <mj-class name="t-h1" font-size="32px" line-height="38px" font-weight="700"></mj-class>
          <mj-class name="t-h2" font-size="24px" line-height="30px" font-weight="700"></mj-class>
          <mj-class name="t-h3" font-size="18px" line-height="24px" font-weight="700"></mj-class>
          <mj-class name="t-h4" font-size="16px" line-height="22px" font-weight="700"></mj-class>

          <mj-class name="t-btn-secondary" background-color="#E5E7EB" color="#111827"></mj-class>
          <mj-class name="t-surface-2" background-color="#F3F4F6"></mj-class>
        </mj-attributes>

        <!-- Optional non-critical CSS (keep minimal) -->
        <mj-style inline="inline">
          p, li { margin: 0; padding: 0; }
        </mj-style>
      </mj-head>

      <mj-body background-color="#F3F4F6">
        <!-- Your template content -->
      </mj-body>
    </mjml>

Theme token best practices
==========================

Use MJML-native theming first
-----------------------------

Prefer defining defaults using MJML's built-in attribute tags:

- ``<mj-all>`` for global font and base color
- ``<mj-text>`` for base text size, line-height, and padding
- ``<mj-button>`` for base button shape, weight, and padding
- ``<mj-section>`` and ``<mj-column>`` for layout padding defaults

Then layer semantic variations using ``<mj-class>`` tokens.

Keep tokens non-destructive
---------------------------

Tokens should set styling properties that don't break layout:

- ``font-size``, ``line-height``, ``font-weight``
- ``background-color``, ``color``
- ``padding``, ``inner-padding``
- ``border-radius``

Avoid adding layout-breaking properties to typography tokens, such as excessive padding inside ``t-h1``.

Avoid relying on mj-style for critical appearance
-------------------------------------------------

Some Email clients strip or inconsistently apply ``<mj-style>`` rules. Use it only for small, safe resets:

.. code-block:: html

    <mj-style inline="inline">
      p { margin: 0; }
    </mj-style>

Don't use ``<mj-style>`` as the only definition of core typography, button colors, or layout spacing

GrapesJS Builder Plugins
************************

From Mautic 5.1 it's possible to create Plugins for the GrapesJS Builder. This allows you to add custom blocks, Components, and styles to the Builder. It's how the GrapesJS Preset works, which ships with Mautic.

This uses the :xref:`grapesjs-plugins` feature. Read more about the potential this unlocks in the :xref:`grapesjs-api`.

.. vale off

Creating a Plugin for GrapesJS
===============================

.. vale on

To create a Plugin for the GrapesJS Builder, you need to create a new Bundle in Mautic. This contains the Plugin and any other related code.

1. Create a new Bundle in Mautic, for example ``GrapesJSCustomPluginBundle``.
2. Create a GrapesJS Plugin - for example ``.Assets/src/index.ts`` - as follows. Note this uses TypeScript but vanilla JS also works:

.. code-block:: typescript

        import grapesjs from 'grapesjs';

        // declare type for window so TS will not complain during compiling
        declare global {
            interface Window {
                MauticGrapesJsPlugins: object[];
            }
        }

        export type PluginOptions = {
        };

        export type RequiredPluginOptions = Required<PluginOptions>;

        const GrapesJsCustomPlugin: grapesjs.Plugin<PluginOptions> = (editor, opts: Partial<PluginOptions> = {}) => {
            const options: RequiredPluginOptions = {
                ...opts
            };
            console.log('Run GrapesJsCustomPlugin...')
            console.log('Options passed to GrapesJsCustomPlugin:', options)
            editor.on('load', () => {
                console.log('GrapesJsCustomPlugin: editor.onLoad()')
            });
        }

        // export the plugin in case someone wants to use it as source module
        export default GrapesJsCustomPlugin;

        // create a global window-object which holds the information about GrapesJS plugins
        if (!window.MauticGrapesJsPlugins) window.MauticGrapesJsPlugins = [];
        // add the plugin-function with a name to the window-object
        window.MauticGrapesJsPlugins.push({
            name: 'GrapesJsCustomPlugin', // required
            plugin: GrapesJsCustomPlugin, // required
            context: ['page', 'email-mjml'], // optional. default is none/empty, so the plugin is always added; options: [page|email-mjml|email-html]
            pluginOptions: { options: { test: true, hello: 'world'} } // optional
        })

Due to the ``export default``, you can use this Plugin in a fork, customizing the source files with ``import GrapesJSCustomPlugin from 'path'``. But this isn't required - you can also write a plain JS function as described in the :xref:`grapesjs-plugins` documentation.

3. Add the JavaScript file - compiled or source - to the ``AssetSubscriber`` of your Plugin bundle:

.. code-block:: php

      public function injectAssets(CustomAssetsEvent $assetsEvent): void
    {
        if ($this->config->isPublished()) {
            $assetsEvent->addScript('plugins/GrapesJsCustomPluginBundle/Assets/dist/index.js');
        }
    }

The resulting HTML source appears as follows:

.. code-block:: html

  <script src="/plugins/GrapesJsCustomPluginBundle/Assets/dist/index.js?v6e9fccee" data-source="mautic"></script>
  <script src="/plugins/GrapesJsBuilderBundle/Assets/library/js/dist/builder.js?v6e9fccee" data-source="mautic"></script>

.. note::
  The Plugin code loads before ``builder.js`` which results in the data registering in the global window object.

You can download a :xref:`GrapesJS Demo Plugin` to get you started.

.. _grapesjs-custom-blocks:

Adding custom blocks
********************

.. versionadded:: 5.2
   Independent Plugin pattern for adding custom GrapesJS blocks.

You can create independent Mautic Plugins that add custom blocks to the GrapesJS Builder. This approach uses the ``window.MauticGrapesJsPlugins`` array and doesn't require a build step - plain JavaScript works.

Plugin file structure
=====================

Create an independent Plugin with this structure:

.. code-block:: text

    plugins/CustomBlocksBundle/
    ├── CustomBlocksBundle.php
    ├── Config/
    │   ├── config.php
    │   └── services.php
    ├── EventListener/
    │   └── GrapesJsInjectSubscriber.php
    └── Assets/
        └── js/
            └── grapesjs.customBlocks.js

JavaScript: Register blocks
===========================

Create a JavaScript file that registers with ``window.MauticGrapesJsPlugins`` and adds blocks using GrapesJS's ``BlockManager``:

.. code-block:: javascript

    // plugins/CustomBlocksBundle/Assets/js/grapesjs.customBlocks.js
    (function () {
      window.MauticGrapesJsPlugins = window.MauticGrapesJsPlugins || [];

      // Avoid double-registration if the asset is injected multiple times
      const alreadyRegistered = window.MauticGrapesJsPlugins.some(
        p => p && p.name === 'customblocks-mjml-blocks'
      );
      if (alreadyRegistered) return;

      window.MauticGrapesJsPlugins.push({
        name: 'customblocks-mjml-blocks',
        context: ['email-mjml'],
        plugin: (editor, opts = {}) => {
          const bm = editor.BlockManager;

          // Block: Section (Surface 2)
          const sectionSurface2Id = 'customblocks-section-surface-2';
          if (!bm.get(sectionSurface2Id)) {
            bm.add(sectionSurface2Id, {
              label: 'Section (Surface 2)',
              category: 'Custom Blocks',
              content:
                '<mj-section mj-class="t-section t-surface-2">' +
                  '<mj-column>' +
                    '<mj-text mj-class="t-body">Section content...</mj-text>' +
                  '</mj-column>' +
                '</mj-section>',
              media: `<svg viewBox="0 0 24 24">
                <path fill="currentColor" d="M4 6h16v4H4V6zm0 8h16v4H4v-4z"/>
              </svg>`,
            });
          }

          // Block: Secondary Button
          const secondaryButtonId = 'customblocks-button-secondary';
          if (!bm.get(secondaryButtonId)) {
            bm.add(secondaryButtonId, {
              label: 'Secondary Button',
              category: 'Custom Blocks',
              content: '<mj-button mj-class="t-btn t-btn-secondary" href="https://">Secondary Button</mj-button>',
              media: `<svg viewBox="0 0 24 24">
                <path fill="currentColor" d="M7 7h10a4 4 0 0 1 0 8H7a4 4 0 0 1 0-8Zm0 2a2 2 0 0 0 0 4h10a2 2 0 0 0 0-4H7Z"/>
              </svg>`,
            });
          }
        },
      });
    })();

Block registration options
--------------------------

``name``
    Unique identifier for your Plugin (required)

``context``
    Array of Builder contexts where your Plugin loads. Options: ``page``, ``email-mjml``, ``email-html``. Leave empty to load in all contexts.

``plugin``
    Function receiving the GrapesJS editor instance (required)

When adding blocks, always check if the block exists before adding:

.. code-block:: javascript

    const myBlockId = 'customblocks-my-block';
    if (!bm.get(myBlockId)) {
      bm.add(myBlockId, {
        label: 'My Block Label',
        category: 'Custom Blocks',
        content: '<mj-... mj-class="t-...">...</mj-...>',
        media: `<svg viewBox="0 0 24 24">...</svg>`,
      });
    }

Use theme tokens in block content
---------------------------------

Reference :ref:`theme tokens <grapesjs-theme-tokens>` in your block MJML content to inherit the active theme's styling:

.. code-block:: javascript

    content: '<mj-section mj-class="t-section t-surface-2">...</mj-section>'

PHP: Inject JavaScript on Builder pages
=======================================

Create an event subscriber that injects your JavaScript when the Builder loads:

.. code-block:: php

    <?php
    // plugins/CustomBlocksBundle/EventListener/GrapesJsInjectSubscriber.php

    declare(strict_types=1);

    namespace MauticPlugin\CustomBlocksBundle\EventListener;

    use Mautic\CoreBundle\CoreEvents;
    use Mautic\CoreBundle\Event\CustomContentEvent;
    use Mautic\CoreBundle\Helper\AssetsHelper;
    use Symfony\Component\EventDispatcher\EventSubscriberInterface;
    use Symfony\Component\HttpFoundation\RequestStack;

    class GrapesJsInjectSubscriber implements EventSubscriberInterface
    {
        public function __construct(
            private AssetsHelper $assetsHelper,
            private RequestStack $requestStack
        ) {
        }

        public static function getSubscribedEvents(): array
        {
            return [
                CoreEvents::VIEW_INJECT_CUSTOM_CONTENT => ['onInjectCustomContent', 0],
            ];
        }

        public function onInjectCustomContent(CustomContentEvent $event): void
        {
            $request = $this->requestStack->getCurrentRequest();
            if (!$request) {
                return;
            }

            // Inject only on Email Builder-related routes/pages
            $route = (string) $request->attributes->get('_route');
            $path  = (string) $request->getPathInfo();

            $isEmailArea =
                str_contains($route, 'mautic_email') ||
                str_contains($path, '/emails');

            if (!$isEmailArea) {
                return;
            }

            $src = $this->assetsHelper->getUrl(
                'plugins/CustomBlocksBundle/Assets/js/grapesjs.customBlocks.js'
            );

            // Inject near the end of the body so window.MauticGrapesJsPlugins
            // is available before Builder init
            $event->addContent(
                sprintf('<script src="%s"></script>', $src),
                CustomContentEvent::LOCATION_BODY_END
            );
        }
    }

Register the event subscriber
=============================

Register your subscriber in the services configuration:

.. code-block:: php

    <?php
    // plugins/CustomBlocksBundle/Config/services.php

    declare(strict_types=1);

    return [
        'services' => [
            'events' => [
                'customblocks.grapesjs.inject.subscriber' => [
                    'class'     => \MauticPlugin\CustomBlocksBundle\EventListener\GrapesJsInjectSubscriber::class,
                    'arguments' => [
                        'mautic.helper.assets',
                        'request_stack',
                    ],
                ],
            ],
        ],
    ];

Bundle and configuration files
==============================

Create the bundle class:

.. code-block:: php

    <?php
    // plugins/CustomBlocksBundle/CustomBlocksBundle.php

    declare(strict_types=1);

    namespace MauticPlugin\CustomBlocksBundle;

    use Mautic\PluginBundle\Bundle\PluginBundleBase;

    class CustomBlocksBundle extends PluginBundleBase
    {
    }

Create the Plugin configuration:

.. code-block:: php

    <?php
    // plugins/CustomBlocksBundle/Config/config.php

    declare(strict_types=1);

    return [
        'name'        => 'CustomBlocks',
        'description' => 'Adds custom GrapesJS MJML blocks.',
        'version'     => '1.0.0',
        'author'      => 'Your Company',
    ];

Activating the Plugin
=====================

After creating all files, activate your Plugin:

.. code-block:: bash

    bin/console cache:clear
    bin/console mautic:plugins:reload

.. note::
   You may need to hard-refresh your browser (Ctrl+Shift+R / Cmd+Shift+R) to clear cached JavaScript assets.