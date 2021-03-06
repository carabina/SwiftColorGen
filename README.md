# SwiftColorGen
A tool that generate code for Swift projects, designed to improve the maintainability of UIColors. 

Please notice, this tool still under development. It's on a validation phase, where I'll test it integrated with existing iOS projects to see how useful it is. Feedbacks are appreciated.

**SwiftColorGen** reads all storyboard files to find common **sRGB colors**. Then it creates those colors in a **.xcassets** folder and refer them in the storyboard. Finally it creates a **UIColor extension** allowing to access the same colors programatically.

The rules for naming the colors dinamically:
- The closest web color name (https://en.wikipedia.org/wiki/Web_colors) is considered to name the color
- If the alpha value is less than 255, an "alpha suffix" will be appended to the color name, to avoid name collision
- If two RGB's are close to the same web color, the name still will be used if they have different alphas
- If two RGB's are close to the same web color and they also have the same alpha, the hex of the RGB will be used to avoid name collision

SwiftColorGen is written in Swift and requires Swift to run. The project uses [AEXML](https://github.com/tadija/AEXML) as a dependency to read and write XML and [CommandLine](https://github.com/jatoben/CommandLine) to provide the CLI interface.

# Screenshots
That's the result of the code generation:

### The generated named colors in the Storyboard
![Storyboard](https://github.com/fernandodelrio/SwiftColorGen/raw/master/Resources/Storyboard0.3.0.png)

### The generated colors in the Assets catalog
![Assets Catalog](https://github.com/fernandodelrio/SwiftColorGen/raw/master/Resources/Assets0.3.0.png)

### The generated Swift file
![Swift file](https://github.com/fernandodelrio/SwiftColorGen/raw/master/Resources/Swift0.3.0.png)

You can also create your own extensions in a separated file to give a more semantic name to the colors:

```swift
extension UIColor {
    class func defaultTextColor() -> UIColor {
        return .deepPinkColor()
    }
}
```

# Using the CLI
First, call the **build.sh** script (it will produce the **swiftcg** binary in the same folder):
```shell
$ chmod +x build.sh
$ ./build.sh
```

Then call the binary passing with the appropriate parameters:

```shell
Usage: ./swiftcg [options]
-o --outputFile (required):
    Path to the output Swift file
-b --baseFolder (optional):
    Path to the folder where the storyboards are located. Defaults to the current working directory
-a --assetsFolder (optional):
    Path to the assets folder. Defaults to the first .xcassets found under the base folder
 ```
 
Example:
```shell
$ ./swiftcg -o Example/Generated.swift
```

To test with the Example provided, call the **test.sh** script (it will update the files inside the Example folder):
```shell
$ chmod +x test.sh
$ ./test.sh
```

# Installation
You can install the tool using CocoaPods and then add a **Build Phase** step, that runs SwiftColorGen every time the project is built.

### CocoaPods
[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects.

1. To integrate SwiftColorGen into your Xcode project, specify it in your Podfile:
```ruby
pod 'SwiftColorGen'
```
2. Install the dependencies:

```shell
$ pod install
```
3. In Xcode: Click on your project in the file list, choose your target under TARGETS, click the **Build Phases** tab and add a **New Run Script Phase** by clicking the little plus icon in the top left. Drag the **New Run Script Phase** above the **Compile Sources phase** and below **Check Pods Manifest.lock**, expand it and then call the tool with something like that:
```shell
"$PODS_ROOT/SwiftColorGen/swiftcg" -b "$SRCROOT" -o "$SRCROOT/CustomColors.swift"
```
4. Build your project and it's done. Remember to add the generated Swift file to Xcode, if it's not already there.

# TODO
1. Add support to other color spaces than the sRGB
2. Reduce the number of changes in the storyboards
3. Update the color name and storyboard/.xcassets references, when the user manually updates the RGB of a named color in the assets folder
4. Test on a larger project to see what will happen
5. Add tests

# Contributing
This project still on a initial stage of development. Feel free to contribute by testing it and reporting bugs. If you want to help developing it, checkout the TODO list. If you made some enhancement, open a pull request.

# License
SwiftColorGen is available under the MIT license. See the LICENSE file for more info.
