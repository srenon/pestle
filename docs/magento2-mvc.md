## generate:route



    Usage:
        $ pestle.phar magento2:generate:route

    Arguments:

    Options:

    Help:
        Creates a Route XML
        generate_route module area id
        @command magento2:generate:route
        @argument module_name Which Module? [Pulsestorm_HelloWorld]
        @argument area Which Area (frontend, adminhtml)? [frontend]
        @argument frontname Frontname/Route ID? [pulsestorm_helloworld]
        @argument controller Controller name? [Index]
        @argument action Action name? [Index]

The `magento2:generate:route` command generates all the configuration and code you'll need to make Magento respond to a URL.

**Interactive Invocation**

    $ pestle.phar magento2:generate:route
    Which Module? (Pulsestorm_HelloWorld)] Pulsestorm_Pestle
    Which Area (frontend, adminhtml)? (frontend)] frontend
    Frontname/Route ID? (pulsestorm_helloworld)] pulsestorm_pestle
    Controller name? (Index)] Index
    Action name? (Index)] Index
    /path/to/m2/app/code/Pulsestorm/Pestle/etc/frontend/routes.xml
    /path/to/m2/app/code/Pulsestorm/Pestle/Controller/Index/Index.php

**Argument Invocation**

    $ pestle.phar magento2:generate:route Pulsestorm_Pestle frontend pulsestorm_pestle Index Index

The `magento2:generate:route` command will ask you which module you want your controller and route configuration created in (`Pulsestorm_Pestle`), whether this is a cart (`frontend`) or admin-area (`adminhtml`) route, and the short class name versions of the controller and action names (both `Index` above).  With that information in hand, pestle will create a route configuration file.

    $ cat app/code/Pulsestorm/Pestle/etc/frontend/routes.xml
    <?xml version="1.0"?>
    <config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
        <router id="standard">
            <route id="pulsestorm_pestle" frontName="pulsestorm_pestle">
                <module name="Pulsestorm_Pestle"/>
            </route>
        </router>
    </config>

and a controller class file

    $ cat app/code/Pulsestorm/Pestle/Controller/Index/Index.php
    <?php
    namespace Pulsestorm\Pestle\Controller\Index;
    class Index extends \Magento\Framework\App\Action\Action
    {

        protected $resultPageFactory;
        public function __construct(
            \Magento\Framework\App\Action\Context $context,
            \Magento\Framework\View\Result\PageFactory $resultPageFactory)
        {
            $this->resultPageFactory = $resultPageFactory;
            parent::__construct($context);
        }

        public function execute()
        {
            return $this->resultPageFactory->create();
        }
    }

In the above example, your page would be available at the URLs

    http://your-site.example.com/pulsestorm_pestle/index/index

    # or, with Magento's "index as default" behavior
    http://your-site.example.com/pulsestorm_pestle/

**Further Reading**

- [Introduction to Magento 2 -- No More MVC](https://alanstorm.com/magento_2_mvvm_mvc/)

- [Magento 2: Advanced Routing](https://alanstorm.com/magento_2_advanced_routing/)

- [Magento 2: Admin MVC/MVVM Endpoints](https://alanstorm.com/magento_2_admin_mvcmvvm_endpoints/)

## generate:view

    Usage:
        $ pestle.phar magento2:generate:view

    Arguments:

    Options:

    Help:
        Generates view files (layout handle, phtml, Block, etc.)

        @command magento2:generate:view
        @argument module_name Which Module? [Pulsestorm_HelloGenerate]
        @argument area Which Area? [frontend]
        @argument handle Which Handle? [<$module_name$>_index_index]
        @argument block_name Block Name? [Main]
        @argument template Template File? [content.phtml]
        @argument layout Layout (ignored for adminhtml) ? [1column]

The `magento2:generate:view` command allows you to generate a "Magento View".  View here is a loose concept -- what this command actually does is allow you to generate-or-edit a Magento layout handle XML file, auto generate layout handle XML code that will add a block to the `content` container block, add a new class and `phtml` template for the block.

**Interactive Invocation**

    $ pestle.phar magento2:generate:view
    Which Module? (Pulsestorm_HelloGenerate)] Pulsestorm_Pestle
    Which Area? (frontend)] frontend
    Which Handle? (pulsestorm_pestle_index_index)] pulsestorm_pestle_index_index
    Block Name? (Main)] Main
    Template File? (content.phtml)] content.phtml
    Layout (ignored for adminhtml) ? (1column)] 1column
    Creating  /path/to/m2/app/code/Pulsestorm/Pestle/view/frontend/templates/content.phtml
    Creating: Pulsestorm\Pestle\Block\Main
    Creating: /path/to/m2/app/code/Pulsestorm/Pestle/view/frontend/layout/pulsestorm_pestle_index_index.xml

