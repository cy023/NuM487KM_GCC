# Debug Usage Tutorial
##  Requirment
### Tools
* [OpenOCD_Nuvoton](https://github.com/OpenNuvoton/OpenOCD-Nuvoton)
* arm-none-eabi-gcc
* cmake(optional)
* NuLink Command Tools(optional)
* `M481_v1.svd` from [Nuvoton_DFP](https://github.com/OpenNuvoton/cmsis-packs)
### VScode Extensions
* C/C++
* Cortex-Debug(must use v1.2.2)
## Before usage
1. install
2. Edit following excutable path
   * `c_cpp_properties.json`: compilerPath
   * `launch.json`
     * executable
     * serverpath
     * armToolchainPath
   * `arm-none-eabi-gcc.cmake`: ARM_TOOLCHAIN_DIR
   * CMakeLists.txt: project 
   * flash.cmd: NuLink.exe
   
        **flash command is WINDOWS ONLY**

## Usage
1. Build
2. Click `Run and Debug` button from the left side of vscode window.
3. Choose launch.json of current porject.
4. Do one of the following:
   * Click `Start Debugging` button (the green one).
   * Press `F5`.
5. Enjoy Debugging :cry:.