Avant d'ajouter le fichier Inutile.cpp  a la bibliotheque

    Taille de Essaie.exe = 967 ko

    Taille de MonRhi.lib = 87 ko


Apres avoir jouter le fichier inutile.cpp a la bibliotheque
    
    Taille de Essaie.exe = 967 ko

    Taille de MonRhi.lib = 102 ko


    Le linker ne lie que les fichiers de la bibliotheques qui sont utiliser dans le main.cpp. 
    Etant donne que l'on a pas utiliser ni inclue le fichier Inutile dans le fichier
    main.cpp de du dossier Essai, le linker l'ignore ce qui laisse la taille de l'executable inchange 