**Argument Invocation**

    $ pestle.phar magento2:generate:view Pulsestorm_Pestle frontend pulsestorm_pestle_index_index Main content.phtml 1column

This command will ask you for

1. The module you want to create your view inside of (`Pulsestorm_Pestle`)
2. The area this view applies to (`frontend`, `adminhtml`)
3. The full action name layout handle this view applies to. (`pulsestorm_pestle_index_index`)
4. The short class name for your block (`Main`)
5. The template file for your block (`content.phtml`)
6. The magento layout key for your handle file (`1column`)

Magento's blocks and layout system is complicated.  If you're not sure what any of the above means, be sure to checkout the further reading section below.

**Further Reading**

- [No Frills Magento 2 Layout](https://store.pulsestorm.net/products/no-frills-magento-2-layout)

- [Introduction to Magento 2 — No More MVC](https://alanstorm.com/magento_2_mvvm_mvc/)

- [Magneto 1 Layouts, Blocks and Templates](https://alanstorm.com/layouts_blocks_and_templates/)

## generate:theme

    Usage:
        $ pestle.phar magento2:generate:theme

    Arguments:

    Options:

    Help:
        Generates Theme Configuration

        @command magento2:generate:theme
        @argument package Theme Package Name? [Pulsestorm]
        @argument theme Theme Name? [blank]
        @argument area Area? (frontend, adminhtml) [frontend]
        @argument parent Parent theme (enter 'null' for none) [Magento/blank]

A Magento 2 theme is a collection of layout handles, templates, and frontend files that will change the look, feel, and (sometimes) behavior of a Magento system.  Themes are distributed as packages separate from modules, and the `magento2:generate:theme` command will create the basic skeleton of a theme.

**Interactive Invocation**

    $ pestle.phar magento2:generate:theme
    Theme Package Name? (Pulsestorm)] Pulsestorm
    Theme Name? (blank)] blank
    Area? (frontend, adminhtml) (frontend)] frontend
    Parent theme (enter 'null' for none) (Magento/blank)]
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/theme.xml
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/registration.php
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/etc/view.xml
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/web/css/source
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/fonts
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/images
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/js
    /path/to/m2/app/design/frontend/Pulsestorm/blank

**Argument Invocation**

    $ pestle.phar magento2:generate:theme Pulsestorm blank frontend ''

    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/theme.xml
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/registration.php
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/etc/view.xml
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/web/css/source
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/fonts
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/images
    Creating: /path/to/m2/app/design/frontend/Pulsestorm/blank/js
    /path/to/m2/app/design/frontend/Pulsestorm/blank

**Further Reading**

- [What is a Magento Theme, the 10,000 Foot View](https://alanstorm.com/what-is-a-magento-theme-the-10000-foot-view/)

## generate:crud-model

    Usage:
        $ pestle.phar magento2:generate:crud-model

    Arguments:

    Options:

    Help:
        Generates a Magento 2 CRUD/AbstractModel class and support files

        @command magento2:generate:crud-model
        @argument module_name Which module? [Pulsestorm_HelloGenerate]
        @argument model_name  What model name? [Thing]
        @option use-upgrade-schema Create UpgradeSchema and UpgradeData
        classes instead of InstallSchema
        @option use-upgrade-schema-with-scripts Same as use-upgrade-schema,
        but uses schema script helpers
        @option use-install-schema-for-new-model Allows you to add another
        model definition to InstallSchema

https://alanstorm.com/magento_2_crud_models_for_database_access/

TODO: WRITE THE DOCS!

## generate:schema-upgrade

    Usage:
        $ pestle.phar magento2:generate:schema-upgrade

    Arguments:

    Options:

    Help:
        BETA: Generates a migration-based UpgradeSchema and UpgradeData
        classes

        @command magento2:generate:schema-upgrade
        @argument module_name Module Name? [Pulsestorm_Helloworld]
        @argument upgrade_version New Module Version? [0.0.2]
        @option use-simple-upgrade Option to skip creating script helpers

https://alanstorm.com/magento-2-setup-migration-scripts/

TODO: WRITE THE DOCS!
