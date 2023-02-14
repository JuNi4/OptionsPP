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
- Put the required libraries in the include folder
- If needed, change the default path for the options file and key binds file by changing the return string from the defaultOptionsPath function and defaultKeyBindsPath function

Demo script:
```cpp
/*
This goal of this demo is for you to understand how to use this library.
The things that this script acomplishes are:
 - it deletes the options file and key binds file
 - Next, it will check if the optionsfile already exists, wich it shuldn't
 - if it doesn't, it will check if the options.txt file in the default minecraft directory exists
 - if that is the case, it will import it
 - otherwise, it will create ana options.txt and keyBinds.txt from scratch
 
 - Next, it will load the options file
 - then it will directly write a different title to the optionsfile
 - after that it will save the unchanged json object, containing the options, to the file, reverting the in the previous step changed title back to Minecraft++
*/

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
