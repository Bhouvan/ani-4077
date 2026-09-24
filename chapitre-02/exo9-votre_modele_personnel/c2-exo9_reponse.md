import sys<br>
import os<br>
import glob<br>
from Jenga import *<br>
<br>
NKENTSEU = os.environ.get("NKENTSEU", <br>
                    r"D:\Nkentseu") <br>
<br>
INCLUDES = sorted(glob.glob(<br>
    os.path.join(NKENTSEU, "Kernel", "*", "*", "src")))<br>
LIBDIR = os.path.join(NKENTSEU, "Build", "Lib", "Debug-Windows")<br>
<br>
with workspace("Model", location="."):<br>
    configurations(["Debug", "Release"])<br>
    #Ma Bibliotheque<br>
    with project("Tools"):<br>
        staticlib() #indique c'est une bibliotheque qui produira une archive<br>
        language("C++") #precise le language<br>
        cppdialect("C++17") #  La version du language<br>
        location(".")   # L'emplacement de ma Bibliotheque<br>
        files(["src/Tools/**.cpp"]) # les fichiers qui seront compiles<br>
        includedirs(["src"])#pour inclure ses propres entetes<br>
        includedirs(INCLUDES) #Les includes provenant du moteur externe<br>
        libdirs([LIBDIR]) # les fichiers de liaison provenant du moteur externe<br>
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque<br>
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release<br>
        targetdir("%{wks.location}/Build/Lib/" # Indication du lieu de creation des fichiers de liaison de la biblio* par le linker<br>
                  "%{cfg.buildcfg}-%{cfg.system}") # Pour ne pas ecrase le Debug en cas de configuation en Release<br>
<br>
        #Projet<br>
    with project("Nom_Projet"):<br>
        consoleapp() #indique c'est le programme qui produira une une .exe<br>
        language("C++") #precise le language<br>
        cppdialect("C++17") #  La version du language<br>
        location(".")   # L'emplacement de ma Bibliotheque<br>
        files(["src/Nom_Projet/**.cpp"]) # les fichiers du projet <br>
        includedirs(["src"]) #Pour inclure ses propres entetes<br>
        includedirs(INCLUDES) #Les includes provenant du moteur externe<br>
        libdirs(["%{wks.location}/Build/Lib/"# Indication du lieu de creation des fichiers de liaison de la biblio* par le linker<br>
                        "%{cfg.buildcfg}-%{cfg.system}"])<br>
        libdirs([LIBDIR]) # Les fichiers de liaison provenant du moteur externe<br>
        links(["Tools","MonUtil","NKTime", "NKCore"]) # Les projets du moteur que l'on souhaite importer pour ce projetc (ici)<br>
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque<br>
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release<br>
        targetdir("%{wks.location}/Build/Bin/" # Indication du lieu de creation des fichiers du linker<br>
                  "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le Debug en cas de configuation en Release<br>
        <br>
<br>
        with filter("system:Linux"):<br>
            links(["%{wks.location}/Build/Lib/"<br>
                "%{cfg.buildcfg}-%{cfg.system}/Tools.a"])<br>
        with filter("system:Windows"):<br>
            links(["Tools"])<br>