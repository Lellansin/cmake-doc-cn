# file

文件操作命令。

```cmake
file(WRITE filename "message to write"... )
file(APPEND filename "message to write"... )
file(READ filename variable [LIMIT numBytes] [OFFSET offset] [HEX])
file(<MD5|SHA1|SHA224|SHA256|SHA384|SHA512> filename variable)
file(STRINGS filename variable [LIMIT_COUNT num]
     [LIMIT_INPUT numBytes] [LIMIT_OUTPUT numBytes]
     [LENGTH_MINIMUM numBytes] [LENGTH_MAXIMUM numBytes]
     [NEWLINE_CONSUME] [REGEX regex]
     [NO_HEX_CONVERSION])
file(GLOB variable [RELATIVE path] [globbing expressions]...)
file(GLOB_RECURSE variable [RELATIVE path]
     [FOLLOW_SYMLINKS] [globbing expressions]...)
file(RENAME <oldname> <newname>)
file(REMOVE [file1 ...])
file(REMOVE_RECURSE [file1 ...])
file(MAKE_DIRECTORY [directory1 directory2 ...])
file(RELATIVE_PATH variable directory file)
file(TO_CMAKE_PATH path result)
file(TO_NATIVE_PATH path result)
file(DOWNLOAD url file [INACTIVITY_TIMEOUT timeout]
     [TIMEOUT timeout] [STATUS status] [LOG log] [SHOW_PROGRESS]
     [EXPECTED_HASH ALGO=value] [EXPECTED_MD5 sum]
     [TLS_VERIFY on|off] [TLS_CAINFO file])
file(UPLOAD filename url [INACTIVITY_TIMEOUT timeout]
     [TIMEOUT timeout] [STATUS status] [LOG log] [SHOW_PROGRESS])
file(TIMESTAMP filename variable [<format string>] [UTC])
file(GENERATE OUTPUT output_file
     <INPUT input_file|CONTENT input_content>
     [CONDITION expression])
```

**WRITE** 会将字符串写入一个名为 'filename' 的文件中。如果该文件已经存在，它将覆盖该文件，并在文件不存在时创建该文件。（如果文件是构建输入，则只有在文件内容发生更改时才使用 `configure_file` 来更新文件。）

**APPEND **与 **WRITE** 类似，会将字符串写入的文件，不过它会将其附加到文件末尾。

**READ** 会读取文件的内容并将其存储到变量中。它将从给定的偏移量开始并读取 numBytes 长度的内容。如果指定了 HEX 参数，则二进制数据将被转换为十六进制表示形式。

**MD5**，**SHA1**，**SHA224**，**SHA256**，**SHA384** 和 **SHA512** 将计算文件内容的加密散列（例如获取文件的 MD5）。

**STRINGS** 将从文件中解析 ASCII字符串并将其存储在一个变量中。文件中的二进制数据将被忽略。回车符（CR）字符被忽略。它也适用于 Intel Hex 和 Motorola S-record 文件，它们在读取时会自动转换为二进制格式。使用`NO_HEX_CONVERSION` 禁用此功能。

**LIMIT\_COUNT **设置要返回的最大字符串长度。**LIMIT\_INPUT **设置从输入文件读取的最大字节数。**LIMIT\_OUTPUT **设置要存储在输出变量中的最大字节数。**LENGTH\_MINIMUM **设置要返回的字符串的最小长度。较短的字符串被忽略。**LENGTH\_MAXIMUM **设置要返回的字符串的最大长度。更长的字符串被分成不超过最大长度的字符串。**NEWLINE\_CONSUME **允许换行符包含在字符串中，而不是终止它们。

**REGEX **指定一个字符串必须匹配才能返回的正则表达式。典型用法

```cmake
file(STRINGS myfile.txt myfile)
```

从 `myfile.txt` 中读取每一行的内容并存储到变量 “myfile” 中。

**GLOB **将生成一个与 globbing 表达式匹配的所有文件的列表，并将其存储到变量中。Globbing 表达式与正则表达式类似，但更简单。如果为表达式指定了 RELATIVE 标志，则结果将作为给定路径的相对路径返回。（我们不建议使用 GLOB 从源代码树中收集源文件列表，如果在添加或删除源代码时没有 CMakeLists.txt 文件更改，则生成的构建系统无法知道何时要求CMake重新生成）。

通配表达式的例子包括：

```cmake
*.cxx      - 匹配所有扩展名为 cxx 的文件
*.vt?      - 使用扩展名 vta、vtb、...、vtz 匹配所有文件
f[3-5].txt - 匹配文件 f3.txt，f4.txt，f5.txt
```

**GLOB\_RECURSE **会生成一个类似于常规 GLOB 的列表，除了它将遍历匹配目录的所有子目录并匹配这些文件。只有在给定FOLLOW\_SYMLINKS 或 cmake 策略 CMP0009 未设置为 NEW 时，才会遍历符号链接的子目录。有关更多信息，请参阅`cmake -help-policy CMP0009`。

