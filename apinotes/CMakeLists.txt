set(sources
  Accelerate.apinotes
  Dispatch.apinotes
  ScriptingBridge.apinotes
  os.apinotes
)

set(output_dir "${SWIFTLIB_DIR}/apinotes")

set(inputs)
set(outputs)
foreach(input ${sources})
  list(APPEND inputs "${CMAKE_CURRENT_SOURCE_DIR}/${input}")
  list(APPEND outputs "${output_dir}/${input}")
endforeach()

add_custom_command(
    OUTPUT "${output_dir}"
    COMMAND ${CMAKE_COMMAND} "-E" "make_directory" "${output_dir}")
add_custom_command(
    OUTPUT ${outputs}
    DEPENDS ${inputs} "${output_dir}"
    COMMAND
      "${CMAKE_COMMAND}" "-E" "copy_if_different" ${inputs} "${output_dir}/")

add_custom_target("copy_apinotes"
    DEPENDS "${outputs}" "${output_dir}"
    COMMENT "Copying API notes to ${output_dir}"
    SOURCES "${sources}")

swift_install_in_component(sdk-overlay
    FILES ${sources}
    DESTINATION "lib/swift/apinotes")
