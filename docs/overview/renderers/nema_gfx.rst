.. _stm32_nema_gfx:

===================================
NemaGFX Acceleration (AKA NeoChrom)
===================================

Some of the more powerful STM32 MCUs such as the
STM32U5 feature a 2.5D GPU which can natively draw most
LVGL primitives.

Get Started with the Riverdi STM32U5 5-inch Display
***************************************************

`lv_port_riverdi_stm32u5 <https://github.com/lvgl/lv_port_riverdi_stm32u5>`__
is a ready-to-use port for the Riverdi STM32 5.0" Embedded Display
(STM32U599NJH6Q or STM32U5A9NJH6Q) which has Nema enabled.
Follow the instructions in the readme to get started.

Usage and Configuration
***********************

Enable the renderer by setting :c:macro:`LV_USE_NEMA_GFX` to ``1`` in
lv_conf.h. If using :c:macro:`LV_USE_NEMA_VG`,
set :c:macro:`LV_NEMA_GFX_MAX_RESX` and :c:macro:`LV_NEMA_GFX_MAX_RESY`
to the size of the display you will be using so that enough static
memory will be reserved for VG. Without VG, more task types will be
performed by the software renderer.

"libs/nema_gfx" contains pre-compiled binaries for the Nema GPU
drivers. In `lv_port_riverdi_stm32u5 <https://github.com/lvgl/lv_port_riverdi_stm32u5>`__
the project is already configured to link the binaries when building.
With a different STM32CubeIDE project, you can configure the libraries to be linked
by right-clicking the project in the "Project Explorer" sidebar, clicking
"Properties", navigating to "C/C++ Build", "Settings", "MCU G++ Linker", and then
"Libraries". Add an entry under "Libraries (-l)" that is "nemagfx-float-abi-hard".
Add an entry under "Library search path (-L)" which is a path to
"libs/nema_gfx/lib/core/cortex_m33/gcc" e.g.
"${workspace_loc:/${ProjName}/Middlewares/LVGL/lvgl/libs/nema_gfx/lib/core/cortex_m33/gcc}".
You will also want to add the "libs/nema_gfx/include" directory to your include
search paths. Under "MCU GCC Compiler", "Include paths", add an entry to "Include paths (-I)"
which is a path to "libs/nema_gfx/include" e.g.
"${workspace_loc:/${ProjName}/Middlewares/LVGL/lvgl/libs/nema_gfx/include}".
Click "Apply and Close".

32 and 16 bit :c:macro:`LV_COLOR_DEPTH` is supported.

At the time of writing, :c:macro:`LV_USE_OS` support is experimental
and not yet working in
`lv_port_riverdi_stm32u5 <https://github.com/lvgl/lv_port_riverdi_stm32u5>`__

"src/draw/nema_gfx/lv_draw_nema_gfx_hal.c" implements the HAL functionality
required by Nema to allocate memory and lock resources (in this implementation,
no locking is done). It may conflict with existing definitions
if you have an existing Nema HAL implementation. You may
simply be able to remove yours.

DMA2D
*****

The Nema renderer uses DMA2D to flush in parallel with rendering in
`lv_port_riverdi_stm32u5 <https://github.com/lvgl/lv_port_riverdi_stm32u5>`__.

If your STM does not have the Nema GPU, it may still support
DMA2D. DMA2D is a simple peripheral which can draw fills
and images independently of the CPU.
See the LVGL :ref:`DMA2D support <dma2d>`.

API
***

:ref:`lv_draw_nema_gfx_h`

:ref:`lv_draw_nema_gfx_utils_h`