递归通配的例子：

```
/dir/*.py  - match all python files in /dir and subdirectories
```

**MAKE\_DIRECTORY **可以创建给定的目录，同时适用于没有父目录的多级创建。

**RENAME** 移动文件系统中的文件或目录，以原子方式替换目标。

**REMOVE** 会删除给定的文件、目录。

**REMOVE\_RECURSE **将删除给定的文件和目录，也是非空目录。

**RELATIVE\_PATH **将确定从目录到给定文件的相对路径。

**TO\_CMAKE\_PATH **将使用 unix 的 `/` 将路径转换为 cmake 样式路径。输入可以是单个路径或系统路径，如 `“$ENV{PATH}”`。请注意，ENV 调用 **TO\_CMAKE\_PATH** 的双引号只有一个参数。该命令还会将本机列表分隔符转换为路径列表，如 PATH 环境变量。

TO\_NATIVE\_PATH works just like TO\_CMAKE\_PATH, but will convert from a cmake style path into the native path style for windows and / for UNIX.

DOWNLOAD will download the given URL to the given file. If LOG var is specified a log of the download will be put in var. If STATUS var is specified the status of the operation will be put in var. The status is returned in a list of length 2. The first element is the numeric return value for the operation, and the second element is a string value for the error. A 0 numeric error means no error in the operation. If TIMEOUT time is specified, the operation will timeout after time seconds, time should be specified as an integer. The INACTIVITY\_TIMEOUT specifies an integer number of seconds of inactivity after which the operation should terminate. If EXPECTED\_HASH ALGO=value is specified, the operation will verify that the downloaded file’s actual hash matches the expected value, where ALGO is one of MD5, SHA1, SHA224, SHA256, SHA384, or SHA512. If it does not match, the operation fails with an error. \(“EXPECTED\_MD5 sum” is short-hand for “EXPECTED\_HASH MD5=sum”.\) If SHOW\_PROGRESS is specified, progress information will be printed as status messages until the operation is complete. For https URLs CMake must be built with OpenSSL. TLS/SSL certificates are not checked by default. Set TLS\_VERIFY to ON to check certificates and/or use EXPECTED\_HASH to verify downloaded content. Set TLS\_CAINFO to specify a custom Certificate Authority file. If either TLS option is not given CMake will check variables CMAKE\_TLS\_VERIFY and CMAKE\_TLS\_CAINFO, respectively.

UPLOAD will upload the given file to the given URL. If LOG var is specified a log of the upload will be put in var. If STATUS var is specified the status of the operation will be put in var. The status is returned in a list of length 2. The first element is the numeric return value for the operation, and the second element is a string value for the error. A 0 numeric error means no error in the operation. If TIMEOUT time is specified, the operation will timeout after time seconds, time should be specified as an integer. The INACTIVITY\_TIMEOUT specifies an integer number of seconds of inactivity after which the operation should terminate. If SHOW\_PROGRESS is specified, progress information will be printed as status messages until the operation is complete.

TIMESTAMP will write a string representation of the modification time of filename to variable.

Should the command be unable to obtain a timestamp variable will be set to the empty string “”.

See documentation of the string TIMESTAMP sub-command for more details.

The file\(\) command also provides COPY and INSTALL signatures:

```
file(<COPY|INSTALL> files... DESTINATION <dir>
     [FILE_PERMISSIONS permissions...]
     [DIRECTORY_PERMISSIONS permissions...]
     [NO_SOURCE_PERMISSIONS] [USE_SOURCE_PERMISSIONS]
     [FILES_MATCHING]
     [[PATTERN <pattern> | REGEX <regex>]
      [EXCLUDE] [PERMISSIONS permissions...]] [...])
```

The COPY signature copies files, directories, and symlinks to a destination folder. Relative input paths are evaluated with respect to the current source directory, and a relative destination is evaluated with respect to the current build directory. Copying preserves input file timestamps, and optimizes out a file if it exists at the destination with the same timestamp. Copying preserves input permissions unless explicit permissions or NO\_SOURCE\_PERMISSIONS are given \(default is USE\_SOURCE\_PERMISSIONS\). See the install\(DIRECTORY\) command for documentation of permissions, PATTERN, REGEX, and EXCLUDE options.

The INSTALL signature differs slightly from COPY: it prints status messages, and NO\_SOURCE\_PERMISSIONS is default. Installation scripts generated by the install\(\) command use this signature \(with some undocumented options for internal use\).

GENERATE will write an &lt;output\_file&gt; with content from an &lt;input\_file&gt;, or from &lt;input\_content&gt;. The output is generated conditionally based on the content of the &lt;condition&gt;. The file is written at CMake generate-time and the input may contain generator expressions. The &lt;condition&gt;, &lt;output\_file&gt; and &lt;input\_file&gt; may also contain generator expressions. The &lt;condition&gt; must evaluate to either ‘0’ or ‘1’. The &lt;output\_file&gt; must evaluate to a unique name among all configurations and among all invocations of file\(GENERATE\).



