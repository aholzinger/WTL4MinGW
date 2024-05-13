cmake_minimum_required(VERSION 3.20.0)

option(WTL4MINGW_USE_DEBUGBREAK "Use scottt's debugbreak header for breaking into debugger" OFF)
if(WTL4MINGW_USE_DEBUGBREAK)
    set(WTL4MINGW_USE_DEBUGBREAK_REPO "https://github.com/scottt/debugbreak.git" CACHE STRING "debugbreak repository to use")
    if(NOT WTL4MINGW_USE_DEBUGBREAK_TAG)
        if($ENV{WTL4MINGW_USE_DEBUGBREAK_TAG})
            set(WTL4MINGW_USE_DEBUGBREAK_TAG $ENV{WTL4MINGW_USE_DEBUGBREAK_TAG})
        else()
            set(WTL4MINGW_USE_DEBUGBREAK_TAG "5dcbe41d2bd4712c8014aa7e843723ad7b40fd74")   # set default tag, when neither cache nor environment variable found.
        endif()
    endif()

    FetchContent_Declare(debugbreak_build
        GIT_TAG         ${WTL4MINGW_USE_DEBUGBREAK_TAG}
        GIT_REPOSITORY  ${WTL4MINGW_USE_DEBUGBREAK_REPO}
    )
    message(STATUS "Fetching debugbreak sources ${WTL4MINGW_USE_DEBUGBREAK_TAG} from git at ${WTL4MINGW_USE_DEBUGBREAK_REPO}")
    FetchContent_GetProperties(debugbreak_build)
    if(NOT debugbreak_build_POPULATED)
        FetchContent_Populate(debugbreak_build)
        message(STATUS "Building debugbreak in ${debugbreak_build_BINARY_DIR} from sources in ${debugbreak_build_SOURCE_DIR}")
        add_subdirectory(${debugbreak_build_SOURCE_DIR} ${debugbreak_build_BINARY_DIR} EXCLUDE_FROM_ALL)
    endif()

    include_directories(
        ${debugbreak_build_SOURCE_DIR}
        atlport/include
        stdext/include
        wtl/include
        )
else()
    include_directories(
        atlport/include
        stdext/include
        wtl/include
        )
endif()

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
