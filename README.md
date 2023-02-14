## Minecraft++ Options file library
By: @JuNi4

A library for everyting related to the options.txt (Made to replicate the options.txt file from Minecraft)

-- Requirements --
  - nlohmann/json (https://github.com/nlohmann/json)

 (Included in MC++ [https://github.com/juni4/MinecraftPP])
  - utils.hpp
  - os.hpp
  
Installation:
- Download the latest release
- Extract the zip archive and move the options folder to you include folder
- put the required libraries in the include folder

Demo script:
```cpp
#include <nlohmann/json.hpp>
#include <iostream>

#include <options/options.hpp>

int main() {
    // -- Demo Options file implementaion --
    // delete the option files (only if they exist)
    options::deleteOptionsFile();
    options::deleteOptionsFile(options::defaultKeyBindsPath());

    // Check if optionsfile exists
    if ( ! std::filesystem::is_regular_file( options::defaultOptionsPath() )) {
        // If the mc options file exists: import it
        if (std::filesystem::is_regular_file(options::getMcPath()+"options.txt")) {
            options::importConfig();
        } else { // otherwise, create new files
            // Crate Options file
            options::createOptionsFile();
            // Create key binds file
            options::createOptionsFile(false, options::defaultKeyBindsPath(), options::defaultKeyBindStruct);
        }
    } else if ( ! std::filesystem::is_regular_file( options::defaultKeyBindsPath() ) ) {
        // Create key binds file
        options::createOptionsFile(false, options::defaultKeyBindsPath(), options::defaultKeyBindStruct);
    }
    // load the two files
    json opt = options::getOptions();
    json keys = options::getOptions(options::defaultKeyBindsPath());

    // get the title of the window
    std::cout << opt["title"] << std::endl;

    // set the title directly
    options::setOption("title", "Minecraft--");

    // set the title inside of the json object
    opt["title"] = "Minecraft**";

    // save the opt json object
    options::saveOptions(opt);

    // -- End of demo --
    return 0;
}
```
