# EMTG_153 Instructions
The following instructions are for setting up NASA's EMTG on Mac. This repository is a forked version of the original EMTG repository and has been edited to ensure it works on Mac, assuming the rest is setup correctly as shown below. Changes that were made between this repository and the original repository may be viewed in the code itself, but details of the changes are also outlined at the end of this README.


## Prerequisites
Below are things that need to be done before EMTG can be built. It is recommended to use Homebrew to install necessary software.


### GSL
Install GSL (`brew install gsl`) and make sure the `EMTG-Config.cmake' file points to its `lib` directory


### CMake
Install CMake (`brew install cmake`)


### Boost
Install Boost if not already dont so (`brew install boost`)


### GCC version 11
1. Install GCC version 11 (`brew install gcc@11`)
2. In terminal shell profile (if using bash then `nano ~/.bashrc`, if using zshrc then `nano ~/.zshrc`), add pointers to gcc
    1. `export CXX=/path/to/g++-11` (for example, `export CXX=/opt/homebrew/Cellar/gcc@11/11.3.0/bin/g++-11`)
    2. `export CC=/path/to/gcc-11` (for example, `export CC=/opt/homebrew/Cellar/gcc@11/11.3.0/bin/gcc-11`)
    3. `export FC=/path/to/gfortran-11` (for example, `export FC=/opt/homebrew/Cellar/gcc@11/11.3.0/bin/gfortran-11`)
4. Make sure shell is updated by either quitting and reopening or running `source ~/.bashrc' (or `source ~/.zshrc`)


### Autoconf
Install autoconf (`brew install autoconf`)


### SNOPT
1. Unzip the zip flolder containing SNOPT anywhere
2. Go inside SNOPT root
3. Run `./configure --with-cpp`
4. Run `make` (warnings can be ignored)
5. Run `make install`
6. Run `make check` and ensure no errors occur
7. Inside SNOPT root, run `git clone https://github.com/snopt/snopt-interface.git` and rename new folder (should be `/snopt-interface') to `/interfaces` (with `mv snopt-interface interfaces`)
8. Go inside `/interfaces`
9. Make sure autoconf is installed (homebrew)
10. Run `autoconf`, run `autoupdate` if prompted then rerun `autoconf`
11. Run `./configure --with-snopt=/path/to/your/snopt/lib` (for example, `./configure --with-snopt=/Users/nick/Documents/SNOPT/snopt7/lib`)
13. Run `make` (ignore warnings)
14. Run `make install`
15. Run `make examples`


## Building EMTG_153
1. Create “build” folder in EMTG root
2. Go inside /build
3. In terminal type “cmake ..”
4. Then type “make”


## Python
Download version of python for EMTG with `env PYTHON_CONFIGURE_OPTS="--enable-framework` pyenv install 3.10.4”


## General notes that might help
_ Make sure NASA's CSIPCE (https://naif.jpl.nasa.gov/naif/toolkit.html) is built for correct operating system


## Steps Takent to Modify Original EMTG Repository
1. Clone original EMTG repository (https://github.com/nasa/EMTG)
2. Copy `EMTG-Config-template.cmake` and rename as `EMTG-Config.cmake`
3. In `EMTG-Config.cmake`
    1. Set 'CSPCICE_DIR' to wherever NASA’s SPICE toolkit has been downloaded (download to anywhere other than EMTG root)
    2. Set 'SNOPT' to point to SNOPT root location (anywhere other than EMTG root)
    3. Comment out all Boost related settings
    4. Change GSL path to wherever GSL `lib` is
    5. Change python requirement version to 3.10 (or 3.9, should still work)
4. Download `randutils.hpp` (from https://gist.github.com/gnzlbg/efbacf7c668f783cd021) and put into `emtg/src/Math/`
5. In `emtg/CMakeLists.txt`, change file extension in lines 206 and 207 from `.so` to `.dylib`
6. In `emtg/Utilities/file_utilities.cpp`
    1. Comment out line 22
    2. Add `#include <filesystem>`
    3. Change `namespace fs = ::boost::filesystem` to `namespace fs = std::filesystem`
7. In `emtg/Utilities/file_utilities.h`
    1. Comment out lines 23 and 24
    2. Comment out line 29
    3. Add `#include <filesystem>`
    4. Change `namespace fs = ::boost::filesystem` to `namespace fs = std::filesystem`
8. In `emtg/src/Executable/EMTG_v9.cpp`
    1. Comment out line 50
    2. Add `#include <filesystem>`
    3. Change all `boost::filesystem` to `std::filesystem` (use search and replace), remove all `::` from `::std::filesystem` where necessary 
