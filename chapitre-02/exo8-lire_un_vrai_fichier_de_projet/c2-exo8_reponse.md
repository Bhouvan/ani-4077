```
1) Il y'a 12 dependances de base <br>

2) il y'a 45  conditions et ces dernieres modifient la configuration du projet NKRHi selon les<br>
dependances disponibles.
    * Donc 4 if Python  modifient rhiDeps/rhiDefines selon :
WANT_VULKAN, USE_NKGLAD, USE_NKGLSLANG, USE_NKSPIRVCROSS.

    * 21 blocs with filter(...) portant sur : la plateforme, les options de backend, la configuration
    * Et 20 if imbriqués dans les filtres, surtout sur WANT_VULKAN, USE_NKGLSLANG, USE_NKSPIRVCROSS, VULKAN_LIB, _IS_MINGW

3)Defines : 2 de base + ≥17 conditionnels

    * Toujours posés (2) :
        - NKRENDERER_USE_NKGLAD=0/1
        - NKENTSEU_ENABLE_VULKAN_BACKEND=0/1

    * Conditionnels selon la machine (5) :
        - NK_RHI_VK_ENABLED, NK_RHI_GLSLANG_ENABLED, ENABLE_HLSL, ENABLE_OPT=0, NK_RHI_SPIRVCROSS_ENABLED.

    * Conditionnels selon la plateforme (≥12) :
        - Windows : WIN32_LEAN_AND_MEAN, NK_RHI_DX11_ENABLED, NK_RHI_DX12_ENABLED
        - UWP : NKENTSEU_PLATFORM_UWP
        - Linux X11 : NKENTSEU_FORCE_WINDOWING_XLIB_ONLY, VK_USE_PLATFORM_XLIB_KHR
        - Linux headless : NKENTSEU_FORCE_WINDOWING_NOOP_ONLY
        - Linux XCB : NKENTSEU_FORCE_WINDOWING_XCB_ONLY, VK_USE_PLATFORM_XCB_KHR
        - Linux Wayland : WAYLAND_DEFINES, VK_USE_PLATFORM_WAYLAND_KHR
        - macOS/iOS : NK_RHI_METAL_ENABLED
        - Android : NK_OPENGL_ES, VK_USE_PLATFORM_ANDROID_KHR
        - HarmonyOS  : NK_OPENGL_ES, VK_USE_PLATFORM_OHOS_KHR
        - Web : NK_OPENGL_ES

    * Conditionnels selon la configuration (3) :
        - Debug : _DEBUG, DEBUG ; Release : NDEBUG.