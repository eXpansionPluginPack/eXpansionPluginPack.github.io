---
layout: dev
---

## UI Elements 

* **Autowire: TRUE** This service can be autowired into your services. 
* **Class:** eXpansion\Framework\Gui\Ui\Factory

eXpansion<sup>2</sup> has some custom FML elements written to help you build your ui.
These are accessible through a factory. The idea is to normalize the display in all the manialinks of the controller.

All Widget & Windows factories has the UI Factory and Action Factory pre-defined in them. The factory can be used with:

```php
<?php
$this->uiFactory->createSomething($argument1, $argument2, ...);
```

The idea of using a factory service is that this allows any bundle developer to override our elements to completely 
redesign eXpansion.

### Label

usage differs only a little from FML native label:

```php
<?php
namespace mybundle\example\plugins\example;
use eXpansion\Framework\Gui\Components\Label;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {   
        $label = $this->uiFactory->createLabel('text', Label::TYPE_NORMAL);
        $label->setPosition(0,0);
        $manialink->addChild($label);
    }   
}
?>
```

type can be: `Label::TYPE_NORMAL`, `Label::TYPE_TITLE`, `Label::TYPE_HEADER`

### Button

Create easily clickable buttons.

usage:
```php
<?php
namespace mybundle\example\plugins\example;
use eXpansion\Framework\Gui\Components\Button;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {    
        $button = $this->uiFactory->createButton('apply', Button::TYPE_DECORATED);
        $button->setAction(
            $this->actionFactory->createManialinkAction($manialink, [$this, "callbackApply"],[])
            );
        $manialink->addChild($button);
    }
}
?>
```
Parameters:

1. button label
2. type
3. manialink id

types:
`Button::TYPE_DEFAULT` default button, `Button::TYPE_DECORATED` button with frame.

methods:
`setText('string)`, `setTextColor('rrggbbbaa')`,`setBackgroundColor('rrggbbbaa')`, `setFocusColor()`, `setBorderColor()`, `setAction()`

### Checkbox

Creates a checkbox.
 
To receive values, you need to add button with callback.


example:
```php
<?php
namespace mybundle\example\plugins\example;

use eXpansion\Framework\Gui\Components\Button; 

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {         
        $checkbox = $this->uiFactory->createCheckbox('enable option', 'checkboxName');
        $checkbox->setPosition(0,0);
        $manialink->addChild($checkbox);
        
        $sendbutton = $this->uiFactory->createButton("Send", Button::TYPE_DECORATED);
        $sendbutton->setPosition(36,0);
        $sendbutton->setAction($this->actionFactory->createManialinkAction($manialink, [$this, 'myButtonCallback'], []));
        $manialink->addChild($sendbutton);
    }

    function myButtonCallback($manialink, $login, $entries, $args) 
    {
        if (isset($entries['checkboxName']) && $entries['checkboxName'] == "1") {
            // checkbox is active
        } 
        else {
            // checkbox is passive    
        }
    }
}
?>
```

> Checkbox returns $parameters: key as name and value of string "0" or "1"


### Input

Create input fields. You can have password field masked by ****** changing the type to `uiInput::TYPE_PASSWORD`   

usage 
```php
<?php
$input = $this->uiFactory->createInput("name", "", 60, Input::TYPE_BASIC);
$manialink->addChild($input);

```

1. name for entry to return 
2. default value
3. width
4. type

Types:
`uiInput:TYPE_BASIC`, `uiInput::TYPE_PASSWORD`


### InputMasked

Create password field with a button to toggle masked/normal contents.

usage 
```php
<?php
$input = $this->uiFactory->createInputMasked("name", "", 60);
$manialink->addChild($input);

```

1. name for entry to return 
2. default value
3. width

### TextBox

Create multiline input field  

usage 
```php
<?php
$textbox = $this->uiFactory->createTextbox("name", "", 3,60);
$manialink->addChild($textbox);

```

1. name for entry to return 
2. default value
3. lines
4. width


### Dropdown

