# adb简介

adb (Android Debug Bridge)——Android调试桥，一种可以使开发者和设备进行通信的命令行工具。adb命令可以用于执行各种设备操作，并提供对Unix shell的访问权限（开发者可以通过Unix shell在设备上运行各种命令）。 

adb是一种C/S架构的程序，包括以下三个组件：

* **客户端**：运行于开发机上，用于发送命令。开发者可以通过发出adb命令来调用客户端。
* **守护程序(adbd)**：在每台设备上作为后台进程运行，用于在设备上运行命令。
* **服务器**：在开发机器上作为后台进程运行，用于管理客户端与守护进程之间的通信。

# adb的工作原理

当adb客户端启动时，该客户端首先会检查是否有adb服务器在运行，如果没有，它会启动一个服务器进程。

服务器启动后会与本地TCP端口5037绑定，并通过此端口来监听adb客户端发出的命令（所有adb客户端均通过端口5037与adb服务器通信）。

然后服务器会通过扫描5555到5585之间的奇数号端口来与设备建立联系。每个模拟器都会使用一对按顺序排列的端口（偶数号端口用于和控制台连接，奇数号端口用于和adb连接）。服务器一旦发现adbd，便会与相应的端口建立连接。

在服务器与设备建立联系后，开发者便可以通过使用adb命令来访问这些设备（可以从任意客户端访问任意设备）。



# 在设备上启用adb调试

开发者可以通过以下三种方式连接设备进行adb调试：

1. 在模拟器上进行adb调试。
2. 通过USB连接对硬件进行adb调试。
3. 通过WLAN连接对硬件进行adb调试。

