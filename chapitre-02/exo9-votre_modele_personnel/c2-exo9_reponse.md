```
import sys
import os
import glob
from Jenga import *

NKENTSEU = os.environ.get("NKENTSEU", 
                    r"D:\Nkentseu") 

INCLUDES = sorted(glob.glob(
    os.path.join(NKENTSEU, "Kernel", "*", "*", "src")))
LIBDIR = os.path.join(NKENTSEU, "Build", "Lib", "Debug-Windows")

with workspace("Model", location="."):
    configurations(["Debug", "Release"])
    #Ma Bibliotheque
    with project("Tools"):
        staticlib() #indique c'est une bibliotheque qui produira une archive
        language("C++") #precise le language
        cppdialect("C++17") #  La version du language
        location(".")   # L'emplacement de ma Bibliotheque
        files(["src/Tools/**.cpp"]) # les fichiers qui seront compiles
        includedirs(INCLUDES) #Les includes provenant du moteur externe
        libdirs([LIBDIR]) # les fichiers de liaison provenant du moteur externe
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release
        targetdir("%{wks.location}/Build/Lib/" # Indication du lieu de creation des fichiers de liaison de la biblio* par le linker
                  "%{cfg.buildcfg}-%{cfg.system}") # Pour ne pas ecrase le Debug en cas de configuation en Release

        #Projet
    with project("Nom_Projet"):
        consoleapp() #indique c'est le programme qui produira une une .exe
        language("C++") #precise le language
        cppdialect("C++17") #  La version du language
        location(".")   # L'emplacement de ma Bibliotheque
        files(["src/Nom_Projet/**.cpp"]) # les fichiers du projet 
        includedirs(["src"]) #Pour inclure les fichiers de la bibliotheque
        includedirs(INCLUDES) #Les includes provenant du moteur externe
        libdirs(["%{wks.location}/Build/Lib/"# Indication du lieu de creation des fichiers de liaison de la biblio* par le linker
                        "%{cfg.buildcfg}-%{cfg.system}"])
        libdirs([LIBDIR]) # Les fichiers de liaison provenant du moteur externe
        links(["MonRhi","MonUtil","NKTime", "NKCore"]) # Les projets du moteur que l'on souhaite importer pour ce projetc (ici)
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release
        targetdir("%{wks.location}/Build/Lib/" # Indication du lieu de creation des fichiers du linker
                  "%{cfg.buildcfg}-%{cfg.system}") # Pour ne pas ecrase le Debug en cas de configuation en Release
        

        with filter("system:Linux"):
            links(["%{wks.location}/Build/Lib/"
                "%{cfg.buildcfg}-%{cfg.system}/MonRhi.a"])
        with filter("system:Windows"):
            links(["MonRhi"])
```