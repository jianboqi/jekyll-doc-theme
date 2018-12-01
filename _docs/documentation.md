---
title: Documentation
permalink: /docs/documentation/
redirect_from: /docs/index.html
---

{{% toc %}}

#### A new version of User Manual can be found here: [`LESS User Manual (V1.8.4)`](http://lessrt.org/Attachments/LESS_User_Manual_1.8.4.pdf)


<br><br>

#### Use LESS under linux

Currently, the GUI is not provided for linux systems, because of some difficulties to package all of them in a single installer.
However, the radiative transfer core can run on linux with commandline. The workflow is:
**Generate scene file (windows)**->**upload scene file to linux**->**run with commandline**. This is particularly useful when we want to do simulation with a powerful linux server (e.g., high performance computer or clusters).

After scene generation, under "Parameters" folder, there is a folder named "_scenefile", you can copy all the contents of the folder into the linux system.
and run with **`lessrt _scenefile/main.xml`**. If you are doing a batch simulation, you can also do all the simulations with **`lessrt _scenefile/nameofbatch*.xml`**

##### The commandline version of LESS can be downloaded from:
Ubuntu 16.04: [LESS-1.8.6-Ubuntu16.04.zip](https://github.com/jianboqi/lessrt/releases/download/LESS1.8.6/LESS-1.8.6-Ubuntu16.04.zip)

CentOS 6.8: [LESS-1.8.6-CentOS6.8.zip](https://github.com/jianboqi/lessrt/releases/download/LESS1.8.6/LESS-1.8.6-CentOS6.8.zip)
