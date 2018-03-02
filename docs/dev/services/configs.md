---
layout: dev
---

## In-Game Configuration

One of the big features of eXpansion has always been the ability to configure nearly everything directly ingame, instead
of going through complex. 

In eXpansion2 we have kept this awsome feature, but we have taken it a step further to make it as easy as possible to
implement and use. 

### Basics 

#### Creating a new configuration. 

A cnfiguration, a parameter, like everything else in eXpansion2 is a service. So you just need to add your service : 

```yaml
services:
    _defaults:
        autowire: true
        autoconfigure: true
        
    eXpansion.local_records.config.nb_records:
        class: eXpansion\Framework\Config\Model\IntegerConfig
        arguments:
            $path: "eXpansion/LocalRecords/NbRecords"
            $name: "expansion_local_records.config.nb_records.name"
            $scope: "global"
            $description: "expansion_local_records.config.nb_records.dsc"
            $defaultValue: 100
            $minValue: 10
            $maxValue: 10000
        tags:
            - {name: expansion.config}
```

The **path** is used to display the configuration in the proper place in the menu. In this casse it will be in :
_Admin > Config > eXpansion > Local Records_. Cicking on this element will open a window containing all the configs
in this path. 
The **path** is normally only used to organize the configs in the menu. The real unique identifier of the config that 
will be used to fetch it is the service id. 

The **name** parameter is a translated string that will be displayed in the window

**scope** works much like eXpansion1. There are multiple scopes : 
* **global :** All servers sharing the same install will have same configuration
* **key :** Allows servers sharing the same key to have same configuration values. (Unused at the moment same as global) 
* **server :** Server configs are configs that are unique to the server. Exemple : Dedimania key and such. 

The **description** Will be displayed in the config window and allow people to better understand what they are configuring. 

The **defaultValue** allows to have a default value when nothing is configured.

The other parameters will depend upon the config Model you use. You can find all available config models in 
the `eXpansion\Framework\Config\Model` namespece. We will se later how you can create you own model.

#### Making the translations: 

First let's translate the menu elements. These translations keys are generated automatically by eXpansion. 

```yaml
expansion_config:
  menu:
    eXpansion:
      label: "eXpansion Configs"
      LocalRecords:
        label: "Local Records"
```

As you can see the path is taken back in the config.

Let's also translate our custom configs : 
```yaml
expansion_local_records:
  config:
    nb_records:
      name: "Number of Local Records"
```

#### Using the config

Yo use the config you will simply need to inject it in the service(s) that needs it. 

```yaml
eXpansion.local_records.services.lap_record_handler_factory:
    class: eXpansion\Bundle\LocalRecords\Services\RecordHandlerFactory
    arguments:
        $ordering: ASC
        $nbRecords: '@eXpansion.local_records.config.nb_records'
```

### Creating your own. 

In order to create your own config you need an class implementing `ConfigInterface`. Easiest way is to do this is to 
use `AbstractConfig`.  

```php
<?php 
class MyTotoConfig extends AbstractConfig
{
    /**
     * @inheritdoc
     */
    public function __toString() : string
    {
        return 'TODO'; /* Do stuff here */
    }
}
```

This can then be used as wished. By default nevertheless the ui will use a text field to display the config. 
To customize the ui you will need to add a custom UiField, implementing `UiInterface`

Let's first create our class : 

```php
<?php

class MyTotoField implements UiInterface
{
    /** @var Factory */
    protected $uiFactory;
    /**
     * TextField constructor.
     *
     * @param Factory $uiFactory
     */
    public function __construct(Factory $uiFactory)
    {
        $this->uiFactory = $uiFactory;
    }
    /**
     * @inheritdoc
     */
    public function build(ConfigInterface $config, $width, ManialinkInterface $manialink): Renderable
    {
        // TODO change this and do what you actually need
        return $this
            ->uiFactory
            ->createInput($config->getPath(), $config->getRawValue())
            ->setWidth($width);
    }
    /**
     * @inheritdoc
     */
    public function isCompatible(ConfigInterface $config): bool
    {
        return ($config instanceof MyTotoConfig);
    }
}
```

We can now need to create a service and register it as a config ui field.

```yaml
    Vendor\MyBundle\Ui\Fields\MyTotoField:
        class: Vendor\MyBundle\Ui\Fields\MyTotoField
        tags:
            - {name: 'expansion.config.ui'}
```

There you go, you have created you own config, with a dedicated server that displayes it properly.