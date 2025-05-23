file(GLOB_RECURSE SOURCES main.cpp testExecutorManager.cpp testKeyLocks.cpp testScheduler.cpp testChecksumAddress.cpp testDmcStepRecorder.cpp testExecutivePool.cpp testDmcExecutor.cpp testSchedulerImpl.cpp testBlockExecutive.cpp testShardingBlockExecutive.cpp testShardingKeyLocks.cpp )

# cmake settings
set(TEST_BINARY_NAME test-scheduler)

add_executable(${TEST_BINARY_NAME} ${SOURCES})
target_include_directories(${TEST_BINARY_NAME} PRIVATE . ${CMAKE_SOURCE_DIR}/bcos-scheduler/src)
target_compile_options(${TEST_BINARY_NAME} PRIVATE -Wno-unused-variable)

find_package(Boost REQUIRED unit_test_framework serialization)

target_link_libraries(${TEST_BINARY_NAME} ${SCHEDULER_TARGET} ${LEDGER_TARGET} ${CRYPTO_TARGET}
Boost::unit_test_framework ${TARS_PROTOCOL_TARGET} Boost::serialization ${STORAGE_TARGET} ${SECURITY_TARGET})
set_source_files_properties("main.cpp" "testDmcExecutor.cpp" PROPERTIES SKIP_UNITY_BUILD_INCLUSION ON)
set_target_properties(${TEST_BINARY_NAME} PROPERTIES UNITY_BUILD "ON")

add_test(NAME test-scheduler WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY} COMMAND ${TEST_BINARY_NAME})
# include(SearchTestCases)
# config_test_cases("" "${SOURCES}" ${TEST_BINARY_NAME} "${EXCLUDE_SUITES}")
