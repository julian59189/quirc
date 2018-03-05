cmake_minimum_required(VERSION 3.5)
project(quirc)

set(CMAKE_CXX_STANDARD 11)

find_package(OpenCV 3.2.0 REQUIRED)
message("Use OpenCV: ${OpenCV_VERSION}")

find_package(Boost COMPONENTS system filesystem REQUIRED)
message("Use Boost: ${Boost_VERSION}")

set(LIB_SOURCE
        lib/decode.c
        lib/identify.c
        lib/quirc.c
        lib/quirc_internal.h
        lib/version_db.c
)

set(DEMO_SOURCE
    demo/camera.c
    demo/mjpeg.c
    demo/convert.c
    demo/dthash.c
    demo/demoutil.c
)


#add_executable(libquirk ${LIB_SOURCE})
add_library(quirc_dyn SHARED ${LIB_SOURCE})
target_link_libraries(quirc_dyn ${OpenCV_LIBS} ${Boost_LIBRARIES} -pg)

add_library(quirc_stat STATIC ${LIB_SOURCE})
target_link_libraries(quirc_stat ${OpenCV_LIBS} ${Boost_LIBRARIES} -pg)

add_executable(qrtest tests/dbgutil.c tests/qrtest.c)
target_include_directories(qrtest PRIVATE ${CMAKE_SOURCE_DIR}/lib)
target_link_libraries(qrtest ${CMAKE_SOURCE_DIR}/build/libquirc_stat.a -lm -ljpeg -lpng)