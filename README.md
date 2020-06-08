[![Build Status](https://travis-ci.com/STaRiCHDED/Lab05.svg?branch=master)](https://travis-ci.com/STaRiCHDED/Lab05)
## Laboratory work V

<a href="https://yandex.ru/efir/?stream_id=vQw_LH0UfN6I"><img src="https://raw.githubusercontent.com/tp-labs/lab05/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```sh
$ open https://github.com/google/googletest
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
```sh
$ export GITHUB_USERNAME=STaRiCHDED
alias gsed=sed
$ cd ${GITHUB_USERNAME}/workspace
$  pushd .
~/STaRiCHDED/workspace ~/STaRiCHDED/workspace
$ source scripts/activate
$ $ git clone https://github.com/${GITHUB_USERNAME}/Lab04 projects/Lab05
$ cd projects/Lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/Lab05
$ mkdir third-party
$ git submodule add https://github.com/google/googletest third-party/gtest
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
$ git add third-party/gtest
$ git commit -m"added gtest framework"
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
> option(BUILD_TESTS "Build tests" OFF)
> ' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF
> 
> if(BUILD_TESTS)
>   enable_testing()
>   add_subdirectory(third-party/gtest)
>   file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
>   add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
>   target_link_libraries(check \${PROJECT_NAME} gtest_main)
>   add_test(NAME check COMMAND check)
> endif()
> EOF
$ mkdir tests
$ cat > tests/test1.cpp <<EOF
> #include <print.hpp>
> 
> #include <gtest/gtest.h>
> 
> TEST(Print, InFileStream)
> {
>   std::string filepath = "file.txt";
>   std::string text = "hello";
>   std::ofstream out{filepath};
> 
>   print(text, out);
>   out.close();
> 
>   std::string result;
>   std::ifstream in{filepath};
>   in >> result;
> 
>   EXPECT_EQ(result, text);
> }
> EOF
$ cmake -H. -B_build -DBUILD_TESTS=ON
$ cmake --build _build
$ cmake --build _build --target test
$ _build/check
$ cmake --build _build --target test -- ARGS=--verbose
$ gsed -i 's/Lab04/Lab05/g' README.md
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
$  gsed -i '/cmake --build _build --target install/a\
> - cmake --build _build --target test -- ARGS=--verbose
> ' .travis.yml
$  travis lint
$ git add .travis.yml
$ git add tests
$ git add -p
$  git commit -m"added tests"
$ git push origin master
$ travis login --auto
$ travis enable
$ mkdir artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png
```
## Report

```sh
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```
## Links
- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2020 The ISC Authors
```
