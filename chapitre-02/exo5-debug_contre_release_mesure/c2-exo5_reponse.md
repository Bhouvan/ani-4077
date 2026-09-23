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

En configuration Debug, Le temps ecoule pour la somme d'une suite arithmetique
allant de 0 a 100 000 000 en debug en milliseconde est de 295.748


en configuration Release,  Le temps ecoule pour la somme d'une suite arithmetique
allant de 0 a 100 000 000 en Release en milliseconde est de 289.322