# EPIC DIRC

## Setting up the framework
* ```curl --location https://get.epic-eic.org | bash```

  The above command gave the following error:
  ```ERROR: no singularity found, please make sure you have singularity in your $PATH```
  Full output after the command:
  ```
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                   Dload  Upload   Total   Spent    Left  Speed
    0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
  100 14134  100 14134    0     0   6123      0  0:00:02  0:00:02 --:--:--  6123
  ~/G4WORK/EIC/EICDIRC/epicdirc ~/G4WORK/EIC/EICDIRC/epicdirc
  Setting up development environment for eicweb/eic_xl:nightly
   - Detected OS: Linux
   - Detected CPU: x86_64
  ERROR: no singularity found, please make sure you have singularity in your $PATH
  ```

  Installed singularity using ```sudo apt-get install singularity```

  After installing singularity the previous error was gone. 

  Now facing the following error:
  ```
  Destination: /tmp/ci-2EtXRfe0up/lib/eic_xl-nightly.sif
  WARNING: failed to retrieve container artifact
  Attempting alternative download from docker registry
  Executing: singularity pull --force /tmp/ci-2EtXRfe0up/lib/eic_xl-nightly.sif docker://eicweb/eic_xl:nightly
  Singularity 1.0b1 (commit: b0d3406e311f4ac3aa6f4d0187f32926b3808fef)
  Running under Python 3.8.10 (default, Mar 18 2025, 20:04:55) [GCC 9.4.0]
  pygame 1.9.6
  Hello from the pygame community. https://www.pygame.org/contribute.html
  The error-log configured as /home/shubhamdutta/.local/share/singularity/log/error.log (lazily created when something is logged)
  Usage: singularity [options]

  singularity: error: no such option: --force
  Traceback (most recent call last):
    File "/usr/lib/python3.8/urllib/request.py", line 1354, in do_open
      h.request(req.get_method(), req.selector, req.data, headers,
    File "/usr/lib/python3.8/http/client.py", line 1256, in request
      self._send_request(method, url, body, headers, encode_chunked)
    File "/usr/lib/python3.8/http/client.py", line 1302, in _send_request
      self.endheaders(body, encode_chunked=encode_chunked)
    File "/usr/lib/python3.8/http/client.py", line 1251, in endheaders
      self._send_output(message_body, encode_chunked=encode_chunked)
    File "/usr/lib/python3.8/http/client.py", line 1011, in _send_output
      self.send(msg)
    File "/usr/lib/python3.8/http/client.py", line 951, in send
      self.connect()
    File "/usr/lib/python3.8/http/client.py", line 1418, in connect
      super().connect()
    File "/usr/lib/python3.8/http/client.py", line 922, in connect
      self.sock = self._create_connection(
    File "/usr/lib/python3.8/socket.py", line 787, in create_connection
      for res in getaddrinfo(host, port, 0, SOCK_STREAM):
    File "/usr/lib/python3.8/socket.py", line 918, in getaddrinfo
      for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
  socket.gaierror: [Errno -5] No address associated with hostname

  During handling of the above exception, another exception occurred:

  Traceback (most recent call last):
    File "./install.py", line 307, in <module>
      urllib.request.urlretrieve(url, container)
    File "/usr/lib/python3.8/urllib/request.py", line 247, in urlretrieve
      with contextlib.closing(urlopen(url, data)) as fp:
    File "/usr/lib/python3.8/urllib/request.py", line 222, in urlopen
      return opener.open(url, data, timeout)
    File "/usr/lib/python3.8/urllib/request.py", line 525, in open
      response = self._open(req, data)
    File "/usr/lib/python3.8/urllib/request.py", line 542, in _open
      result = self._call_chain(self.handle_open, protocol, protocol +
    File "/usr/lib/python3.8/urllib/request.py", line 502, in _call_chain
      result = func(*args)
    File "/usr/lib/python3.8/urllib/request.py", line 1397, in https_open
      return self.do_open(http.client.HTTPSConnection, req,
    File "/usr/lib/python3.8/urllib/request.py", line 1357, in do_open
      raise URLError(err)
  urllib.error.URLError: <urlopen error [Errno -5] No address associated with hostname>

  During handling of the above exception, another exception occurred:

  Traceback (most recent call last):
    File "./install.py", line 316, in <module>
      raise ContainerDownloadError()
  __main__.ContainerDownloadError
  mv: cannot stat 'lib/eic_xl-nightly.sif': No such file or directory
  chmod: cannot access '/home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif': No such file or directory
  ~/G4WORK/EIC/EICDIRC/epicdirc ~/G4WORK/EIC/EICDIRC/epicdirc
  /home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif
  ls: cannot access '/home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif': No such file or directory
  ERROR: no singularity image found
  ```
  **TL;DR -> The option --force is unavailable in singularity that I installed earlier.** From chatgpt got the following recommendation: 

  1. Why the error mentions Singularity
     Many HEP tools (e.g. CMS Combine, CRAB, or CMSSW utilities) still reference the command singularity for backward compatibility.
     However, the original Singularity project has since split into two separate branches:

      | Project           | Maintainer       | Current Binary | Status                              |
      | ----------------- | ---------------- | -------------- | ----------------------------------- |
      | **Apptainer**     | Linux Foundation | `apptainer`    | ✅ Official open-source continuation |
      | **SingularityCE** | Sylabs, Inc.     | `singularity`  | ⚙️ Commercial/community fork        |

      So, when a script reports an error mentioning Singularity, it doesn’t necessarily mean that you need the older “SingularityCE” package — it simply expects a compatible container runtime (which Apptainer   provides).

  2. Why Apptainer is recommended
     * Apptainer is the actively maintained and fully open-source successor to Singularity, now under the Linux Foundation.
     * SingularityCE is a commercial/community fork maintained by Sylabs.
     * Both provide nearly identical command-line interfaces, so you can safely alias:
     ```ln -s $(which apptainer) /usr/local/bin/singularity```
     to ensure full compatibility with tools that still expect the old singularity command name.
     **TL;DR -> Installing Apptainer ensures compatibility with modern HEP software while avoiding legacy issues.**

    Installing **apptainer** after removing singularity:
    * Remove singularity: ```sudo apt-get remove singularity``` 
    * Add the Apptainer repository: ```sudo add-apt-repository ppa:apptainer/ppa``` (this was needed since apptainer wasn't available in the default Ubuntu repositories) 
    * Install Apptainer: ```sudo apt-get install apptainer```

  The command gets stuck at a point while downloading ```eic_xl```. It only goes looking for an alternative when ```Ctrl+C``` is pressed.
  ```
  Downloading container from: https://eicweb.phy.anl.gov/api/v4/projects/290/jobs/artifacts/master/raw/build/eic_xl.sif?job=eic_xl:singularity:nightly
  Destination: /tmp/ci-fq0YQKk5vT/lib/eic_xl-nightly.sif
  ^[[A^[[A^[[A



  ^CWARNING: failed to retrieve container artifact
  Attempting alternative download from docker registry
  Executing: singularity pull --force /tmp/ci-fq0YQKk5vT/lib/eic_xl-nightly.sif docker://eicweb/eic_xl:nightly
  ```

* ```cd eic```
* ```git clone https://github.com/niwgit/epic.git```
* ```cd epic```
* ```git checkout box_envelope```
* ```mkdir install```
* ```cmake -B build -S . -DCMAKE_INSTALL_PREFIX=install```

  Facing following error while running the above:
  ```
  CMake Error at CMakeLists.txt:43 (find_package):
  By not providing "FindDD4hep.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "DD4hep", but
  CMake did not find one.

  Could not find a package configuration file provided by "DD4hep" (requested
  version 1.21) with any of the following names:

    DD4hepConfig.cmake
    dd4hep-config.cmake

  Add the installation prefix of "DD4hep" to CMAKE_PREFIX_PATH or set
  "DD4hep_DIR" to a directory containing one of the above files.  If "DD4hep"
  provides a separate development package or SDK, be sure it has been
  installed.


  -- Configuring incomplete, errors occurred!
  ```
  **TL;DR -> DD4HEP package needs to be installed**

  Checked DD4HEP installation requirements, it needs Boost (>=1.56)

  **Installing Boost**

  Boost is already installed, version 1.71

  **Installing EDM4hep**

  EDM4hep requires podio

  **Installing Podio**
  ```
  cmake -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/podio /home/shubhamdutta/Applications/Package_source/podio
  ```
  Got the following error while trying to run the above:
  ```
  CMake Error at CMakeLists.txt:115 (message):
  You are trying to build podio against a version of ROOT that has not been
  built with a sufficient c++ standard.  podio requires c++20 or higher
  ```

  Tried building the exisiting Root (v6.28.06) with C++20 by running:
  ```
  cmake -DCMAKE_INSTALL_PREFIX="/home/shubhamdutta/Applications/Package_install/root-6.28.06" -DGSL_CONFIG_EXECUTABLE="/home/shubhamdutta/Applications/Package_install/gsl-2.7/bin/gsl-config" -DGSL_DIR="/home/shubhamdutta/Applications/Package_install/gsl-2.7" -Dbuiltin_gsl="ON" -Dgdml="ON" -Dmathmore="ON" -Dpythia8="ON" -Droofit="ON" -DCMAKE_CXX_STANDARD=20 /home/shubhamdutta/Applications/Package_source/root_v6.28.06.source/root-6.28.06
  ```

  Got the following error (only small excerpt shown, rest is similar):
  ```
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/std.modulemap:69:12: error: header 'compare' not found
    header "compare"
           ^
  input_line_1:1:10: note: submodule of top-level module 'std' implicitly imported here
  #include <new>
         ^
  Warning in cling::IncrementalParser::CheckABICompatibility():
    Failed to extract C++ standard library version.
  While building module 'Core':
  While building module 'Cling_Runtime' imported from input_line_2:1:
  While building module 'Cling_Runtime_Extra' imported from /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/RuntimeUniverse.h:27:
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/std.modulemap:69:12: error: header 'compare' not found
    header "compare"
           ^
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/DynamicExprInfo.h:13:10: note: submodule of top-level module 'std' implicitly imported here
  #include <string>
         ^
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/DynamicExprInfo.h:40:7: error: use of undeclared identifier 'std'
      std::string m_Result;
      ^
  While building module 'Core':
  While building module 'Cling_Runtime' imported from input_line_2:1:
  While building module 'Cling_Runtime_Extra' imported from /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/RuntimeUniverse.h:27:
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/std.modulemap:69:12: error: header 'compare' not found
    header "compare"
           ^
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/DynamicLookupLifetimeHandler.h:12:10: note: submodule of top-level module 'std' implicitly imported here
  #include <string>
         ^
  /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/DynamicLookupLifetimeHandler.h:56:7: error: use of undeclared identifier 'std'
      std::string m_Type;
      ^
  While building module 'Core':
  While building module 'Cling_Runtime' imported from input_line_2:1:
  While building module 'Cling_Runtime_Extra' imported from /home/shubhamdutta/Applications/Package_build/root-6.28.06/etc/cling/Interpreter/RuntimeUniverse.h:27:
  ```
  **TL;DR -> Root v6.28.06 is incompatible with C++20**

  Building Root v6.36.04:
  ```
  cmake -DCMAKE_INSTALL_PREFIX="/home/shubhamdutta/Applications/Package_install/root-6.36.04" -DGSL_CONFIG_EXECUTABLE="/home/shubhamdutta/Applications/Package_install/gsl-2.7/bin/gsl-config" -DGSL_DIR="/home/shubhamdutta/Applications/Package_install/gsl-2.7" -Dbuiltin_gsl="ON" -Dgdml="ON" -Dmathmore="ON" -Dpythia8="ON" -Droofit="ON" -DCMAKE_CXX_STANDARD=20 /home/shubhamdutta/Applications/Package_source/root_v6.36.04.source/root-6.36.04
  ```
  Got the following warning:
  ```
  CMake Warning at cmake/modules/SearchInstalledSoftware.cmake:80 (_find_package):
  By not providing "FindXRootD.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "XRootD", but
  CMake did not find one.

  Could not find a package configuration file provided by "XRootD" with any
  of the following names:

    XRootDConfig.cmake
    xrootd-config.cmake

  Add the installation prefix of "XRootD" to CMAKE_PREFIX_PATH or set
  "XRootD_DIR" to a directory containing one of the above files.  If "XRootD"
  provides a separate development package or SDK, be sure it has been
  installed.
  Call Stack (most recent call first):
    cmake/modules/SearchInstalledSoftware.cmake:997 (find_package)
    CMakeLists.txt:287 (include)
  ```
   **TL;DR -> XRootD is missing**
  
  Installed XRootD without issues.

  There were few more libraries missing, while running cmake to install Root. Installed the missing libraries:
  ```
  sudo apt install liblzma-dev libxxhash-dev liblz4-dev libgif-dev libjpeg-dev libtiff-dev libgl2ps-dev libsqlite3-dev
  ```

  CMake was still taking C++17. Solution was to update gcc and g++ to version 11:
  ```
  sudo add-apt-repository ppa:ubuntu-toolchain-r/test  # Add Ubuntu Toolchain PPA to the repository list
  sudo apt install gcc-11 g++-11
  ```
  
  Make CMake use version 11 of gcc and g++:
  ```
  export CC=/usr/bin/gcc-11
  export CXX=/usr/bin/g++-11
  ```
  ```
  cmake --build . -- -j$(nproc)
  ```
  **Root v6.36.04 built sucessfully!**  
  
  ```
  cmake -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/podio /home/shubhamdutta/Applications/Package_source/podio
  ```
  Tried building podio again, got the following error:
  ```
  CMake Error at /home/shubhamdutta/.local/lib/python3.8/site-packages/cmake/data/share/cmake-3.27/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
    Could NOT find Vdt (missing: VDT_INCLUDE_DIR VDT_LIBRARY)
  Call Stack (most recent call first):
    /home/shubhamdutta/.local/lib/python3.8/site-packages/cmake/data/share/cmake-3.27/Modules/FindPackageHandleStandardArgs.cmake:600 (_FPHSA_FAILURE_MESSAGE)
    /home/shubhamdutta/Applications/Package_install/root-6.36.04/cmake/modules/FindVdt.cmake:63 (find_package_handle_standard_args)
    /home/shubhamdutta/.local/lib/python3.8/site-packages/cmake/data/share/cmake-3.27/Modules/CMakeFindDependencyMacro.cmake:76 (find_package)
    /home/shubhamdutta/Applications/Package_install/root-6.36.04/cmake/ROOTConfig.cmake:168 (find_dependency)
    CMakeLists.txt:89 (find_package)
  ```
  **TL;DR -> VDT is missing**

  Rebuilt Root with ```-Dbuilt-in_vdt="ON"``` and reinstalled.
  ```
  cmake -DCMAKE_INSTALL_PREFIX="/home/shubhamdutta/Applications/Package_install/root-6.36.04" -DGSL_CONFIG_EXECUTABLE="/home/shubhamdutta/Applications/Package_install/gsl-2.7/bin/gsl-config" -DGSL_DIR="/home/shubhamdutta/Applications/Package_install/gsl-2.7" -Dbuiltin_gsl="ON" -Dgdml="ON" -Dmathmore="ON" -Dpythia8="ON" -Droofit="ON" -Dbuiltin_vdt="ON" -DCMAKE_CXX_STANDARD=20 /home/shubhamdutta/Applications/Package_source/root_v6.36.04.source/root-6.36.04
  cmake --build . -j${nproc}
  cmake --build . install --target
  ```

  Tried building podio again, got the following warnings:
  ```
  CMake Warning at tests/CMakeLists.txt:22 (find_package):
  By not providing "Findnlohmann_json.cmake" in CMAKE_MODULE_PATH this
  project has asked CMake to find a package configuration file provided by
  "nlohmann_json", but CMake did not find one.

  Could not find a package configuration file provided by "nlohmann_json"
  (requested version 3.10) with any of the following names:

    nlohmann_jsonConfig.cmake
    nlohmann_json-config.cmake

  Add the installation prefix of "nlohmann_json" to CMAKE_PREFIX_PATH or set
  "nlohmann_json_DIR" to a directory containing one of the above files.  If
  "nlohmann_json" provides a separate development package or SDK, be sure it
  has been installed.
  ```
  ```
  CMake Warning at tests/unittests/CMakeLists.txt:4 (find_package):
  By not providing "FindCatch2.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Catch2", but
  CMake did not find one.

  Could not find a package configuration file provided by "Catch2" (requested
  version 3.5.0) with any of the following names:

    Catch2Config.cmake
    catch2-config.cmake

  Add the installation prefix of "Catch2" to CMAKE_PREFIX_PATH or set
  "Catch2_DIR" to a directory containing one of the above files.  If "Catch2"
  provides a separate development package or SDK, be sure it has been
  installed.
  ```
  **TL;DR -> Missing Nlohmann_json and Catch2**

  **Installing Nlohmann_json**
  ```
  cd /home/shubhamdutta/Applications/Package_source
  git clone https://github.com/nlohmann/json.git
  cd json
  cmake -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/nlohmann_json .
  make -j$(nproc)
  make install
  ```

  **Installing Catch2**
  ```
  cd /home/shubhamdutta/Applications/Package_source
  git clone https://github.com/catchorg/Catch2.git -b v3.5.0
  mv Catch2 catch2-3.5.0 && cd Catch2
  cd /home/shubhamdutta/Applications/Package_build
  mkdir catch2-3.5.0 && cd catch2-3.5.0
  cmake -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/catch2-3.5.0 /home/shubhamdutta/Applications/Package_source/catch2-3.5.0
  make -j$(nproc)
  sudo make install
  ```

  **Installing fmt**
  ```
  cd /home/shubhamdutta/Applications/Package_source
  git clone https://github.com/fmtlib/fmt.git -b 9.1.0
  mv fmt fmt-9.1.0 && cd fmt-9.1.0
  cd /home/shubhamdutta/Applications/Package_build
  mkdir fmt-9.1.0 && cd fmt-9.1.0
  cmake -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/fmt-9.1.0 /home/shubhamdutta/Applications/Package_source/fmt-9.1.0
  make -j$(nproc)
  sudo make install
  ```
  
  **Installing DD4HEP**

  Got the following error while trying to run ```cmake -DDD4HEP_USE_GEANT4=ON -DD4HEP_IGNORE_GEANT4_TLS=True -DBoost_NO_BOOST_CMAKE=ON -DBUILD_TESTING=ON -DROOT_DIR=$ROOTSYS -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/home/shubhamdutta/Applications/Package_install/DD4hep-1.32.1 /home/shubhamdutta/Applications/Package_source/DD4hep-1.32.1```
  ```
  CMake Error at cmake/DD4hepBuild.cmake:842 (MESSAGE):
    Geant4 was built with initial-exec, DD4hep requires 'global-dynamic'!
    Ignore this ERROR with DD4HEP_IGNORE_GEANT4_TLS=True
  Call Stack (most recent call first):
    CMakeLists.txt:164 (DD4HEP_SETUP_GEANT4_TARGETS)
  ```

  ```
  python problem ET.indent
  ```

  ```
  root problem with geomViewer
  ```

  ```
  pytest error
  ```
