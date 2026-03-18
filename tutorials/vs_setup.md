# Setup BParser with Visual Studio on Windows

1. Prepare Visual Studio
     1. Install the following workloads for Visual Studio. [Here](https://learn.microsoft.com/en-us/visualstudio/install/modify-visual-studio?view=visualstudio) is how. 
        * Desktop Development with C++
        * Linux and embedded development with C++
     2. Install the following components for Visual Studio. 
        * C++ Clang compiler for Windows (xx.x.x)
        * C++ CMake tools for Windows
        * C++ CMake tools for Linux
2. Install Boost libraries
   1. Open [Developer Command Prompt](https://learn.microsoft.com/en-us/visualstudio/ide/reference/command-prompt-powershell?view=visualstudio)
   2. Continue using the [Boost tutorial](https://www.boost.org/doc/user-guide/getting-started.html)
3. Clone the BParser repository
   1. Open a console in a folder where you wish to clone BParser
   2. Open [https://github.com/flow123d/bparser/tree/master](https://github.com/flow123d/bparser/tree/master) in GitHub
   3. Press the green Code button
   4. Choose a clone method and run the displayed command in your opened console
4. Open BParser folder in Visual Studio
   1. Open Visual Studio
   2. Click Open folder...
   3. In the dialog choose the folder with the cloned BParser repository
   Visual studio will open the folder and attempt to generate CMake cache, but fail
5. Configure the project
   1. Add the Clang configuration
      1. In Visual Studio in the top menu click x64-Debug
      2. Select Manage configurations...
      3. On the left side, click the green + button
      4. Select x64-Clang-Debug
   2. change compiler, add BOOST_ROOT 
