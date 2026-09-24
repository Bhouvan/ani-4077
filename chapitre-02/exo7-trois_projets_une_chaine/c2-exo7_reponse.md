```
=>Quand je n'ajoute pas "MonUtil" a links de Essai j'ai cet erreur:

        Compilation Error: Link Failed   
    C:/msys64/ucrt64/bin/ld:                                                                     ║
    D:\ani-4077\Build\Lib\Debug-Windows/MonRhi.lib(src_MonRhi_MonRhi.obj): in function           ║
    `monrhi::BackendActif[abi:cxx11]()':                                                         ║
    D:\ani-4077\src\MonRhi/MonRhi.cpp:9:(.text+0x27): undefined reference to                     ║
   `MonUtil::Afficher()'                                                                        ║
    clang++: error: linker command failed with exit code 1 (use -v to see invocation) 

=> Rien ne se passe quand j'inverse l'ordre au niveau de links sous windows
```