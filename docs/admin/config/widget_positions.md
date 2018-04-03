---
layout: admin
---

## Configuration - Widget Positions

in eXpansion it is possible to configure widget's positions. 
You can configure different positions for each game mode. 

The configurations must be put in the `app/config/expansion.yml` file. 
```yaml
e_xpansion_core:
    #
    # You can configure custom positions for widgets here.
    #
    widget_positions:
      MyVendor\Bundle\MyBundle\Plugins\Gui\MyWidgetFactory: # Id of the widget service
        TM: # Game/Title it's compatible with (ALL to use it on all gamemodes)
          ALL: # The game mode to apply he config for. (Example : Script)
            ALL: # The script mode to apply the config for. (Example "TimeAttack.Script.txt")
              posX: 80
              posY: -85
              options: # Additional values, check the widgets documentation.
                test: "test value"
```

> If you wish to change the position of a widget and can't find it here please contact us. 
We either forget to add it to the the documentation or forgot to properly declare the widget.

### Best Checkpoints

* **Id :** eXpansion\Bundle\WidgetBestCheckpoints\Plugins\Gui\BestCheckpointsWidgetFactory
* **Custom Options :** _none_

### Best Records

* **Id :** eXpansion\Bundle\WidgetBestRecords\Plugins\Gui\BestRecordsWidgetFactory
* **Custom Options :** _none_

### Current Map

* **Id :** eXpansion\Bundle\WidgetCurrentMap\Plugins\Gui\CurrentMapWidgetFactory
* **Custom Options :** _none_

### Custom UI : 

#### Custom Speed Widget 

There are 2 versions of the widget : 

* **Id :** eXpansion\Bundle\CustomUi\Plugins\Gui\CustomSpeedWidget
* **Custom Options :** _none_

And one for SM : 

* **Id :** eXpansion\Bundle\CustomUi\Plugins\Gui\CustomSMSpeedWidget
* **Custom Options :** _none_

#### Custom Checkpoint Widget

* **Id :** eXpansion\Bundle\CustomUi\Plugins\Gui\CustomCheckpointWidget
* **Custom Options :** _none_