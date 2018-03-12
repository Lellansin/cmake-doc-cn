# cmake-commands\(7\)

## 内容

* cmake-commands\(7\)
  * 普通命令
  * 废弃命令
  * CTest 命令

## 普通命令

你可以自由的在 CMake 项目中使用下列命令。

* add\_compile\_options
* add\_custom\_command
* add\_custom\_target
* [add\_definitions](/commands/add_definitions.md)
* add\_dependencies
* add\_executable
* [add\_library](/commands/add_library.md)
* add\_subdirectory
* add\_test
* aux\_source\_directory
* [break](/break)
* build\_command
* cmake\_host\_system\_information
* cmake\_minimum\_required
* cmake\_policy
* configure\_file
* create\_test\_sourcelist
* define\_property
* elseif
* else
* enable\_language
* enable\_testing
* endforeach
* endfunction
* endif
* endmacro
* endwhile
* execute\_process
* export
* [file](/commands/file.md)
* find\_file
* find\_library
* find\_package
* find\_path
* find\_program
* fltk\_wrap\_ui
* foreach
* function
* get\_cmake\_property
* get\_directory\_property
* get\_filename\_component
* get\_property
* get\_source\_file\_property
* get\_target\_property
* get\_test\_property
* if
* include\_directories
* include\_external\_msproject
* include\_regular\_expression
* include
* install
* link\_directories
* list
* load\_cache
* load\_command
* macro
* [mark\_as\_advanced](/commands/mark_as_advanced.md)
* math
* message
* option
* project
* qt\_wrap\_cpp
* qt\_wrap\_ui
* remove\_definitions
* return
* separate\_arguments
* set\_directory\_properties
* set\_property
* set
* set\_source\_files\_properties
* set\_target\_properties
* set\_tests\_properties
* site\_name
* source\_group
* string
* target\_compile\_definitions
* target\_compile\_options
* target\_include\_directories
* target\_link\_libraries
* try\_compile
* try\_run
* unset
* variable\_watch
* while

## 废弃命令

以下命令仅在旧版 CMake 上进行兼容使用。请勿在新代码中使用。

* build\_name
* exec\_program
* export\_library\_dependencies
* install\_files
* install\_programs
* install\_targets
* link\_libraries
* make\_directory
* output\_required\_files
* remove
* subdir\_depends
* subdirs
* use\_mangled\_mesa
* utility\_source
* variable\_requires
* write\_file

## CTest 命令

已下命令仅在 ctest 脚本中可用.

* ctest\_build
* ctest\_configure
* ctest\_coverage
* ctest\_empty\_binary\_directory
* ctest\_memcheck
* ctest\_read\_custom\_files
* ctest\_run\_script
* ctest\_sleep
* ctest\_start
* ctest\_submit
* ctest\_test
* ctest\_update
* ctest\_upload



