cmake_minimum_required(VERSION 3.20.0)

link_libraries(kernel32 user32 gdi32 comdlg32 ole32 winspool shlwapi comctl32)

if(MINGW)
    add_compile_options(-Wno-narrowing -fpermissive -w)
endif()

option(WTL4MING_BUILD_SAMPLES "Build also sample programs" OFF)
if(WTL4MING_BUILD_SAMPLES)
    add_subdirectory(sample/Alpha)
    add_subdirectory(sample/BmpView)
    add_subdirectory(sample/GuidGen)
    add_subdirectory(sample/MDIDocVw)
    add_subdirectory(sample/MTPad)
    add_subdirectory(sample/Wizard97Test)
    add_subdirectory(sample/WTLExplorer)
endif()
