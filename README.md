# iOS libraries cheat sheet

- Library:
The binary, which contains all the compiled code.

- Header:
It's the public interface of a library. It allows the compiler to know all the public methods of a specific library.

- Framework:
It's a directory structure that contains a library, it's module and/or headers. It works for a single architecture.

- XCFramework:
It's a directory structure that contains Frameworks for different architectures. It is Apple's newest way of packaging libraries. 

- Fat library/Framework:
It's the same as a library or a framework, but there are several architectures embedded into the binary. It's the old way of managing multiple architectures.

## Library vs Framework vs XCFramework comparison

| Type                   | Library                                     | Framework                    | XCFramework                            |
|------------------------|---------------------------------------------|------------------------------|----------------------------------------|
| Static                 | ✅                                           | ✅                            | ✅                                      |
| Dynamic                | ✅                                           | ✅                            | ✅                                      |
| Multiple architectures | ❌                                           | ❌                            | ✅                                      |
| Can be fat?            | ✅                                           | ✅                            | no sense                               |
| Drag and drop in Xcode | ❌                                           | ✅                            | ✅                                      |
| Extension when static  | .a                                          | .framework                   | .xcframework                           |
| Extension when dynamic | None (Unix Executable File )                | .framework                   | .xcframework                           |
| Flag for compiler      | -I/path/to/header/parent                   | -F/path/to/framework/parent/ | Same as framework<br>But Xcode unwraps it internally |
| Flags for linker        | -L/path/to/library/parent<br>-lLibrary name | -F/path/to/framework/parent/ | Same as framework<br>But Xcode unwraps it internally |

## Dynamic vs Static comparison

| Type        | Static                                               | Dynamic                                                          |
|-------------|------------------------------------------------------|------------------------------------------------------------------|
| Link time   | When the binary is generated.<br>When yo do the Archive | At run time.<br>Every time you open the app                         |
| Launch time | Faster                                               | Slower                                                           |
| Size        | Smaller (as long as -Objc flag is not present)       | Bigger                                                           |
| dSYM        | One for the whole app                                | One for the app, and one more for each dynamic library/framework |
