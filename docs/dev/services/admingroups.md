---
layout: dev
---

## Admin Groups

The Admin Groups allows having various admin groups with players in them, each group having different permissions. 

The Admin groups functionality of expansion will place each player in a certain group, 
player that has no privileges will be put in the guest group. 

> Difference to eXpansion<sup>1</sup> -  A player can only be in one Admin group.

### Admin Groups Helper

* **Autowire: TRUE** This service can be autowired into your services. 
* **Class:** eXpansion\Framework\AdminGroups\Helpers\AdminGroups

Allows to check if a player has certain permissions, to which admin group a player belongs, 
and get player groups in order to display widgets for each admin group.


| Method                     | Description |
| -------------------------- | ----------- |
| **getUserGroups**          | Get list of all admin user groups, with the guest user group as well.  |
||
| **getLoginUserGroups**     | Get the user group a certain player is.  |
|                            | **String $login** The login of the user to get the user group of.|
||
| **hasPermission**          | Check if a certain player has a certain permission  |
|                            | **String $login** The login of the user to check the permission for.|
|                            | **String $permission** The permission to check for.|ser group of.|
||
| **hasGroupPermission**     | Check if a certain group has a certain permission  |
|                            | **String $groupName** The name of the group to check permissions for.|
|                            | **String $permission** The permission to check for.|
||

This helper can be used in your windows factories or any other place you need to check permissions for. 
You should not need to use this for chat commands as they will handle their permissions automatically.

### Admin Chat Commands. 

This component also allows the creation of admin chat commands. In order to do this we will simply use 
`eXpansion\Framework\AdminGroups\Model\AbstractAdminChatCommand` instead of `eXpansion\Framework\Core\Model\ChatCommand\AbstractChatCommand`

When creating chat command using this class we our commands will automatically be prefixed with **admin**.
So if we register a **restart** command it will actually work with **/admin restart** and not just **/restart**.

It will also register aliases so that **/adm restart** works as well. Like normal chat commands you can also register 
aliases. Lihe the command line if you register **res** as alias you are going to have **/admin res** & **/adm res** work.
. 
Finally when registering a AdminChatCommand you must register the permission needed for a user to use the command. 

#### Exemple 
```yaml
    expansion.admin_chat.chat_command.restart:
        class: eXpansion\Bundle\AdminChat\ChatCommand\Restart
        parent: expansion.admin_chat.chat_command.base
        arguments:
            - "restart" # The chat commend to have /admin restart work
            - "restart" # The permission needed by the player to use the chat command. 
            -  ['res', 'restartmap'] # The aliases registered with this chat command.
            - '@eXpansion\Framework\AdminGroups\Helpers\AdminGroups' # The admin chat commands requires the admin groups helper, this can be autowired !!
```

### Adding a new permission

Feature added **@BETA-3**

If you are adding in your bundle a new admin command it's quite probable tou will want to create your own permission. 
in eXpansion adding a new permission that will be configurable ingame by the admin is like everthing else, you just 
need to create a new service. 

#### Exemple
```yaml
    .expansion.admin_groups.permission.admin:
        class: eXpansion\Framework\AdminGroups\Model\AdminPermission
        arguments:
            $description: "Access to generic admin actions"
        tags:
            - {name: exp.permission, permission: "admin"}

```

We set here a description that will be displayed in the admin groups configuration window. The permission tag attribute 
is the name of the permission.

> The service name starts with a `.` its a hidden service and can't be accessed. 
