# add\_library

使用指定的源文件作为库添加到项目中。

```
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            source1 [source2 ...])
```

Adds a library target called`<name>`to be built from the source files listed in the command invocation. The`<name>`corresponds to the logical target name and must be globally unique within a project. The actual file name of the library built is constructed based on conventions of the native platform \(such as`lib<name>.a`or`<name>.lib`\).

`STATIC`,`SHARED`, or`MODULE`may be given to specify the type of library to be created.`STATIC`libraries are archives of object files for use when linking other targets.`SHARED`libraries are linked dynamically and loaded at runtime.`MODULE`libraries are plugins that are not linked into other targets but may be loaded dynamically at runtime using dlopen-like functionality. If no type is given explicitly the type is`STATIC`or`SHARED`based on whether the current value of the variable[`BUILD_SHARED_LIBS`](https://cmake.org/cmake/help/v3.0/variable/BUILD_SHARED_LIBS.html#variable:BUILD_SHARED_LIBS)is`ON`. For`SHARED`and`MODULE`libraries the[`POSITION_INDEPENDENT_CODE`](https://cmake.org/cmake/help/v3.0/prop_tgt/POSITION_INDEPENDENT_CODE.html#prop_tgt:POSITION_INDEPENDENT_CODE)target property is set to`ON`automatically.

By default the library file will be created in the build tree directory corresponding to the source tree directory in which thecommand was invoked. See documentation of the[`ARCHIVE_OUTPUT_DIRECTORY`](https://cmake.org/cmake/help/v3.0/prop_tgt/ARCHIVE_OUTPUT_DIRECTORY.html#prop_tgt:ARCHIVE_OUTPUT_DIRECTORY),[`LIBRARY_OUTPUT_DIRECTORY`](https://cmake.org/cmake/help/v3.0/prop_tgt/LIBRARY_OUTPUT_DIRECTORY.html#prop_tgt:LIBRARY_OUTPUT_DIRECTORY), and[`RUNTIME_OUTPUT_DIRECTORY`](https://cmake.org/cmake/help/v3.0/prop_tgt/RUNTIME_OUTPUT_DIRECTORY.html#prop_tgt:RUNTIME_OUTPUT_DIRECTORY)target properties to change this location. See documentation of the[`OUTPUT_NAME`](https://cmake.org/cmake/help/v3.0/prop_tgt/OUTPUT_NAME.html#prop_tgt:OUTPUT_NAME)target property to change the`<name>`part of the final file name.

If`EXCLUDE_FROM_ALL`is given the corresponding property will be set on the created target. See documentation of the[`EXCLUDE_FROM_ALL`](https://cmake.org/cmake/help/v3.0/prop_tgt/EXCLUDE_FROM_ALL.html#prop_tgt:EXCLUDE_FROM_ALL)target property for details.

See the[`cmake-buildsystem(7)`](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#manual:cmake-buildsystem%287%29)manual for more on defining buildsystem properties.

---

```
add_library(<name> <SHARED|STATIC|MODULE|UNKNOWN> IMPORTED
            [GLOBAL])
```

An[IMPORTED library target](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#imported-targets)references a library file located outside the project. No rules are generated to build it, and the[`IMPORTED`](https://cmake.org/cmake/help/v3.0/prop_tgt/IMPORTED.html#prop_tgt:IMPORTED)target property is`True`. The target name has scope in the directory in which it is created and below, but the`GLOBAL`option extends visibility. It may be referenced like any target built within the project.`IMPORTED`libraries are useful for convenient reference from commands like[`target_link_libraries()`](https://cmake.org/cmake/help/v3.0/command/target_link_libraries.html#command:target_link_libraries). Details about the imported library are specified by setting properties whose names begin in`IMPORTED_`and`INTERFACE_`. The most important such property is[`IMPORTED_LOCATION`](https://cmake.org/cmake/help/v3.0/prop_tgt/IMPORTED_LOCATION.html#prop_tgt:IMPORTED_LOCATION)\(and its per-configuration variant[`IMPORTED_LOCATION_<CONFIG>`](https://cmake.org/cmake/help/v3.0/prop_tgt/IMPORTED_LOCATION_CONFIG.html#prop_tgt:IMPORTED_LOCATION_<CONFIG>)\) which specifies the location of the main library file on disk. See documentation of the`IMPORTED_*`and`INTERFACE_*`properties for more information.

---

```
add_library(<name> OBJECT <src>...)
```

Creates a special “object library” target. An object library compiles source files but does not archive or link their object files into a library. Instead other targets created by[`add_library()`](https://cmake.org/cmake/help/v3.0/command/add_library.html#command:add_library)or[`add_executable()`](https://cmake.org/cmake/help/v3.0/command/add_executable.html#command:add_executable)may reference the objects using an expression of the form`$<TARGET_OBJECTS:objlib>`as a source, where`objlib`is the object library name. For example:

```
add_library(... $<TARGET_OBJECTS:objlib> ...)
add_executable(... $<TARGET_OBJECTS:objlib> ...)
```

will include objlib’s object files in a library and an executable along with those compiled from their own sources. Object libraries may contain only sources \(and headers\) that compile to object files. They may contain custom commands generating such sources, but not`PRE_BUILD`,`PRE_LINK`, or`POST_BUILD`commands. Object libraries cannot be imported, exported, installed, or linked. Some native build systems may not like targets that have only object files, so consider adding at least one real source file to any target that references`$<TARGET_OBJECTS:objlib>`.

---

```
add_library(<name> ALIAS <target>)
```

Creates an[Alias Target](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#alias-targets), such that`<name>`can be used to refer to`<target>`in subsequent commands. The`<name>`does not appear in the generatedbuildsystem as a make target. The`<target>`may not be an[Imported Target](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#imported-targets)or an`ALIAS`.`ALIAS`targets can be used as linkable targets and as targets to read properties from. They can also be tested for existance with the regular[`if(TARGET)`](https://cmake.org/cmake/help/v3.0/command/if.html#command:if)subcommand. The`<name>`may not be used to modify properties of`<target>`, that is, it may not be used as the operand of[`set_property()`](https://cmake.org/cmake/help/v3.0/command/set_property.html#command:set_property),[`set_target_properties()`](https://cmake.org/cmake/help/v3.0/command/set_target_properties.html#command:set_target_properties),[`target_link_libraries()`](https://cmake.org/cmake/help/v3.0/command/target_link_libraries.html#command:target_link_libraries)etc. An`ALIAS`target may not be installed or exported.

---

```
add_library(<name> INTERFACE [IMPORTED [GLOBAL]])
```

Creates an[Interface Library](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#interface-libraries). An`INTERFACE`library target does not directly create build output, though it may have properties set on it and it may be installed, exported and imported. Typically the`INTERFACE_*`properties are populated on the interface target using the[`set_property()`](https://cmake.org/cmake/help/v3.0/command/set_property.html#command:set_property),[`target_link_libraries(INTERFACE)`](https://cmake.org/cmake/help/v3.0/command/target_link_libraries.html#command:target_link_libraries),[`target_include_directories(INTERFACE)`](https://cmake.org/cmake/help/v3.0/command/target_include_directories.html#command:target_include_directories),[`target_compile_options(INTERFACE)`](https://cmake.org/cmake/help/v3.0/command/target_compile_options.html#command:target_compile_options)and[`target_compile_definitions(INTERFACE)`](https://cmake.org/cmake/help/v3.0/command/target_compile_definitions.html#command:target_compile_definitions)commands, and then it is used as an argument to[`target_link_libraries()`](https://cmake.org/cmake/help/v3.0/command/target_link_libraries.html#command:target_link_libraries)like any other target.

An`INTERFACE`[Imported Target](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#imported-targets)may also be created with this signature. An`IMPORTED`library target references a library defined outside the project. The target name has scope in the directory in which it is created and below, but the`GLOBAL`option extends visibility. It may be referenced like any target built within the project.`IMPORTED`libraries are useful for convenient reference from commands like[`target_link_libraries()`](https://cmake.org/cmake/help/v3.0/command/target_link_libraries.html#command:target_link_libraries).

