## How to build & run

0) Check JerryScript requirements and install the softwares (compiler, cmake, etc.)
1) Run CMake in the project's root directory:

```sh
$ cmake -Hsrc -Bbuild
```

2) Run Make to compile the example application:

```sh
$ make -C build/
```

3) Run the compiled application:

```sh
$ ./build/app-jerry 
```
