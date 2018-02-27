---
layout: dev
---

## Scripts

To write scripts for your widgets or windows, you can use FML scripting framework and facilities.

```php
<?php

function createManialink($manialink) {
    
    // to add custom functions and labels you can use this snippet:
    $manialink->getFmlManialink()->getScript()->addScriptFunction("", 
    <<<EOL
        // your maniascript goes here
        Void sayHello() {
            log("hello");        
        }
        
        Text sayHello(Text text) {
            return "hello " ^ text;
        }                         
EOL
    );     
    
    // normally what you'll write at begin of main(), you do onInit    
    $manialink->getFmlManialink()->getScript()->addCustomScriptLabel(ScriptLabel::OnInit, 
    <<<EOL
        // your maniascript goes here
        declare Text toto = "toto";
EOL
     );
     
     // and parts that goes to while loop goes here:     
     $manialink->getFmlManialink()->getScript()->addCustomScriptLabel(ScriptLabel::Loop,
     <<<EOL
     // your maniascript goes here
     
               
EOL
    );       
     
     // and there's some helpers also
     $manialink->getFmlManialink()->getScript()->addCustomScriptLabel(ScriptLabel::MouseClick,
     <<<EOL
     // Event is predefined in the label.
     
     if (Event.ControlId == "toto")  {
         sayHello();
     }   
EOL
    );       
      
 
     
    
}
```

