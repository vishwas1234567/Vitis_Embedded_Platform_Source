# Vitis Base Platform for the zcu102 Board

This platform comes with PetaLinux and includes OpenCV. It is useful
as a base platform for exercising Vitis capabilities and topologies on the zcu102 board.

# Building the Platform

**Last Tested Vivado Release: 2019.2**

The platform build process is entirely scripted. Note that as this platform
build process involves cross-compiling Linux, build of the platform is supported
on Linux environments **only** (although it is possible to build inside a VM or
Docker container).

Also note that the default PetaLinux configuration uses local scratchpad areas. This
will *not* work if you are building on a networked file system; Yocto will error out.
Update PetaLinux to change the build area to a locally-mounted hard drive (most
Xilinx internal network servers have a /scratch or /tmp area for this purpose).

After cloning the platform source, and with both Vivado and PetaLinux set up, run
`make` from the top-level platform directory.

Note that by default this Makefile will install the platform to "platform_repo/zcu102_base/export/zcu102_base/"

# Installing the Yocto SDK

A bundled Yocto SDK "sysroot" is not available with this package by default. To build
non-trivial Linux software for this platform sysroot need to be built and installed.
This can be done with command "make peta_sysroot"
It is installed to "platform_repo/sysroot" once the build completes.

To cross-compile against this platform from the command line, source the
`environment-setup-aarch64-xilinx-linux` script to set up your environment (cross
compiler, build tools, libraries, etc).

# Build instructions

This packages comes with sources to generate hardware specification file (xsa) from Vivado,
petlainux sources to generate the image.ub and platform sources to generate the Vitis platform.

Build platform from scratch:
	make all

Build a platform without modifying hardware:
	make petalinux_proj XSA_DIR=<xsa dir path>
	make pfm XSA_DIR=<xsa dir path>

	example:
		make petalinux_proj XSA_DIR=/home/user/zcu102_base/vivado
		make pfm /home/user/zcu102_base/vivado

# Notes

By default the rootfs is packaged in image.ub in the generated platform. The zcu102 board is having limited
4GB DDR4 memory for PS. In case of adding more rootfs packages or running the applications that require larger
CMA memory, the recommended solution is to load the rootfs from SD card instead of from DDR.
Refer section "Configuring SD Card ext File System Boot" in page 65 of ug1144 for Petalinux 2019.2:
"https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_2/ug1144-petalinux-tools-reference-guide.pdf"

This platform is built with 2019.2 XRT tag: https://github.com/Xilinx/XRT/tree/2019.2_RC1.
To build the platforms with latest 2019.2 XRT branch, change the commit id in the following files by taking it
from https://github.com/Xilinx/XRT/tree/2019.2 and build the platform:
	Xilinx_Official_Platforms/zcu102_base/petalinux/project-spec/meta-user/recipes-xrt/xrt/xrt_git.bb
	Xilinx_Official_Platforms/zcu102_base/petalinux/project-spec/meta-user/recipes-xrt/zocl/zocl_git.bb

Once the Vitis platform is ready, some example applications to build with these platforms can be found here:
https://github.com/Xilinx/Vitis_Accel_Examples
