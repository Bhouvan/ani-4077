# Journal - construction


# chapitre-02 — links() hors projet

    # message d'erreur
        <"Error loading workspace: 'links()' used outside any project block — put it inside 'with project(...):' (at D:\ani-4077\MonRhi.jenga:48). It would have had no effect: refused rather than ignored.">

    # Cause réelle
        <"Les blocs with filter('system:Linux') et with filter('system:Windows') étaient indentés au niveau du workspace, pas dans with project('Essai'). links() n'a de sens qu'à l'intérieur d'un projet.">

    # Temps mis pour trouver l'erreur
        3min

# chapitre-02 — MonRhi/MonRhi.h introuvable

    # message d'erreur
        <"fatal error: 'MonRhi/MonRhi.h' file not found">

    # Cause réelle
        <"Les sources étaient dans D:\ani-4077\chapitre-02\src\, mais le .jenga pointait sur src\ à la racine. Jenga compilait un main.cpp venu de chapitre-02, et includedirs(['src']) ne correspondait à rien.">

    # Temps mis pour trouver l'erreur
        10 min

# Essai — exécutable introuvable

    # message d'erreur
        <"Executable not found: D:\ani-4077\Build\Bin\Debug-Windows\Essai\Essai.exe">

    # Cause réelle
        <"Le projet Essai n'avait produit aucun exécutable. Le dossier Build\Bin\ n'existait même pas. Le build de MonRhi passait, mais Essai n'avait rien à compiler.">

    # Temps mis pour trouver l'erreur
        5min

# Essai — aucun fichier source

    # message d'erreur
        <"No source files found for project Essai">

    # Cause réelle
        <"Le motif src/Essai/**.cpp ne matchait rien. main.cpp n'était pas au bon endroit par rapport au .jenga. Jenga ne signale pas un projet vide comme une erreur : il affiche juste Projects Built: 2/2 et ne produit rien.">

    # Temps mis pour trouver l'erreur
        6min


# Workspace — no .jenga workspace file found

    # message d'erreur
        <"No .jenga workspace file found.">

    # Cause réelle
        <"J'avais commenté le with workspace(...) pour tester. Le fichier restait du Python valide, mais aucun workspace n'était enregistré à l'exécution. Jenga 2.8 refuse au lieu d'afficher Projects Built: 0/0 comme dans le livre.">

    # Temps mis pour trouver l'erreur
        0min

# MonRhi — NKTime/NkChrono.h introuvable

    # message d'erreur
        <"fatal error: 'NKTime/NkChrono.h' file not found">

    # Cause réelle
        <"Le projet MonRhi n'avait pas includedirs(INCLUDES). Le compilateur cherchait NKTime/NkChrono.h dans src/, qui n'existe pas. Il fallait ajouter les chemins d'inclusion du moteur.">

    # Temps mis pour trouver l'erreur
        2min

# NKTime — timeBeginPeriod, timeGetDevCaps, timeEndPeriod

    # message d'erreur
        <"undefined reference to `timeBeginPeriod'
undefined reference to `timeGetDevCaps'
undefined reference to `timeEndPeriod'">

    # Cause réelle
        <"NKTime utilise l'API multimédia Windows, qui vit dans winmm. Une bibliothèque système ne s'ajoute pas toute seule : il faut la nommer dans links. C'est la première catégorie d'erreur : un module système manquant.">

    # Temps mis pour trouver l'erreur
        2min

# NKTime — nkentseu::NkSnprintf et nkentseu::NkString

    # message d'erreur
        <"undefined reference to `nkentseu::NkSnprintf(char*, unsigned long long, char const*, ...)'
undefined reference to `nkentseu::NkString::NkString(char const*)'">

    # Cause réelle
        <"NKTime dépend de NKCore (NkSnprintf) et NKContainers (NkString). Ni l'un ni l'autre n'était dans links. Le namespace du symbole manquant donne le nom du module : nkentseu::NkSnprintf → NKCore, nkentseu::NkString → NKContainers.">

    # Temps mis pour trouver l'erreur
        5min

# NKContainers — nkentseu::memory::NkGetDefaultAllocator

    # message d'erreur
        <"undefined reference to `nkentseu::memory::NkGetDefaultAllocator()'
undefined reference to `nkentseu::memory::NkMemCopy(void*, void const*, unsigned long long)'
undefined reference to `nkentseu::memory::NkMemSet(void*, int, unsigned long long)'
undefined reference to `nkentseu::memory::NkMemMove(void*, void const*, unsigned long long)'
undefined reference to `nkentseu::memory::NkMemCompare(void const*, void const*, unsigned long long)'">

    # Cause réelle
        <"NKContainers dépend de NKMemory. J'avais oublié NKMemory dans links. Le namespace memory pour le module NKMemory. Trouvé en cherchant NkGetDefaultAllocator dans les sources : Kernel\Foundation\NKMemory\src\NKMemory\.">

    # Temps mis pour trouver l'erreur
        9min


# Essai — nkentseu::NkString::NkString (deuxième fois)

    # message d'erreur
        <"undefined reference to `timeBeginPeriod'
undefined reference to `timeGetDevCaps'
undefined reference to `timeEndPeriod'
undefined reference to `nkentseu::NkString::NkString(char const*)'">

    # Cause réelle
        <"J'avais enlevé NKContainers et winmm de links en simplifiant. Le message redonne exactement les deux modules manquants. Une bibliothèque statique n'embarque pas ses dépendances : Essai doit lister toute la chaîne.">

    # Temps mis pour trouver l'erreur
        5 min

# MonRhi.jenga — Error loading workspace: 'ON'

    # message d'erreur
        <"Error loading workspace: 'ON'">

    # Cause réelle
        <"symbols() attend un booléen Python (True/False), pas une chaîne. J'avais écrit symbols('On') en m'inspirant de optimize('Off') qui, lui, attend une chaîne. Trouvé en regardant comment le moteur lui-même appelle symbols dans ConquerorProto.jenga.">

    # Temps mis pour trouver l'erreur
        5min


# Debug et Release — temps identiques

    # message d'erreur
        <"Debug : 279,517 ms
Release : 280,329 ms">

    # Cause réelle
        <"Les blocs with filter('configurations:Debug') et with filter('configurations:Release') étaient dans le projet Essai uniquement. Or la boucle mesurée est dans MonRhi.cpp, et MonRhi n'avait aucune option. Jenga compilait MonRhi en -O0 dans les deux configurations. Les 2% d'écart, c'était du bruit machine.">
     # Temps mis pour trouver l'erreur
        33 min