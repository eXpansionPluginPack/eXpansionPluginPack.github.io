---
layout: dev
---

## Widget - Dynamic Positioning

A Widget is a bit different then a window, windows should appear in constant positions, 
usually in the center of the screen. But widget positions are very important and might need to change depending on the
game, title, mode. 

When configuring a widget factory service with the fallowing definition : 
```yaml
    MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory:
        class: MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory
        autowire: true #You can set this as default if need be
        arguments:
            $name:  'Title of the Widget' # This can be a translation string as well 
            $sizeX: 180 # Width of the ML
            $sizeY: 90  # Height of the ML
            $posX:  0
            $posY:  0
```

We have set the defaults positions to be 0. But if we would like to have our widget to have a custom positions depending
on the game, title, mode you will need to do more. 

First we need to make your widget a plugin, this can be done by adding the proper tag to the service definition.

```yaml
    MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory:
        class: MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory
        autowire: true #You can set this as default if need be
        arguments:
            $name:  'Title of the Widget' # This can be a translation string as well 
            $sizeX: 180 # Width of the ML
            $sizeY: 90  # Height of the ML
            $posX:  0
            $posY:  0
        tags:
            - {name: expansion.plugin, data_provider: 'exp.widget'}
```

The widget is now ready to receive dynamic positioning. Now we can configure these positions. 
Let's create a new directory `expansion_defaults` in the bundles `Resources/config` directory. Here let's put a yml file
name `core_config.yml`

The content of this yaml needs to be : 

```yaml
e_xpansion_core:
    #
    # You can configure custom positions for widgets here.
    #
    widget_positions:
      MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory: # Id of the widget service
        TM: # Game/Title it's compatible with (ALL to use it on all gamemodes)
          ALL: # The game mode to apply he config for.
            ALL: # The script mode to apply the config for.
              posX: 80
              posY: -85
              options: # Additional values, check the widgets documentation.
                test: "test value"
        TM: 
          Script:
            "TimeAttack.Script.txt":
              posX: 12
              posY: -25
              options: # Additional values, check the widgets documentation.
                test: "test value"
```

In this exemple we have configured in total 3 positions. 
* In SM default configurations of the service will be used (0,0)
* In TM, with TA script (12,-25) will be used as position
* And in any other TM game (80,-85). 

These configurations can later be overriden by the server administator. [See the admin docs](/docs/admin/config/widget_positions.html)

Let's note that eXpansion will handle the changes. You don't need to "move" the widget. The move & update of the widget
is triggered automatically. 