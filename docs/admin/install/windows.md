---
layout: admin
---

# Install eXpansion on Windows

## Install wamp 

We will start by installing wamp, this will basically install
* PHP
* Mysql

Which we need. 

1. Download 32-bit wamp from: [http://www.wampserver.com/](http://www.wampserver.com/)
2. Execute the installer 

### Know issues 

#### VCRUNTIME140.dll not found

1. Install following on your computer : [https://www.microsoft.com/en-us/download/details.aspx?id=30679](https://www.microsoft.com/en-us/download/details.aspx?id=30679)
1. Uninstall wamp
1. Restart computer
1. Reinstall wamp.

#### Class 'COM' not found

```yaml
[
  "error" => Error {
    #message: "Class 'COM' not found",
    #code: 0,
    #file: "C:\dedicated\exp2\vendor\oliverde8\asynchronous-jobs\src\AsynchronousJobs\JobRunner.php",
    #line: 78,
    ...
    ...   
```
In case you installed 64bit version of wamp, you'll get following error, this can be fixed.
Uninstall 64-bit version and installi 32-bit version shoud fix, othervice check that the following extension is installed at your php.ini:

> php.ini 

```ini
extension=php_com_dotnet.dll
```

## Install composer

1. Download installer from : [https://github.com/johnstevenson/composer-setup/releases/latest](https://github.com/johnstevenson/composer-setup/releases/latest)
1. Execute the installer
1. At one point you will have `chose the command line php you want to use`.
    * Chose php 7.1

## Configure eXpansion 

{% include docs/install/config_base.md %}

For more information check : [Configure eXpansion](../config/configuration.html)

## Startup eXpansion

To start expansion double click on the `run.bat` file in the `bin` directory.

This will install/update eXpansion and all it's dependencies.

## Update eXpansion

> Alpha1 has an issue and the update.bat is missing in bin, but you fix this by copying it from exp directory.

Double click on the `update.bat` file in the `bin` directory. 

