## Creating a new project

The commands below are for PowerShell, and will need to be adjusted
slightly if you're using Command Prompt instead.

1.  Copy pico_sdk_import.cmake from the SDK into your project directory:

    ``` powershell
    copy ${env:PICO_SDK_PATH}\external\pico_sdk_import.cmake .
    ```

2.  Copy VS Code configuration from the SDK examples into your project
    directory:

    ``` powershell
    copy ${env:PICO_EXAMPLES_PATH}\.vscode .
    ```

3.  Setup a `CMakeLists.txt` like:

    ``` cmake
    cmake_minimum_required(VERSION 3.13)

    # initialize the SDK based on PICO_SDK_PATH
    # note: this must happen before project()
    include(pico_sdk_import.cmake)

    project(my_project)

    # initialize the Raspberry Pi Pico SDK
    pico_sdk_init()

    # rest of your project
    ```

4.  Write your code (see
    [pico-examples](https://github.com/raspberrypi/pico-examples) or the
    [Raspberry Pi Pico C/C++ SDK](https://rptl.io/pico-c-sdk)
    documentation for more information)

    About the simplest you can do is a single source file (e.g.
    hello_world.c)

    ``` c
    #include <stdio.h>
    #include "pico/stdlib.h"

    int main() {
        setup_default_uart();
        printf("Hello, world!\n");
        return 0;
    }
    ```

    And add the following to your `CMakeLists.txt`:

    ``` cmake
    add_executable(hello_world
        hello_world.c
    )

    # Add pico_stdlib library which aggregates commonly used features
    target_link_libraries(hello_world pico_stdlib)

    # create map/bin/hex/uf2 file in addition to ELF.
    pico_add_extra_outputs(hello_world)
    ```

    Note this example uses the default UART for *stdout*; if you want to
    use the default USB see the
    [hello-usb](https://github.com/raspberrypi/pico-examples/tree/master/hello_world/usb)
    example.

5.  Launch VS Code from the *Pico - Visual Studio Code* shortcut in the
    Start Menu, and then open your new project folder.

6.  Configure the project by running the *CMake: Configure* command from
    VS Code's command palette.

7.  Build and debug the project as described in previous sections.
