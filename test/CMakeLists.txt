cmake_minimum_required(VERSION 3.12.4)

SET(CMAKE_CXX_STANDARD 14)

# Download and unpack googletest at configure time
configure_file(CMakeLists.txt.in googletest-download/CMakeLists.txt)
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
        RESULT_VARIABLE result
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
    message(FATAL_ERROR "CMake step for googletest failed: ${result}")
endif()
execute_process(COMMAND ${CMAKE_COMMAND} --build .
        RESULT_VARIABLE result
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
    message(FATAL_ERROR "Build step for googletest failed: ${result}")
endif()

# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Add googletest directly to our build. This defines
# the gtest and gtest_main targets.
add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/googletest-src
        ${CMAKE_CURRENT_BINARY_DIR}/googletest-build
        EXCLUDE_FROM_ALL)

# The gtest/gtest_main targets carry header search path
# dependencies automatically when using CMake 2.8.11 or
# later. Otherwise we have to add them here ourselves
if (CMAKE_VERSION VERSION_LESS 2.8.11)
    include_directories("${gtest_SOURCE_DIR}/include")
endif()

include_directories(../src/)

# Link against gtest or gtest_main as needed
add_executable(tests util/date_test.cpp auth/auth_user/auth_user_manager_test.cpp auth/current_session/current_session_test.cpp auth/auth_user/auth_user_test.cpp util/string_util_test.cpp model/company/user_manager_test.cpp model/offer/offer_test.cpp model/preference/preference_list_test.cpp model/user/admin_test.cpp model/rental/rental_test.cpp model/rental/rental_manager_test.cpp model/schedule/schedule_test.cpp model/user/admin_manager_test.cpp model/user/client_company_manager_test.cpp model/user/client_manager_test.cpp model/vehicle/vehicle_list_test.cpp model/offer/offer_manager_test.cpp model/vehicle/maintained_vehicle_test.cpp model/vehicle/vehicle_manager_test.cpp model/contract/contract_manager_test.cpp)
target_link_libraries(tests PRIVATE gtest_main feup-aeda-proj1)

add_test(NAME tests COMMAND tests)