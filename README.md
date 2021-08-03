# iOS libraries cheat sheet

- Library:
  - The binary, which contains all the compiled code.

- Header:
  - It's the public interface of a library. It allows the compiler to know all the public methods of a specific library.

- Framework:
  - It's a directory structure that contains a binary, it's module and/or headers. It works for a single architecture.

- XCFramework:
  - It's a directory structure that contains frameworks or libraries for different architectures. It is Apple's newest way of packaging frameworks/libraries. 

- Fat library/framework:
  - It's the same as a library or a framework, but there are several architectures embedded into the binary. It's the old way of managing multiple architectures.

> :warning: If you decide to use fat libraries/frameworks instead of XCFrameworks, you must remove simulator architecure before send it to the AppStore, or you binary will be rejected.

## Library vs Framework vs XCFramework comparison

| Type                   | Library                                     | Framework                    | XCFramework                            |
|------------------------|---------------------------------------------|------------------------------|----------------------------------------|
| Static                 | ✅                                           | ✅                            | ✅                                      |
| Dynamic                | ✅                                           | ✅                            | ✅                                      |
| Multiple architectures | ❌                                           | ❌                            | ✅                                      |
| Can be fat?            | ✅                                           | ✅                            | no sense                               |
| Drag and drop in Xcode | ❌                                           | ✅                            | ✅                                      |
| Extension when static  | .a                                          | .framework                   | .xcframework                           |
| Extension when dynamic | .dylib               | .framework                   | .xcframework                           |
| Flag for compiler      | -I/path/to/header/parent                   | -F/path/to/framework/parent/ | Same as framework<br>But Xcode unwraps it internally |
| Flags for linker        | -L/path/to/library/parent<br>-lLibrary name | -F/path/to/framework/parent/ | Same as framework<br>But Xcode unwraps it internally |

## Dynamic vs Static comparison

| Type        | Static                                               | Dynamic                                                          |
|-------------|------------------------------------------------------|------------------------------------------------------------------|
| Link time   | When the binary is generated.<br>When yo do the Archive | At run time.<br>Every time you open the app                         |
| Launch time | Faster                                               | Slower                                                           |
| Size        | Smaller (as long as -Objc flag is not present)       | Bigger                                                           |
| dSYM        | One for the whole app                                | One for the app, and one more for each dynamic library/framework |

## How to build them

- Library:
  - Xcode > File > New Project > Static Library
  - You can change from static to dynamic in Build Settings > Mach-O Type
  - Set Build Settings > Skip Instal > No
  - Archive it for the desire architecture (simulator or Any iOS Device), and you will find the library in the products folder inside the archive

- Framework:
  - Xcode > File > New Project > Framework
  - You can change from dynamic to static in Build Settings > Mach-O Type
  - Set Build Settings > Skip Instal > No
  - Archive it for the desire architecture (simulator or Any iOS Device), and you will find the framework in the products folder inside the archive
  - If you can't archive it for simulator in Xcode, use `xcodebuild` command in terminal to do it.

- XCFramework:
  - You will need two libraries or frameworks built for different architectures.
  - With two frameworks: 
  ```bash
  xcodebuild -create-xcframework \
  -framework "path/to/simulator.framework" \
  -framework "path/to/arm64.framework" \
  -output output/path/myXCFramework.xcframework
  ```
   - With two libraries: 
   - Use `.dylib` instead of `.a` for dynamic
    ```bash
    xcodebuild -create-xcframework \
    -library "path/to/simulator.a" \
    -library "path/to/arm64.a" \
    -output output/path/myXCFramework.xcframework
    ```
 - Fat library: 
   - You will need two libraries or frameworks built for different architectures.
   - Use `.dylib` instead of `.a` for dynamic
    ```bash
    lipo -create \
    "path/to/simulator.a" \
    "path/to/arm64.a" \
    -output output/path/myFatLibrary.a
    ```
 ## CocoaPods
 
 By default, CocoPods uses dyamic frameworks, if you want to get the advantages of static libraries, change this line in your podfile:
 
```ruby
use_frameworks!
```

for

```ruby
use_frameworks! :linkage => :static
```

If you don't have CocoPods `1.9.0`, you will have to use `use_modular_headers!` instead of `use_frameworks!`