usage:
```php
<?php
namespace mybundle\example\plugins\example;

use eXpansion\Framework\Gui\Components\Button; 


class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {
        parent::createContent($manialink);
        
        $dropdown = $this->uiFactory->createDropdown('myDropdown', ["tech" => "techmap", "lol" => "roflmap"], -1);   
        $manialink->addChild($dropdown);
        
        $sendbutton = $this->uiFactory->createButton("Send", Button::TYPE_DECORATED);
        $sendbutton->setPosition(36,0);
        $sendbutton->setAction($this->actionFactory->createManialinkAction($manialink, [$this, 'callbackMyButton'], []));
        $manialink->addChild($sendbutton);
    }
    
    public function callbackMyButton($manialink, $login, $entries, $args) 
    {
         if (isset($entries['myDropdown'])) {
            $selection = $entries['myDropdown'];   // will return "", "techmap" or "roflmap"
            echo $login . " selected: ". $selection . "\n";
         }
    }
}

?>
```

### Line

Draws a line based on starting point and (angle + length) or ending point

```php
<?php
namespace mybundle\example\plugins\example;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {          
        $line1 = $this->uiFactory->createLine(0,0);
        $line1->to(0,10);
        $manialink->addChild($line1);
        
        
        $line2 = $this->uiFactory->createLine(0,0);
        $line2->setAngle(45.0)->setLength(5.3);        
        $manialink->addChild($line2);                          
    }   
}
?>
```

## UI Helpers

### Animation

You can easily add animations to FML elements that implements class of **\FML\Controls\Control**, only label of uiElements is currently supported.
Frames, Quads and Labels are good to animate.

```php
<?php
    // create animation helper
    $animation= $this->uiFactory->createAnimation();
    $manialink->addChild($animation);
    
    $label = $this->uiFactory->createLabel("test");
    $label->setOpacity(0);
    
    $animation->addAnimation($label, "opacity='1'", 1000, 0, "Linear");
    $manialink->addChild($label);
    
```

addAnimation(control, animations, length, delay, easing)
1. Control for animation to be added
2. animation properties, you can automate xml properties of the control:
   normally opacity, scale and pos,  
3. animation length in milliseconds
4. animation delay in milliseconds
5. easing function, string or const

