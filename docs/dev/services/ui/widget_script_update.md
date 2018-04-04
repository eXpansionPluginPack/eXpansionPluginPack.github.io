---
layout: dev
---

## Widget - Script Update

**eXpansion 2** comes with a new approach for widget updates. We can either update the widget manialink for each player 
individually but this is a slow process when there is 200 players on the server. Even with eXpansions optimized GuiHandler
that will group calls. 

The second option you have is to use 2 manialinks. Both manialinks are sent to all players. One of the manialinks contains
the `data` that needs to be displayed in maniascript. The other manialink contains the display, and a script that updates the data.
The server controller will then update only the `data` manialink. The player specific process is executed on client side. 

This way we can individual widgets for players even on very busy servers without compromising performance. eXpansion 
comes with all the tools to make this as easy as possible. 

### Pushing data to the front

To push data to the front you need 2 things : 
1. A `eXpansion\Bundle\WidgetBestCheckpoints\Plugins\Gui\ScriptVariableUpdateFactory` to update the variables
2. A plugin to update the data. 

So first let's imagine we wish to have list of best checkpoints times. We therefore need a variable of type `Integer[Integer]`. 

As nearly everything else in eXpansion this update factory is actually a service :

```yaml
    eXpansion.bundle.widget_best_checkpoints.plugins.Gui.updater_widget_factory:
            class: eXpansion\Bundle\WidgetBestCheckpoints\Plugins\Gui\ScriptVariableUpdateFactory
            autowire: true
            arguments:
                $name:  "BestCheckpoints Updater"
                $variables:
                  - {name: "LocalRecordCheckpoints", type: "Integer[Integer]", default: "Integer[Integer]"}
            tags:
                - {name: expansion.plugin, data_provider: exp.timer}
                - {name: expansion.plugin, data_provider: expansion.user_group}
```

This plugin service has user_groups & timer as data providers. This allows it to update automatically. 

As you can see one UpdaterScript can update multiple variables. 

Second step is to fetch the data, we will use a simple plugin for that. 

```php
<?php
   
class BestCheckpoints implements StatusAwarePluginInterface {
        
    protected $updater;
    
    protected $playerGroup;
    
    public function __construct(
            ScriptVariableUpdateFactory $updater,
            Group $playerGroup
        ) {

            $this->updater = $updater;
            $this->playerGroup = $playerGroup;
        }
        
        public function setStatus($status)
        {
            if ($status) {
                $this->updater->create();
            } else {
                $this->updater->destroy();
            }
        }
        /**
         * Called when cp times are updated
         */
        public function onDataUpdate($times)
        {
            if (count($times) > 0) {
                $this->updater->updateValue($this->playerGroup, "LocalRecordCheckpoints", \FML\Script\Builder::getArray($times, true));
            } else {
                $this->updater->updateValue($this->playerGroup, "LocalRecordCheckpoints", "Integer[Integer]");
            }
        }
}
```

And the definition of the service.

```yaml
services:
    eXpansion\Bundle\WidgetBestCheckpoints\Plugins\BestCheckpoints:
        class: eXpansion\Bundle\WidgetBestCheckpoints\Plugins\BestCheckpoints
        autowire: true
        arguments:
                $allPlayers: '@expansion.framework.core.user_groups.all_players'
                $updater: '@eXpansion.bundle.widget_best_checkpoints.plugins.Gui.updater_widget_factory'
        tags:
          - {name: 'expansion.plugin', data_provider: 'my_data_provider'}
```

With this in place the data will be sent properly. We now need to actually display it. 

### Displaying the data

To display the data we will need a widget. 

```php
<?php
class BestCheckpointsWidgetFactory extends WidgetFactory
{
    /** @var UpdaterWidgetFactory */
    protected $updaterWidgetFactory;
    
    public function __construct(
        $name,
        $sizeX,
        $sizeY,
        $posX,
        $posY,
        WidgetFactoryContext $context,
        UpdaterWidgetFactory $updaterWidgetFactory
    ) {
        parent::__construct($name, $sizeX, $sizeY, $posX, $posY, $context);
        $this->updaterWidgetFactory = $updaterWidgetFactory;
    }
    
    protected function createContent(ManialinkInterface $manialink)
    {
        // ... Creating all the fml manialinks.

        // Get the name of the variable. 
        $cpVariable = $this->updaterWidgetFactory->getVariable('LocalRecordCheckpoints')->getVariableName();

        // On initialization init properly the "shared" variables.
        $manialink->getFmlManialink()->getScript()->addCustomScriptLabel(
            ScriptLabel::OnInit,
            <<<EOL
            {$this->updaterWidgetFactory->getScriptInitialization()}
EOL
        );

        // Do a function that will update the content.
        $manialink->getFmlManialink()->getScript()->addScriptFunction(
            "",
            <<<EOL
            Void Refresh() {
                // Do stuff
                {$cpVariable}[_Index];
            }
EOL
        );
        
        // Handle in script loop the update.
        $manialink->getFmlManialink()->getScript()->addCustomScriptLabel(
            ScriptLabel::Loop,
            <<<EOL
            
            // handle data change
            {$this->updaterWidgetFactory->getScriptOnChange('Refresh();')}
EOL
        );
    }
}
```

> Note that we don't use `LocalRecordCheckpoints` directly in maniascript. We fetch the name of the variable from the
updater. The updater creates a unique name to prevent unwanted interactions between widgets that might have same name.

And declare the widget 

```yaml
services:
    eXpansion\Bundle\WidgetBestCheckpoints\Plugins\Gui\BestCheckpointsWidgetFactory:
            class: eXpansion\Bundle\WidgetBestCheckpoints\Plugins\Gui\BestCheckpointsWidgetFactory
            autowire: true
            arguments:
                $name:  "BestCheckpoints"
                $posX: -95
                $posY: 89
                $sizeX: null
                $sizeY: null
                $updaterWidgetFactory: '@eXpansion.bundle.widget_best_checkpoints.plugins.Gui.updater_widget_factory'
```