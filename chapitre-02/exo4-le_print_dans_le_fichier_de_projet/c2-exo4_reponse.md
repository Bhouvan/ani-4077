Le print apparait juste en haut avant jenga Workspace

La valeur de rhiDeps sur ma machine est : ['NKMath', 'NKTime', 'NKLogger', 'NKEvent', 'NKWindow', 'NKContainers', 'NKMemory', 'NKCore', 'NKPlatform', 'NKThreading', 'NKFileSystem', 'NKSL', 'NKGlad', 'NKGLSlang', 'NKSPIRVCross']

extrait du code NKRHI.jenga
```
    rhiDeps = ["NKMath", "NKTime", "NKLogger", "NKEvent", "NKWindow", "NKContainers",
               "NKMemory", "NKCore", "NKPlatform", "NKThreading", "NKFileSystem", "NKSL"]
    rhiDefines = [
        f"NKRENDERER_USE_NKGLAD={1 if USE_NKGLAD else 0}",
        f"NKENTSEU_ENABLE_VULKAN_BACKEND={1 if WANT_VULKAN else 0}",
    ]
    if WANT_VULKAN:
        rhiDefines.append("NK_RHI_VK_ENABLED")
    if USE_NKGLAD:
        rhiDeps.append("NKGlad")
    if USE_NKGLSLANG:
        rhiDeps.append("NKGLSlang")
        rhiDefines += ["NK_RHI_GLSLANG_ENABLED", "ENABLE_HLSL", "ENABLE_OPT=0"]
    if USE_NKSPIRVCROSS:
        rhiDeps.append("NKSPIRVCross")
        rhiDefines.append("NK_RHI_SPIRVCROSS_ENABLED")

           

    nkentseudependson(
        rhiDeps,
        selfexport="NKRHI",
        extra_includes=["%{wks.location}/Externals"] + ([VULKAN_INCLUDE] if VULKAN_INCLUDE else []),
        extra_defines=rhiDefines,
    )
    #le print qui me donne la valeur du rhiDeps
    print("La valeur de rhiDeps sur ma machine est :", rhiDeps)

```