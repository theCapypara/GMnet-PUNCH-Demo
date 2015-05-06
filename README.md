![GMnet PUNCH](http://parakoopa.de/GMnet/punch.png)
#GMnet PUNCH Demo Project
This repository contains ONLY the **demo project for GMnet PUNCH** as used on the Game Maker Marketplace. It does not contain any of the actual scripts of GMnet PUNCH and does not work without them.

**To get PUNCH or make changes to it, please use the repository of GMnet ENGINE.**

**More information about GMnet products can be found on the website:**  
http://gmnet.parakoopa.de

##Get GMnet PUNCH
To get GMnet PUNCH and this demo project, visit  
http://gmnet.parakoopa.de/punch

##Other repositories

* [GMnet ENGINE](https://github.com/Parakoopa/GMnet-ENGINE) (Repository for GMnet ENGINE/CORE/ACCESS and PUNCH)
* [GMnet ACCESS demo project](https://github.com/Parakoopa/GMnet-ACCESS-Demo)
* [GMnet GATE.ACCESS](https://github.com/Parakoopa/GMnet-GATE-ACCESS)
* [GMnet GATE.PUNCH](https://github.com/Parakoopa/GMnet-GATE-PUNCH)
* [GMnet GATE](https://github.com/Parakoopa/GMnet-GATE) (Installer and service that combines GATE.ACCESS and GATE.PUNCH)
* [GMnet GATE.TESTER](https://github.com/Parakoopa/GMnet-GATE-TESTER) (Web-based debugging tool for other GMnet GATE producs)
* [Manual pages](https://github.com/Parakoopa/GMnet-manual) (Get and edit the manual pages found on http://gmnet.parakoopa.de)

## Branches & Versioning

* The branch ``master`` contains all working changes to the demo projects. Some commits may be tagged with a tag like ``punch-VERSION``. These tags have the same version numbers as the oldest PUNCH version that is compatible with this demo project commit. Always use the latest version of the demo project that is older or has the same number as the PUNCH version you want to work with.
* All other branches are used for testing and development and may not work.

## How to commit
This repository contains a Game Maker Studio 1.x project. We want to keep the repo as clean as possible, because of that the following rules apply for commits & pull requests exist:
* Please change this project with the newest beta version of Game Maker or at least make sure the project doesn't create any junk data when using an older version or an early access version. Also make sure everything is working with the newest beta.
* Don't add any files that are not directly releated to any GMnet components or are a requirement of Game Maker to be able to open the project. That includes folders like the ``Config`` and the ``extensions`` folder.
* DON'T ADD COPYRIGHTED IMAGES OR MEDIA that is owned by YoYoGames, such as logos. If such a file is required in the future, replace it with other images, that we have the rights to use (e.g. Public Domain images).
* Do not commit changes to internal Game Maker files, such as the ``.project.gmx``, unless it contains important changes (such as added or removed files).
* Do not commit unused files. Sometimes when copying scripts in GM for example, it might create unused files. Delete them.

## Logo
The GMnet logos use icons from Entypo (http://entypo.com/) and Open Iconic (https://useiconic.com/open/). They are licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).