---
layout: dev
---

## Manialink actions

Allows adding functionality for buttons and other ui elements.
 
* **Autowire : TRUE** This service can be autowired into your services. 
* **Class :** eXpansion\Framework\Core\Plugins\Gui\ActionFactory

By default this service is already available in Window & Widget Factories. So you won't need to inject it. 

### Signature

The signature of the method allowing the creation of a action is as fallow : 
```php
<?php 
function createManialinkAction(ManialinkInterface $manialink, $callable, $args, $permanent = false);
```

* **$manialink :** The manialink for which the action is being generated for.
* **$callable :** The function to class in a certain object. _See php callable doc_
* **$args :** Arguments you wish to get back in the callback function. 
* **$permanent :** By default actions on a manialinks are deleted when the manialink is updated. When an action is parmenent it won't be deleted as long as the manialink is used. Basically if you create an action in the `create` method of the manialink factory you should use `permanent : true`, if not `false`. This way on each update the actions unused will be deleted.

### Exemple 

Let's see how to use it for a button, you can use the same technique with any element that supports the `setAction` method. 

```php
<?php
/** @var string $myAction */
$myAction = $this->actionFactory->createManialinkAction($manialink, [$this, 'callbackSayHello'], null);

$helloButton = $this->uiFactory->createButton("myBundle.gui.button.hello", uiButton::TYPE_DECORATED);
        $helloButton->setTranslate(true)->setAction($myAction);
        $manialink->addChild($helloButton);
```
 
You usually don't need to unset actions, as they are cleared automatically on window close.

If need to delete an action you can do so with the fallowing method :
```php
<?php
$this->actionFactory->destroyAction($myAction);
```

The callback function is always structured like this:
* **$login :** The login of the player who clicked the action,
* **$entries : ** An array of input elements, usually we use these to send data back from checkboxes, dropdown menus and 
textboxes and text inputs.
* **$args : ** The args you can set freely at createManialinkAction.

**Exemple :**
```php
<?php
    public function callbackSayHello(ManialinkInterface $manialink, $login, $entries, $args) {
    
    }
```

here the $manialink is provided for closing the window, or updating the contents with ease:
```php
<?php
 $this->update($manialink->getUserGroup());
 // or to close
 $this->closeManialink($manialink);
```