Valid easings are defined as const for uiAnimation, you can use also string.
You can preview easing functions at [www.easings.net](http://easings.net).

| Easing      | Supported types |
| ----------- | ----------- |
| Linear      |`Linear`  |
| Quad        |`QuadIn`,`QuadOut`,`QuadInOut`|
| Cubic       |`CubicIn`,`CubicOut`,`CubicInOut`|
| Quart       |`QuartIn`,`QuartOut`,`QuartInOut`|
| Quint       |`QuintIn`,`QuintOut`, `QuintInOut`|
| Sine        |`SineIn`,`SineOut`,`SineInOut`|
| Exp         |`ExpIn`,`ExpOut`,`ExpInOut`|
| Circ        |`CircIn`,`CircOut`,`CircInOut`|
| Back        |`BackIn`,`BackOut`,`BackInOut`|
| Elastic     |`ElasticIn`,`ElasticOut`,`ElasticInOut`<br>`Elastic2In`,`Elastic2Out`,`Elastic2InOut`|
| Bounce      |`BounceIn`,`BounceOut`,`BounceInOut`|

### Tooltip

You can easily add tooltips to any FML or uiElements.

```php
<?php
    // create tooltip helper
    $tooltip = $this->uiFactory->createTooltip();
    $manialink->addChild($tooltip);
   
    $btn = $this->uiFactory->createButton("Apply", Button::TYPE_DECORATED);
    $tooltip->addTooltip($btn, "Tooltip example 1");   
    
    $btn = $this->uiFactory->createButton("Cancel", Button::TYPE_DECORATED);
    $tooltip->addTooltip($btn, "Tooltip example 2");
```

parameters:

1. uiElement or FML control
2. tooltip text


## Layout builders

Layout builders are helper classes to position elements more easily! Layouts can take any Renderable 
uiComponent, which has predefined size. You can also add lines to row or vice versa. 

### LayoutRow

```php
<?php
namespace mybundle\example\plugins\example;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {
       // best practise to add multiple elements
        $row = $this->uiFactory->createLayoutRow(0, 0, [], 2);
        $row->addChildren([
            $this->uiFactory->createCheckbox("test checkbox 1", "checkbox1"),
            $this->uiFactory->createButton("button 1"),  
        ]);
        $manialink->addChild($row);
            
            
        // add using constructor array
        $checkbox = $this->uiFactory->createCheckbox("test checkbox 1", "checkbox1");
        $checkbox2 = $this->uiFactory->createCheckbox("test checkbox 2", "checkbox2");
        $manialink->addChild($this->uiFactory->createLayoutRow(0, 0, [$checkbox, $checkbox2], 0));
        
        
        // or add one by one
        $rowHelper = $this->uiFactory->createLayoutRow(0, -$row->getHeight(), [], 1); // initialize with empty array
        for ($x = 0; $x < 10; $x++) {
            $btn = $this->uiFactory->createCheckbox('box'.$x, 'cb_'.$x);
            $rowHelper->addChild($btn); 
        }
        $manialink->addChild($rowHelper);
                              
    }
}
?>
```

### LayoutLine

Creates layout of elements next to each other. 

```php
<?php
namespace mybundle\example\plugins\example;

use eXpansion\Framework\Gui\Components\Button;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {
        // best practise to add multiple elements 
        $row = $this->uiFactory->createLayoutRow(0, 0, [], 0);
        $row->addChildren([
            $this->uiFactory->createButton("Apply"),
            $this->uiFactory->createButton("Cancel", Button::TYPE_DECORATED)
        ]);        
        $manialink->addChild($row);
        
        // or add one by one
        $lineHelper = $this->uiFactory->createLayoutLine(0, -9, [], 1); // initialize with empty array
        for ($x = 0; $x < 10; $x++) {
            $btn = $this->uiFactory->createButton('btn'.$x, 'btn_'.$x);
            $lineHelper->addChild($btn); // then add 
        }
        $manialink->addChild($lineHelper);
    }
}
?>
```

### LayoutScrollable

Creates a scrollable area.

```php
<?php
    
    $content = $this->uiFactory->createLayoutRow(55, 0, [], 1);    
    for ($x = 0; $x < 10; $x++) {
        $btn = $this->uiFactory->createCheckbox('box'.$x, 'cb_'.$x);
        $content->addChild($btn);
    }
    
    $scrollable = $this->uiFactory->createLayoutScrollable($content, 40, 30);
    $scrollable->setAxis(true, true); // set x and y axis available.
    
    $manialink->addChild($scrollable);
        
```

parameters:

1. Frame or uiLayout
2. width
3. heigth

## Complex example

```php
<?php
namespace mybundle\example\plugins\example;

use eXpansion\Framework\Gui\Components\Button;

class myWindowFactory {
    
    protected function createContent(ManialinkInterface $manialink)
    {       
       $checkbox = $this->uiFactory->createCheckbox("test checkbox 1", "checkbox1");
       $checkbox2 = $this->uiFactory->createCheckbox("test checkbox 2", "checkbox2");
       $line1 = $this->uiFactory->createLayoutRow(0, 0, [$checkbox, $checkbox2], 0);   // sum the two ui components to a line

       $ok = $this->uiFactory->createButton("Apply", Button::TYPE_DECORATED);
       $ok->setAction($this->actionFactory->createManialinkAction($manialink, [$this, 'callbackButton'], ["type" => "ok"]));

       $cancel = $this->uiFactory->createButton("Cancel");
       $cancel->setAction($this->actionFactory->createManialinkAction($manialink, [$this, 'callbackButton'], ["type" => "cancel"]));
       $line2 = $this->uiFactory->createLayoutLine(0, 0, [$ok, $cancel], 1); // sum the two ui components to second line 
       
       $row = $this->uiFactory->createLayoutRow(0, -10, [$line1, $line2], 0);  // make two rows of the lines
       
       $manialink->addChild($row);     
        
    }
}
?>
```

