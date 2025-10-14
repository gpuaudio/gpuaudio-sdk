# Installataion Guides

## Windows

### Supported Windows versions

* Windows 11 (any)

### Generic prerequisites

* Visual Studio 2022 with `MSVC v143 - VS 2022 C++ x64/x86 build tools v14.34-17.4` toolchain (should be installed via _Individual Components_ in visual studio installer)
* cmake 3.26.3+
* git
* [GPU Audio Platform](https://www.gpu.audio/sdk-binaries)

### Nvidia-specific prerequisites

* [CUDA 12.9](https://developer.nvidia.com/cuda-12-9-0-download-archive)
* Graphics driver >= 576.2

### AMD-specific prerequisites

* [HIP 6.4.2](https://www.amd.com/en/developer/resources/rocm-hub/hip-sdk.html)
* Perl interpreter. [StrawberryPerl](https://strawberryperl.com/)
* Graphics driver >= 25.10

### Prepare build environment

* Clone this repository
* Initialize submodules `git submodule update --init --recursive`

### Build targeting Nvidia devices
* Configure with `cmake -S . -B @BUILD -G "Visual Studio 17 2022" -DWITH_HIP:BOOL=false -DCUDA_VERSION="12.9"`
* Build with `cmake --build @BUILD --config RelWithDebInfo --parallel`

### Build targeting AMD devices
* Set `PERL_PATH` to the location of your perl interpreter. In case of StrawberryPerl it would be `C:\Strawberry\perl\bin` by default (this step can be skipped if only Nvidia binaries are being built)
* Configure with `cmake -S . -B @BUILD -G "Visual Studio 17 2022" -DWITH_CUDA:BOOL=false`
* Build with `cmake --build @BUILD --config RelWithDebInfo --parallel`

### Run tests

* Add `path_to_gpuaudio_sdk/@BUILD/bin/RelWithDebInfo` to the `GPUAUDIO_PROCESSOR_PATH` environment variable
* Run tests with `ctest --test-dir @BUILD --build-config RelWithDebInfo`

### NeuralAmpModeler deployment

Be default GPU Audio SDK doesn't have any side effects outside of the repository bounds. Automatic deployment of NeuralAmpModeler VST3 into the location expected by the DAWs can be enabled by setting `-DVST3_DEPLOY=1` at the configuration time. For this to work `C:\Program Files\Common Files\VST3` should exist and be writable by the current user

Alternative is managing deployment of VST3 manually, it can be found in `./@BUILD/out/NeuralAmpModeler.vst3`

### Use NeuralAmpModeler with GPU acceleration in the DAW

* Ensure that `GPUAUDIO_PROCESSOR_PATH` contains the path to built `nam_processor` DLLs
* Ensure that VST3 is deployed to `C:\Program Files\Common Files\VST3` by the method of your choosing
* Run the DAW of your choosing and proceed as usually

Models from this [repo](https://github.com/pelennor2170/NAM_models) may be used to quickly test the functionality and performance

## macOS

Supported macOS versions:

* macOS 13
* macOS 14
* macOS 15
* macOS 26

### Prerequisites

* Xcode
* cmake 3.26.3+
* GPU Audio Platform

### Xcode version management

In order to build binaries for your local macOS version the appropriate version of Xcode must be used to match target SDK and clang versions. The listing below is not complete, those are combinations that were tested and are known to be working

| OS                | Xcode version | Target | Clang |
| :---------------- | :------:      | ----:  | :-    |
| macOS 13          |   14.2        | 13.1   | 14    |
| macOS 14          |   15.0.1      | 14     | 15    |
| macOS 15          |   16.0        | 15     | 16    |
| macOS 26          |   26.0.1      | 26     | 17    |

There are many ways managing Xcode versions, recommended way is using [xcodes](https://github.com/XcodesOrg/xcodes)

```
xcodes install 14.2
xcodes select 14.2
```

### Build

* Clone this repository
* Initialize submodules `git submodule update --init --recursive`
* Configure with `cmake -S . -B @BUILD -G Xcode`
* Build with `cmake --build @BUILD --config RelWithDebInfo --parallel`

### Run tests

* Add `path_to_gpuaudio_sdk/@BUILD/bin/RelWithDebInfo` to the `GPUAUDIO_PROCESSOR_PATH` environment variable
* Run tests with `ctest --test-dir @BUILD --build-config RelWithDebInfo`

### NeuralAmpModeler deployment

Be default GPU Audio SDK doesn't have any side effects outside of the repository bounds. Automatic deployment of NeuralAmpModeler VST3 and/or AUv2 into the location expected by the DAWs can be enabled by setting `-DVST3_DEPLOY=1` and `-DAUv2_DEPLOY=1` at the configuration time.

### Use NeuralAmpModeler with GPU acceleration in the DAW

* Ensure that `GPUAUDIO_PROCESSOR_PATH` contains the path to built `nam_processor` dylibs. To achieve this you may use `launchctl setenv GPUAUDIO_PROCESSOR_PATH path_to_sdk/@BUILD/bin/RelWithDebInfo` and restart the DAW
* Ensure that VST3/AUv2 are deployed to `~/Library/Audio/Plug-Ins/Components` and `~/Library/Audio/Plug-Ins/VST3` by the method of your choosing
* Run the DAW of your choosing and proceed as usually

Models from this [repo](https://github.com/pelennor2170/NAM_models) may be used to quickly test the functionality and performance
