### Using the Library Manager

1. **Open Arduino IDE**:
   Launch the Arduino IDE on your computer.

2. **Access the Library Manager**:
   - Go to the menu bar and click on **Tools** > **Manage Libraries...** or
   - Press `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Shift+I` (Mac).

3. **Search for the Library**:
   - In the **Library Manager** window, type the name of the library you're looking for in the search box.
   - A list of libraries will appear, matching your query.

4. **Install the Library**:
   - Locate the desired library in the list.
   - Click the **Install** button next to the library name.
   - Some libraries allow you to choose a specific version from a dropdown; if necessary, select the version you want before installing.

5. **Wait for Installation**:
   - The IDE will download and install the library.
   - Once completed, the library will be ready for use.

### Manual Installation

If the library is not available in the Library Manager, you can install it manually:

1. **Download the Library**:
   - Visit a trusted source like [Arduino Library GitHub repositories](https://github.com/) or other official websites.
   - Download the library as a `.zip` file.

2. **Add the Library**:
   - In the Arduino IDE, go to **Sketch** > **Include Library** > **Add .ZIP Library...**.
   - Select the downloaded `.zip` file.

3. **Confirm Installation**:
   - The IDE will unpack and install the library.
   - You'll see a message in the IDE console confirming the installation.

### Manual File Placement

1. **Extract the `.zip` File**:
   - Unzip the library archive to reveal its folder.

2. **Move the Folder**:
   - Copy the extracted folder to the Arduino libraries directory:
     - **Windows**: `Documents\Arduino\libraries`
     - **Mac**: `Documents/Arduino/libraries`
     - **Linux**: `~/Arduino/libraries`

3. **Restart the Arduino IDE**:
   - Relaunch the Arduino IDE to detect the new library.

### **Verify the Installation**
1. Open a new sketch in Arduino IDE.
2. Go to `Sketch` > `Include Library`.
3. Check the dropdown list for the installed library.
4. If found, the library is successfully installed and ready to use.

--- 

Include the library in the code using `#include <LibraryName.h>`.