参考链接：[在硬件设备上运行应用](https://developer.android.google.cn/studio/run/device?hl=zh_cn)

设备连接后，可以通过从``android_sdk\platform-tools\``目录执行``adb devices``命令来验证设备是否已连接。



# 常用的adb命令

1. **查询设备**：``adb devices -l``

   调用该命令后，adb会输出设备的以下状态信息：

   1. 序列号：由abd创建的字符串，通过端口号唯一标识设备。

   2. 状态：设备的连接状态
      1. offline：设备未连接或没有响应
      2. device：设备已连接到adb服务器。
      3. no device：未连接任何设备
   3. 说明：如果上述命令包含``-l``，该命令会在结果中附以简要的设备信息，以供开发者在多台已连接的设备之间作区分。

2. **发送命令至特定的设备**：``adb -s  serial_number  command`` 

   例如：adb -s emulator-5555 install helloWorld.apk

   ps:使用 -s 和 serial_number来指定设备。

3. **安装应用**：``adb install path_to_apk``

4. **设置端口转发**：使用``forward``命令设置任意端口转发，将特定主机上的请求转发到设备上的其他端口。

   例如：adb forward tcp:6100 tcp:7100

   上述命令设置了主机端口6100到设备端口7100的转发。

5. **将文件复制到设备/从设备复制文件**：

   * ``pull``: 将文件复制到设备 => ``adb pull remote local``
   * ``push``: 从设备复制文件 => ``adb push local remote``

   将local和remote分别替换为开发机器（本地）和设备（远程）上的目标文件/目录 的路径即可。

6. **停止adb服务器**：``adb kill -server``

7. **发出adb命令**：``adb [-d | -e | -s serial_number] command``

   如果只有一个模拟器在运行或者只连接了一个设备，系统会默认将 adb 命令发送至该设备。如果有多个模拟器正在运行并且/或者连接了多个设备，您需要使用 `-d`、`-e` 或 `-s` 选项指定应向其发送命令的目标设备。

8. **发出shell命令**：

   您可以使用 `shell` 命令通过 adb 发出设备命令，也可以启动交互式 shell。如需发出单个命令，请使用 `shell` 命令，如下所示：

   ```
   adb [-d |-e | -s serial_number] shell shell_command
   ```

   要在设备上启动交互式 shell，请使用 `shell` 命令，如下所示：

   ```
   adb [-d | -e | -s serial_number] shell
   ```

   要退出交互式 shell，请按 Ctrl + D 键或输入 `exit`。

9. **调用Activity管理器(am)**：

   在 adb shell 中，您可以使用 Activity 管理器 (`am`) 工具发出命令以执行各种系统操作，如启动 Activity、强行停止进程、广播 intent、修改设备屏幕属性，等等。在 shell 中，相应的语法为：

   ```
   am command
   ```

   您也可以直接从 adb 发出 Activity 管理器命令，无需进入远程 shell。例如：

   ```
   adb shell am start -a android.intent.action.VIEW
   ```

   **表 2.** 可用的 Activity 管理器命令

   | 命令                                  | 说明                                                         |
   | :------------------------------------ | :----------------------------------------------------------- |
   | `start [options] intent`              | 启动由 `intent` 指定的 `Activity`。请参阅 [intent 参数的规范](https://developer.android.google.cn/studio/command-line/adb#IntentSpec)。具体选项包括：`-D`：启用调试功能。`-W`：等待启动完成。`--start-profiler file`：启动性能剖析器并将结果发送至 `file`。`-P file`：类似于 `--start-profiler`，但当应用进入空闲状态时剖析停止。`-R count`：重复启动 Activity `count` 次。在每次重复前，将完成顶层 Activity。`-S`：在启动 Activity 前，强行停止目标应用。`--opengl-trace`：启用 OpenGL 函数的跟踪。`--user user_id | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行。 |
   | `startservice [options] intent`       | 启动由 `intent` 指定的 `Service`。请参阅 [intent 参数的规范](https://developer.android.google.cn/studio/command-line/adb#IntentSpec)。具体选项包括：`--user user_id | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行。 |
   | `force-stop package`                  | 强行停止与 `package`（应用的软件包名称）关联的所有进程。     |
   | `kill [options] package`              | 终止与 `package`（应用的软件包名称）关联的所有进程。此命令仅终止可安全终止且不会影响用户体验的进程。具体选项包括：`--user user_id | all | current`：指定要终止哪个用户的进程；如果未指定，则终止所有用户的进程。 |
   | `kill-all`                            | 终止所有后台进程。                                           |
   | `broadcast [options] intent`          | 发出广播 intent。请参阅 [intent 参数的规范](https://developer.android.google.cn/studio/command-line/adb#IntentSpec)。具体选项包括：`[--user user_id | all | current]`：指定要发送给哪个用户；如果未指定，则发送给所有用户。 |
   | `instrument [options] component`      | 使用 `Instrumentation` 实例启动监控。通常情况下，目标 `component` 采用 `test_package/runner_class` 格式。具体选项包括：`-r`：输出原始结果（否则，对 `report_key_streamresult` 进行解码）。与 `[-e perf true]` 结合使用可生成性能测量的原始输出。`-e name value`：将参数 `name` 设为 `value`。 对于测试运行程序，通用格式为 `-e testrunner_flag value[,value...]`。`-p file`：将剖析数据写入 `file`。`-w`：等待插桩完成后再返回。测试运行程序需要使用此选项。`--no-window-animation`：运行时关闭窗口动画。`--user user_id | current`：指定以哪个用户身份运行插桩；如果未指定，则以当前用户身份运行。 |
   | `profile start process file`          | 启动 `process` 的性能剖析器，将结果写入 `file`。             |
   | `profile stop process`                | 停止 `process` 的性能剖析器。                                |
   | `dumpheap [options] process file`     | 转储 `process` 的堆，写入 `file`。具体选项包括：`--user [user_id | current]`：提供进程名称时，指定要转储的进程的用户；如果未指定，则使用当前用户。`-n`：转储原生堆，而非托管堆。 |
   | `set-debug-app [options] package`     | 设置要调试的应用 `package`。具体选项包括：`-w`：应用启动时等待调试程序。`--persistent`：保留此值。 |
   | `clear-debug-app`                     | 清除之前使用 `set-debug-app` 设置的待调试软件包。            |
   | `monitor [options]`                   | 开始监控崩溃或 ANR。具体选项包括：`--gdb`：在崩溃/ANR 时，在给定的端口上启动 gdbserv。 |
   | `screen-compat {on | off} package`    | 控制 `package` 的[屏幕兼容性](https://developer.android.google.cn/guide/practices/screen-compat-mode)模式。 |
   | `display-size [reset | widthxheight]` | 替换设备显示尺寸。此命令支持使用大屏设备模仿小屏幕分辨率（反之亦然），对于在不同尺寸的屏幕上测试应用非常有用。示例： `am display-size 1280x800` |
   | `display-density dpi`                 | 替换设备显示密度。此命令支持使用低密度屏幕在高密度屏幕环境上进行测试（反之亦然），对于在不同密度的屏幕上测试应用非常有用。示例： `am display-density 480` |
   | `to-uri intent`                       | 以 URI 的形式输出给定的 intent 规范。请参阅 [intent 参数的规范](https://developer.android.google.cn/studio/command-line/adb#IntentSpec)。 |
   | `to-intent-uri intent`                | 以 `intent:` URI 的形式输出给定的 intent 规范。请参阅 [intent 参数的规范](https://developer.android.google.cn/studio/command-line/adb#IntentSpec)。 |

   #### intent 参数的规范

   对于采用 `intent` 参数的 Activity 管理器命令，您可以使用以下选项指定 intent：

   全部显示

   ### 调用软件包管理器 (`pm`)

   在 adb shell 中，您可以使用软件包管理器 (`pm`) 工具发出命令，以对设备上安装的应用软件包执行操作和查询。在 shell 中，相应的语法为：

   ```
   pm command
   ```

   您也可以直接从 adb 发出软件包管理器命令，无需进入远程 shell。例如：

   ```
   adb shell pm uninstall com.example.MyApp
   ```

   **表 3.** 可用的软件包管理器命令。

   | 命令                                                | 说明                                                         |
   | :-------------------------------------------------- | :----------------------------------------------------------- |
   | `list packages [options] filter`                    | 输出所有软件包，或者，仅输出软件包名称包含 `filter` 中的文本的软件包。具体选项：`-f`：查看它们的关联文件。`-d`：进行过滤以仅显示已停用的软件包。`-e`：进行过滤以仅显示已启用的软件包。`-s`：进行过滤以仅显示系统软件包。`-3`：进行过滤以仅显示第三方软件包。`-i`：查看软件包的安装程序。`-u`：也包括已卸载的软件包。`--user user_id`：要查询的用户空间。 |
   | `list permission-groups`                            | 输出所有已知的权限组。                                       |
   | `list permissions [options] group`                  | 输出所有已知的权限，或者，仅输出 `group` 中的权限。具体选项：`-g`：按组进行整理。`-f`：输出所有信息。`-s`：简短摘要。`-d`：仅列出危险权限。`-u`：仅列出用户将看到的权限。 |
   | `list instrumentation [options]`                    | 列出所有测试软件包。具体选项：`-f`：列出测试软件包的 APK 文件。`target_package`：仅列出此应用的测试软件包。 |
   | `list features`                                     | 输出系统的所有功能。                                         |
   | `list libraries`                                    | 输出当前设备支持的所有库。                                   |
   | `list users`                                        | 输出系统中的所有用户。                                       |
   | `path package`                                      | 输出给定 `package` 的 APK 的路径。                           |
   | `install [options] path`                            | 将软件包（通过 `path` 指定）安装到系统。具体选项：`-r`：重新安装现有应用，并保留其数据。`-t`：允许安装测试 APK。仅当您运行或调试了应用或者使用了 Android Studio 的 **Build > Build APK** 命令时，Gradle 才会生成测试 APK。如果是使用开发者预览版 SDK（如果 `targetSdkVersion` 是字母，而非数字）构建的 APK，那么安装测试 APK 时必须在 `install` 命令中包含 [`-t` 选项](https://developer.android.google.cn/studio/command-line/adb#-t-option)。`-i installer_package_name`：指定安装程序软件包名称。`--install-location location`：使用以下某个值设置安装位置：`0`：使用默认安装位置。`1`：在内部设备存储上安装。`2`：在外部介质上安装。`-f`：在内部系统内存上安装软件包。`-d`：允许版本代码降级。`-g`：授予应用清单中列出的所有权限。`--fastdeploy`：通过仅更新已更改的 APK 部分来快速更新安装的软件包。`--incremental`：仅安装 APK 中启动应用所需的部分，同时在后台流式传输剩余数据。如要使用此功能，您必须为 APK 签名，创建一个 [APK 签名方案 v4 文件](https://developer.android.google.cn/studio/command-line/apksigner#v4-signing-enabled)，并将此文件放在 APK 所在的目录中。只有部分设备支持此功能。此选项会强制 adb 使用该功能，如果该功能不受支持，则会失败（并提供有关失败原因的详细信息）。附加 `--wait` 选项，可等到 APK 完全安装完毕后再授予对 APK 的访问权限。`--no-incremental` 可阻止 adb 使用此功能。 |
   | `uninstall [options] package`                       | 从系统中移除软件包。具体选项：`-k`：移除软件包后保留数据和缓存目录。 |
   | `clear package`                                     | 删除与软件包关联的所有数据。                                 |
   | `enable package_or_component`                       | 启用给定的软件包或组件（写为“package/class”）。              |
   | `disable package_or_component`                      | 停用给定的软件包或组件（写为“package/class”）。              |
   | `disable-user [options] package_or_component`       | 具体选项：`--user user_id`：要停用的用户。                   |
   | `grant package_name permission`                     | 向应用授予权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限。 |
   | `revoke package_name permission`                    | 从应用撤消权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限。 |
   | `set-install-location location`                     | 更改默认安装位置。位置值如下：`0`：自动：让系统决定最合适的位置。`1`：内部：在内部设备存储上安装。`2`：外部：在外部介质上安装。**注意**：此命令仅用于调试目的；使用此命令可能会导致应用中断和其他意外行为。 |
   | `get-install-location`                              | 返回当前安装位置。返回值如下：`0 [auto]`：让系统决定最合适的位置`1 [internal]`：在内部设备存储上安装`2 [external]`：在外部介质上安装 |
   | `set-permission-enforced permission [true | false]` | 指定是否应强制执行指定权限。                                 |
   | `trim-caches desired_free_space`                    | 减少缓存文件以达到给定的可用空间。                           |
   | `create-user user_name`                             | 创建具有给定 `user_name` 的新用户，从而输出该用户的新用户标识符。 |
   | `remove-user user_id`                               | 移除具有给定 `user_id` 的用户，从而删除与该用户关联的所有数据。 |
   | `get-max-users`                                     | 输出设备支持的最大用户数。                                   |

10. 其余命令不再赘述，可查看：https://developer.android.google.cn/studio/command-line/adb

    

    

    

    

    

    

    

    

    

    

    





















