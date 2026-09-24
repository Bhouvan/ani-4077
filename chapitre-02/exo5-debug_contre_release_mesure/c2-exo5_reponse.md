```
fichiers:
=>src\Essai\main.cpp

    #include <iostream>
    #include "MonRhi/MonRhi.h"

    int main() {
        int somme = 100000000;
        nkentseu::NkElapsedTime elapsed =  monrhi::sommemillion(somme);
        std::cout<< "Le temps ecoule pour la somme d'une suite arithmetique allant de 0 a 100 000 000 en debug en milliseconde est de " << elapsed.ToMilliseconds() <<"\n";

        return 0;
    }


=>src\MonRhi\MonRhi.h

    #pragma once

    #include <string>
    #include "NKTime/NkChrono.h"
    //#include "NKCore/Logger/NkLogger.h"
    //using namespace nkentseu;
    namespace monrhi {
        const char* Version();
        std::string BackendActif();
        nkentseu::NkElapsedTime sommemillion(int count);

    }


=>src\MonRhi\MonRhi.cpp

    #include "MonRhi.h"
    #include <iostream>
    namespace monrhi {
        const char* Version()          { return "MonRhi 0.2"; }
        std::string BackendActif()     { return "aucun"; }
        nkentseu::NkElapsedTime sommemillion(int count)
        {
            nkentseu::NkChrono chrono;
            double somme = 0.0;
            for (size_t i = 0; i < count; i++)
            {
                somme +=i;
            }
            std::cout<<"Somme :" << somme << "\n";
            nkentseu::NkElapsedTime elapsed = chrono.Elapsed();
            return elapsed;
        }
    }
    
=> fichiers de projet
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
    with project("MonRhi"):
        staticlib() #indique c'est une bibliotheque qui produira une archive
        language("C++") #precise le language
        cppdialect("C++17") #  La version du language
        location(".")   # L'emplacement de ma Bibliotheque
        files(["src/MonRhi/**.cpp"]) # les fichiers qui seront compiles
        includedirs(["src"])#pour inclure ses propres entetes #si oublie alors fatal error: 'MonRhi/MonRhi.h' file not found  
        includedirs(INCLUDES) #Les includes provenant du moteur externe 
        libdirs([LIBDIR]) # les fichiers de liaison provenant du moteur externe #si oublie alors ld: cannot find -lMonRhi
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release
        targetdir("%{wks.location}/Build/Lib/" # Indication du lieu de creation des fichiers de liaison de la biblio* par le linker
                  "%{cfg.buildcfg}-%{cfg.system}") # Pour ne pas ecrase le Debug en cas de configuation en Release
        with filter("configurations:Debug"):
            defines(["_DEBUG"])
            optimize("Off")
            symbols(True)
        
        with filter("configurations:Release"):
            defines(["NDEBUG"])
            optimize("Speed")
            symbols(False)
        

        #Projet
    with project("Essai"):
        consoleapp() #indique c'est le programme qui produira une une .exe
        language("C++") #precise le language
        cppdialect("C++17") #  La version du language
        location(".")   # L'emplacement de ma Bibliotheque
        files(["src/Essai/**.cpp"]) # les fichiers du projet 
        includedirs(["src"]) #Pour inclure ses propres entetes  #si oublie alors ld: cannot find -lMonRhi
        includedirs(INCLUDES) #Les includes provenant du moteur externe
        libdirs(["%{wks.location}/Build/Lib/"# Indication du lieu de creation des fichiers de liaison de la biblio* par le linker
                        "%{cfg.buildcfg}-%{cfg.system}"]) #si oublie alors ld: cannot find -lMonRhi
        libdirs([LIBDIR]) # Les fichiers de liaison provenant du moteur externe 
        links(["MonRhi","NKTime", "NKCore", "NKContainers","NKMemory",  "winmm"]) # Les projets du moteur que l'on souhaite importer pour ce projetc (ici) # undefined reference to ... 
        objdir("%{wks.location}/Build/Obj/" # Indication du lieu de creation des fichier de build binaire de la bibiotheque
               "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le debug en cas de configuation en release
        targetdir("%{wks.location}/Build/Bin/" # Indication du lieu de creation des fichiers du linker
                  "%{cfg.buildcfg}-%{cfg.system}/%{prj.name}") # Pour ne pas ecrase le Debug en cas de configuation en Release
        

        with filter("system:Linux"):
            links(["%{wks.location}/Build/Lib/"
                "%{cfg.buildcfg}-%{cfg.system}/Tools.a"])
        with filter("system:Windows"):
            links(["MonRhi"])
        with filter("configurations:Debug"):
            defines(["_DEBUG"])
            optimize("Off")
            symbols(True)

        with filter("configurations:Release"):
            defines(["NDEBUG"])
            optimize("Speed")
            symbols(False)


=>Somme :5e+15

En configuration Debug, Le temps ecoule pour la somme d'une suite arithmetique
allant de 0 a 100 000 000 en debug en milliseconde est de 290.733


En configuration Release,  Le temps ecoule pour la somme d'une suite arithmetique
allant de 0 a 100 000 000 en Release en milliseconde est de 130.351

La configuration debug prend ~160 millisonde de plus pour execute.
```