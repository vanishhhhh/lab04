# LAB 04 Панин ИУ8-23
## Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:
используйте TravisCI для сборки на операционной системе Linux с использованием компиляторов gcc и clang;
используйте AppVeyor для сборки на операционной системе Windows.

### Так как TravisCI сейчас доступен только за деньги, я сделал сборку linux через github actions, а windows через appveyor как и просили

## Сборка Linux
```
$ touch .github/workflow/cicd.yaml
$ subl .github/workflows/cicd.yaml

name: CI

on: [push, pull_request]

jobs:
  build-linux:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        compiler: [g++, clang++]

    steps:
      - uses: actions/checkout@v4

      - name: Install CMake
        run: sudo apt-get update && sudo apt-get install -y cmake

      - name: Configure
        run: cmake -S lab04 -B _build -DCMAKE_CXX_COMPILER=${{ matrix.compiler }} -DCMAKE_INSTALL_PREFIX=_install

      - name: Build
        run: cmake --build _build

      - name: Install
        run: cmake --build _build --target install
```
### Далее пуш и смотрим в Actions
#### Все собралось успешно(ниже confirgure build install)
#### 1) Confirgure

<details>
<summary>log</summary>

```
Run cmake -S lab04 -B _build -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_INSTALL_PREFIX=_install
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- The C compiler identification is GNU 13.3.0
-- The CXX compiler identification is Clang 18.1.3
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/clang++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at formatter_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at formatter_ex_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at solver_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at hello_world/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at solver/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (6.8s)
-- Generating done (0.0s)
-- Build files have been written to: /home/runner/work/lab04/lab04/_build
```

</details>

#### 2) Build

<details>
<summary>log</summary>

```
Run cmake --build _build
[ 10%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[ 20%] Linking CXX static library libformatter.a
[ 20%] Built target formatter
[ 30%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 40%] Linking CXX static library libformatter_ex.a
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 60%] Linking CXX static library libsolver_lib.a
[ 60%] Built target solver_lib
[ 70%] Building CXX object hello_world/CMakeFiles/hello_world.dir/hello_world.cpp.o
[ 80%] Linking CXX executable hello_world
[ 80%] Built target hello_world
[ 90%] Building CXX object solver/CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
```

</details>

#### 3) Install

<details>
<summary>log</summary>

```
Run cmake --build _build --target install
[ 20%] Built target formatter
[ 40%] Built target formatter_ex
[ 60%] Built target solver_lib
[ 80%] Built target hello_world
[100%] Built target solver
Install the project...
-- Install configuration: ""
-- Installing: /home/runner/work/lab04/lab04/_install/bin/hello_world
-- Installing: /home/runner/work/lab04/lab04/_install/lib/libformatter.a
-- Installing: /home/runner/work/lab04/lab04/_install/lib/libformatter_ex.a
-- Installing: /home/runner/work/lab04/lab04/_install/lib/libsolver_lib.a
```
</details>

## Сборка windows(создаем appveyor.yml)
```
$ subl appveyor.yml

version: 1.0.{build}

image: Visual Studio 2022

build_script:
  - cmake -S lab04 -B _build
  - cmake --build _build --config Release
  - cmake --build _build --config Release --target install
```
### Теперь заходим на сайт ci.appveyor.com
1) входим через гитхаб
2) создаем проект
3) добавляем туда lab04
4) собираем
### Сборка

<details>
<summary>log</summary>

```
Build started
git clone -q --branch=main https://github.com/vanishhhhh/lab04.git C:\projects\lab04
git checkout -qf 62d65582eb537a720fc233c52e58af8a80700ece
cmake -S lab04 -B _build
-- Building for: Visual Studio 17 2022
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
-- Selecting Windows SDK version 10.0.26100.0 to target Windows 10.0.17763.
-- The C compiler identification is MSVC 19.44.35219.0
-- The CXX compiler identification is MSVC 19.44.35219.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.44.35207/bin/Hostx64/x64/cl.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.44.35207/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at formatter_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
CMake Deprecation Warning at formatter_ex_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
CMake Deprecation Warning at solver_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
CMake Deprecation Warning at hello_world/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
CMake Deprecation Warning at solver/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.
  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.
-- Configuring done (28.3s)
-- Generating done (0.1s)
-- Build files have been written to: C:/projects/lab04/_build
cmake --build _build --config Release
MSBuild version 17.14.23+b0019275e for .NET Framework
  1>Checking Build System
  Building Custom Rule C:/projects/lab04/lab04/formatter_lib/CMakeLists.txt
  formatter.cpp
  formatter.vcxproj -> C:\projects\lab04\_build\formatter_lib\Release\formatter.lib
  Building Custom Rule C:/projects/lab04/lab04/formatter_ex_lib/CMakeLists.txt
  formatter_ex.cpp
  formatter_ex.vcxproj -> C:\projects\lab04\_build\formatter_ex_lib\Release\formatter_ex.lib
  Building Custom Rule C:/projects/lab04/lab04/hello_world/CMakeLists.txt
  hello_world.cpp
  hello_world.vcxproj -> C:\projects\lab04\_build\hello_world\Release\hello_world.exe
  Building Custom Rule C:/projects/lab04/lab04/solver_lib/CMakeLists.txt
  solver.cpp
  solver_lib.vcxproj -> C:\projects\lab04\_build\solver_lib\Release\solver_lib.lib
  Building Custom Rule C:/projects/lab04/lab04/solver/CMakeLists.txt
  equation.cpp
  solver.vcxproj -> C:\projects\lab04\_build\solver\Release\solver.exe
  Building Custom Rule C:/projects/lab04/lab04/CMakeLists.txt
cmake --build _build --config Release --target install
MSBuild version 17.14.23+b0019275e for .NET Framework
  formatter.vcxproj -> C:\projects\lab04\_build\formatter_lib\Release\formatter.lib
  formatter_ex.vcxproj -> C:\projects\lab04\_build\formatter_ex_lib\Release\formatter_ex.lib
  hello_world.vcxproj -> C:\projects\lab04\_build\hello_world\Release\hello_world.exe
  solver_lib.vcxproj -> C:\projects\lab04\_build\solver_lib\Release\solver_lib.lib
  solver.vcxproj -> C:\projects\lab04\_build\solver\Release\solver.exe
  1>
  -- Install configuration: "Release"
  -- Installing: C:/Program Files (x86)/lab03/bin/hello_world.exe
  -- Installing: C:/Program Files (x86)/lab03/lib/formatter.lib
  -- Installing: C:/Program Files (x86)/lab03/lib/formatter_ex.lib
  -- Installing: C:/Program Files (x86)/lab03/lib/solver_lib.lib
Discovering tests...OK
Build success
```
</details>


### Также хочу отметить что пришлось поменять required version во всех CMakeLists, так как это вызывало ошибку при сборке appveyor
