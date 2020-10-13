# CPlusPlus_DLL

VS Win32 Console Application empty DLL project

## header file DLL_H.h

    #ifndef _DLL_H_
    #define _DLL_H_
    #include <iostream>
 
    #if defined DLL_EXPORT
    #define DECLDIR __declspec(dllexport)
    #else
    #define DECLDIR __declspec(dllimport)
    #endif
 
    extern "C"
    {
       DECLDIR int Add( int a, int b );
       DECLDIR void Function( void );
    }    
 
    #endif

## source file DLL.cpp

    #include <iostream>
    #include "DLL_H.h"
 
    #define DLL_EXPORT
 
    extern "C"
    {
        DECLDIR int Add( int a, int b )
        {
        return( a + b );
        }
 
        DECLDIR void Function( void )
        {
         std::cout << "DLL Called!" << std::endl;
        }
    }


## explicit Linking

    #include <iostream>
    #include <windows.h>
 
    typedef int (*AddFunc)(int,int);
    typedef void (*FunctionFunc)();
 
    int main()
    {
        AddFunc _AddFunc;
        FunctionFunc _FunctionFunc;
        HINSTANCE hInstLibrary = LoadLibrary("DLL_H.dll");
 
        if (hInstLibrary)
        {
            _AddFunc = (AddFunc)GetProcAddress(hInstLibrary, "Add");
            _FunctionFunc = (FunctionFunc)GetProcAddress(hInstLibrary,
         "Function");
 
            if (_AddFunc)
            {
                std::cout << "23 = 43 = " << _AddFunc(23, 43) << std::endl;
            }
            if (_FunctionFunc)
            {
                _FunctionFunc();
            }
 
            FreeLibrary(hInstLibrary);
        }
        else
        {
            std::cout << "DLL Failed To Load!" << std::endl;
        }
 
    std::cin.get();
 
    return 0;
}
