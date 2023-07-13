cmake_minimum_required(VERSION 2.4.6)
project(platform_description)

find_package(catkin)

if(${catkin_FOUND})

  catkin_package()

  configure_file(module/alias.in.yaml $ENV{HOME}/.config/mc_rtc/aliases/Platform.yaml)

else()

  include(CMakePackageConfigHelpers)
  set(VERSION_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
  set(PROJECT_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake")
  set(CONFIG_INSTALL_DIR "lib/cmake/${PROJECT_NAME}")
  write_basic_package_version_file(
    ${VERSION_CONFIG}
    VERSION 1.0.0
    COMPATIBILITY SameMajorVersion
  )
  configure_package_config_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in" "${PROJECT_CONFIG}" INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}")
  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake" "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake" DESTINATION "${CONFIG_INSTALL_DIR}")

  set(CATKIN_PACKAGE_SHARE_DESTINATION share/mc_rtc/${PROJECT_NAME})

  configure_file(module/alias.yaml ${CMAKE_CURRENT_BINARY_DIR}/module/Platform.yaml)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/module/Platform.yaml ${CMAKE_INSTALL_PREFIX}/lib/mc_robots/aliases)

endif()

configure_file(mujoco/platform.yaml $ENV{HOME}/.config/mc_rtc/mc_mujoco/platform.yaml)

install(DIRECTORY convex/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/convex)
install(DIRECTORY meshes/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/meshes)
install(DIRECTORY rsdf/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/rsdf)
install(DIRECTORY urdf/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf)
