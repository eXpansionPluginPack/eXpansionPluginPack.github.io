---
layout: dev
---

# Installing eXpansion for development. 

There is 2 development cases, either you wish to create a Pull Request to eXpansion or you wish to develop a Bundle for
eXpansion. 

## 1. Creating Custom Bundles

If you wish to create custom bundles, you will need to:
 
1. install eXpansion normally: [Install on Linux](/docs/admin/install.md)
2. Create a `src` folder in the root, this is where you will put your custom code. 
3. Update composer to add src to autoload: 

```json
{
    "autoload": {
        "psr-0": {
            "": "src/"
        }
    }
}
```

## 2. Coding for eXpansion 

If you wish to create a Pull Request in order to help us you will need to: 

1. Fork eXpansion repository: https://github.com/eXpansionPluginPack/eXpansion2
2. Clone the fork using git
3. Install all necessary dependencies for expansion: [Install on Linux](/docs/admin/install.md)
4. Do your changes
5. Write Unit Tests if necessary
6. Create a PullRequest

## Notes 

You can also install php extension to ease debugging: 
* xdebug

to start debugging use following command:
```bash
$ php -dxdebug.remote_enable=1 -dxdebug.remote_mode=req -dxdebug.remote_port=9000 -dxdebug.remote_host=127.0.0.1 bin/console eXpansion:run -vvv --env=dev
```
