# parallella playground

The aim is to create a one stop environment for parallella development

### References:

https://github.com/peteasa/parallella-yoctobuild - A Simple build environment for [Parallella](http://www.parallella.org/) using [Yocto](http://www.yoctoproject.org/)

https://github.com/parallella/oh.git - PARALLELLA: Supercomputing for Everyone - open hardware FPGA design associated with the Parallella project.

https://github.com/analogdevicesinc/hdl - Analog Devices HDL libraries and projects

https://github.com/Xilinx/device-tree-xlnx.git - Xilinx device-tree tcl generation scripts used with the Xilinx SDK to generate a template device tree.

### Provides:

A working environment for a developer to take an idea from concept to working release on the Parallella platform.

As an option you can use the Xilinx tools to build the parallella fpga or you can use the fpga that I have pre-built.

The main requirement is for a few build essentials required for the Yocto Linux build, but after that the job of ensuring that the matching Linux, Analog Devices hdl, Open Hardware is provided by choosing the appropriate branch in this repository and checking out the matching submodules.

Two branches are significant in this repository:

- [elink-redesign](https://github.com/peteasa/parallella) - this branch
- [parallella-elink-redesign](https://github.com/peteasa/parallella/tree/parallella-elink-redesign) - contains an example layer that demonstrates how to extend the yocto build to add your own design.  See the [parallella](https://github.com/peteasa/parallella/wiki) project for more details and Tutorials

## Instructions

### Getting Started Guide

I realise there is a lot to take in with this project so I have create a [Getting Started Guide](https://github.com/peteasa/parallella/wiki/Getting-started) and [Quick Start](https://github.com/peteasa/parallella/wiki/Quick-start) list of instructions.  [Feedback is always welcome](https://parallella.org/forums/viewtopic.php?f=49&t=3180)

### Installing required packages for yocto

To use `yocto` you first need to install some packages. See latest [Yocto Project Quick Start](http://www.yoctoproject.org/docs/latest/yocto-project-qs/yocto-project-qs.html). This assumes you are working on a Ubuntu machine:

```bash
$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm
```

### Installing required software for Xilinx fpga development

As an option you can build the oh fpga in ./parallella-fpga or you can use the bit bin files I provide in ./examples/fpga/bitstreams.  To build your own fpga with the parallella template project ./parallella-fpga/7020_hdmi you need to install Vivado 2015.2 see http://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2015-2.html, download 2015.2. I am also using the 2015.2.1 update.  Once Vivado is installed and setup it is as simple as running make to create the bitstream!

### Cloning this repository

Clone this repository onto your Linux build machine:
```bash
$ git clone git@github.com:peteasa/parallella
$ cd parallella
```

Checkout the branch that provides the versions that you want to use then to prepare the environment and download the necessary git submodules, you need to run the `initgitsubmodules.sh` script. This only needs to be done once:

```bash
$ source initgitsubmodules.sh
```

The result will be new folders `examples`, `parallella-fpga/oh`, `parallella-fpga/7020_hdmi`, `parallella-fpga/AdiHDLLib`, `parallella-yoctobuild`, `parallella-yoctobuild/poky`, `parallella-yoctobuild/meta-xilinx`, `parallella-yoctobuild/meta-parallella` and `parallella-yoctobuild/meta-epiphany` created from specific commits on github.

### Setting up your shell environment

For full instructions to setup the parallella-yoctobuild environment see https://github.com/peteasa/parallella-yoctobuild

For partial instructions to setup and use xilinx tools to build an fpga visit https://www.parallella.org/2015/03/23/new-parallella-elink-fpga-design-project-now-available-in-vivado/.  I provide a top level makefile to build the parallella fpga.

```bash
$ source xilinx/Vivado/2015.2/settings64.sh
$ cd ./parallella-fpga
$ make all
```

If all goes well after the make process is done the ./parallella-fpga/7020_hdmi and ./parallella-fpga/7010_hdmi folders will contain a bitstream.

For instructions that need to be adapted to add more to the fpga be inspired by http://parallellagram.org/

### Adding your own projects or modifying this environment

There are four folders in .gitignore that are ignored by this repository.  You can use these folder to store code for your own projects:

```bash
$ mkdir mywork
$ mkdir project
$ mkdir projects
$ mkdir test
```

There is a corresponding folder in the parallella-yoctobuild directory for the yocto changes that you might need to make for your project. And a corresponding folder in the parallella-fpga directory for your fpga project.  If you use these folders for your work then you dont need to modify any of the files I provide, making git updating easier (no conflicts or local checked out files).

**DANGER** You may need to clean the parallella-fpga project before you attempt to update. As this will remove a lot of generated files and may also remove some files that you want to keep please take care, but consider running

```bash
cd parallella-fpga
source revertlocalchanges.sh
```

Before you run updatesubmodules.sh to update and get the latest versions of the git submodules.

### Update and Changing branch

You may wish to change branch.  For example parallella-elink-redesign branch contains a sample yocto layer.  To make this process easy run the following from the parallella folder:

```bash
$ git checkout parallella-elink-redesign
$ source ./updatesubmodules.sh
```

Note that git submodules and git subtree are not easy to manage.  Usually this script will work.  If it does not then follow the instructions and tidy up by hand.  For example a recent change was to remove parallella-yoctobuild/meta-xilinx.  Running the above script could produce a "Warning: Changes detected in parallella-yoctobuild/meta-xilinx/". Go to the offending submodule and use "git gui" to view the changes in the submodule.  In this case simply remove the folder meta-xilinx to remove the warning, in other cases it may not be so easy. Always backup or commit any changes that you have made before running these scripts!

### Updates to submodules

From time to time I update the source of the submodules or add new submodules.  It is a good idea to run git submodule sync in each submodule:

```bash
$ git submodule sync
$ git submodule foreach git submodule sync
```

so that the contents of the .gitmodules are read and any changes are made.  I have included these commands in the scripts to give you an idea about the sequence.

### Links to other information

Yocto Troubleshooting notes - [Troubleshooting notes](https://github.com/peteasa/parallella-yoctobuild/wiki/Troubleshooting-notes)

Vivado Troubleshooting notes - TODO

Instructions for contributors - [Instructions for contributors](https://github.com/peteasa/parallella-yoctobuild/wiki/Instructions-for-contributors)


---------------------------------------

  * TODO instructions for building the examples
  * TODO oh yes need to add / create the examples!
  * TODO instructions for adding new parts from the Analog Devices libraries

