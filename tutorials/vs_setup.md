# Setup BParser with Visual Studio on Windows

To be able to compile BParser in Visual Studio do:
1. Prepare Visual Studio
     1. Install the following workloads for Visual Studio. If you are unsure how, see the [modify Visual Studio](https://learn.microsoft.com/en-us/visualstudio/install/modify-visual-studio?view=visualstudio) tutorial. 
        * Desktop Development with C++
        * Linux and embedded development with C++
     2. Install the following components for Visual Studio. 
        * C++ Clang compiler for Windows (xx.x.x)
        * C++ CMake tools for Windows
        * C++ CMake tools for Linux
2. Install Boost libraries
   1. Open the [Developer Command Prompt](https://learn.microsoft.com/en-us/visualstudio/ide/reference/command-prompt-powershell?view=visualstudio) provided by Visual Studio
   2. Continue using the [Boost tutorial](https://www.boost.org/doc/user-guide/getting-started.html)
3. Clone the BParser repository
   1. Open a console in a folder where you wish to clone BParser
   2. Open [https://github.com/flow123d/bparser/tree/master](https://github.com/flow123d/bparser/tree/master) in GitHub
   3. Press the green Code button near the top of the page
   4. Choose a clone method and run the displayed command in your opened console. The HTTPS method is recommended.
4. Open BParser folder in Visual Studio
   1. Open Visual Studio
   2. Click Open folder...
   3. In the dialog choose the folder with the cloned BParser repository.  
   Visual studio will open the folder and attempt to generate CMake cache, but fail
5. Configure the project
   1. Add the Clang configuration
      1. In Visual Studio in the top menu next to the green Debug button click the dropdown with text x64-Debug
      2. Select Manage configurations...  
      A CMake Settings window will open
      3. On the left side, click the green + button
      4. Select x64-Clang-Debug  
      A new configuration will appear in the list
   2. Edit the Clang configuration
      1. In the CMake settings, scroll down to "CMake Variables and cache" and tick on Show advanced variables
      2. Edit the following entries in the list:
           * Find SANITIZER_ON and turn it off. This setting controls the AddressSanitizer and may cause problems at this stage of setup.
           * Find CMAKE_C_COMPILER and edit it from `(...)/Llvm/x64/bin/clang-cl.exe` to `(...)/Llvm/x64/bin/clang.exe`
           * Find CMAKE_CXX_COMPILER and edit it from `(...)/Llvm/x64/bin/clang-cl.exe` to `(...)/Llvm/x64/bin/clang++.exe`
      3. Click on Edit JSON  
      A JSON text file editor will replace the CMake Settings window
      4. In the configuration with the name `x64-Clang-Debug` in the `variables` list you see results from the previous steps.
      5. Add another entry into the list specifying where the Boost libraries are installed
      ```json
      {
           "name": "BOOST_ROOT",
           "value": "C:\\boost",
           "type": "PATH"
      },
      ```
      6. The list should look similar to this.  
      The order of the list entries does not matter. You paths may be different
      ```json
      "variables": [
            {
              "name": "CMAKE_C_COMPILER",
              "value": "C:/PROGRAM FILES/MICROSOFT VISUAL STUDIO/18/COMMUNITY/VC/Tools/Llvm/x64/bin/clang.exe",
              "type": "STRING"
            },
            {
              "name": "CMAKE_CXX_COMPILER",
              "value": "C:/PROGRAM FILES/MICROSOFT VISUAL STUDIO/18/COMMUNITY/VC/Tools/Llvm/x64/bin/clang++.exe",
              "type": "STRING"
            },
            {
              "name": "BOOST_ROOT",
              "value": "C:\\boost",
              "type": "PATH"
            },
            {
              "name": "SANITIZER_ON",
              "value": "False",
              "type": "BOOL"
            }
       ]
      ```
   3. For the Release configuration, repeat step 5. with the x64-Clang-Release configuration
6. Start a test
   1. In Visual Studio in the top menu open the dropdown next to the green Debug triangle and select `test_parser_bin.exe`
   2. Click the green Debug triangle  
   Visual Studio will compile everything necessary and start the Parser test
   3. If the Parser test completes successfully you are good to go
