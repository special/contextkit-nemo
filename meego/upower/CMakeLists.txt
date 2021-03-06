pkg_check_modules(DBUS dbus-1)
pkg_check_modules(MLITE mlite)
pkg_check_modules(GCONF gconf-2.0)

set(PROVIDER upower)
set(INTERFACE power)

CKIT_GENERATE_CONTEXT(${INTERFACE} ${PROVIDER})
CKIT_GENERATE_TEST_MAIN(${INTERFACE} ${PROVIDER})

include_directories(
  ${DBUS_INCLUDE_DIRS}
  ${MLITE_INCLUDE_DIRS}
  ${GCONF_INCLUDE_DIRS}
)

link_directories(
  ${DBUS_LIBRARY_DIRS}
  ${MLITE_LIBRARY_DIRS}
  ${GCONF_LIBRARY_DIRS}
)

set(SRC
  powerplugin.cpp
  )

set(HDRS
  powerplugin.h
  )

qt4_add_dbus_interface_no_ns(SRC DeviceKit.Power.xml power Power)
qt4_add_dbus_interface_no_ns(SRC DeviceKit.Power.Device.xml device Device)

qt4_wrap_cpp(MOC_SRC ${HDRS})

add_ckit_plugin(${PROVIDER} MODULE ${SRC} ${MOC_SRC})
target_link_libraries(${PROVIDER}
  ${QT_QTDBUS_LIBRARY}
  ${MLITE_LIBRARIES}
  ${GCONF_LIBRARIES}
  )

install(TARGETS ${PROVIDER} DESTINATION lib/contextkit/subscriber-plugins)
