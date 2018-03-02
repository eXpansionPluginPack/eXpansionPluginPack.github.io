---
layout: dev
---

## Adding elements to the menu

eXpansion 2 have it's own menu, and as all respectable controller any new bundle can add new items and functionalities
to the menu. 

In order to do this you will simply need a new plugin. 

```php
<?php
class MenuItems implements ListenerMenuItemProviderInterface
{
    /**
     * Register items tot he parent item.
     *
     * @param ParentItem $root
     *
     * @return mixed
     */
    public function registerMenuItems(ParentItem $root)
    {
        
    }
}
```

Our logic to create the menu will go in the `registerMenuItems`. There are at the moment 2 supported item types.
Parent items and ChatCommand items :

```php
<?php
$root->addChild(
    ParentItem::class,
    'records',
    'expansion_local_records.menu.label',
    null // Permission are handled by sub elements.
);
$root->addChild(
    ChatCommandItem::class,
    'records/list',
    'expansion_local_records.menu.race_recs',
    null,
    ['cmd' => '/recs']
);
```

So the first argument is the type, which is simply a class that implements MenuItemInterface. 

We then say were we wish to display it, so that's the path. 

We also need to give it a name, this text will be trasnlated. 

There is the permissions to give as well. Usually parent elements don't require one. If none of the child elements of a
parent is visible to the user then the user don't see the element at all.

Finally some parameters, these will depend on the MenuItem you are using. 

You will also need to declare this class as a service. 

```yaml
    Vendor\Bundle\MyBundle\Plugins\MenuItems:
        class: Vendor\Bundle\MyBundle\Plugins\MenuItems
        tags:
            - {name: 'expansion.plugin', data_provider: 'exp.menu.items'}            
            - {name: 'expansion.plugin.parent', parent: 'vendor.my_bundle.plugins.main_plugin'}
```

The parent tag will allow the Menu item to be displayed only if the that plugin is active. If not the menu items will be hidden.
