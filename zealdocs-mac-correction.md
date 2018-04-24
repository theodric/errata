The documentation at https://github.com/zealdocs/zeal/wiki/Build-Instructions-for-macOS instructs users to clone https://github.com/zealdocs/zeal.git and then perform some build operations on it to generate an .app package.

The problem is that https://github.com/zealdocs/zeal.git does not contain all of the files necessary to build a project with qmake. Specifically, the files zeal.pro & .qmake.conf, as well as the entire qmake directory are missing.

https://github.com/zealdocs/zeal/tree/stable contains the necessary files, but if you try to clone the repo from the "Clone or download" button, it will simply clone the current master repo again. You must instead click that button, then "Download ZIP."

Once you have unpacked the archive, you can follow the instructions in the first link to build the project.

The instructions also specify an incorrect fully-qualified path to the *macdeployqt* tool. The correct path is the same as the fully-qualified path to qmake, namely */usr/local/Cellar/qt@5.5/5.5.1_1/bin/macdeployqt* as of the writing of this document.
