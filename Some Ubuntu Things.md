Monday 28 August 2017 01:45:05 AM IST 

- open windows folder in read only mode  
  - sudo mount -t ntfs-3g -o ro /dev/sda3 /media/windows
    
- go to file directory,right click, open in termnal and add ./ to executable files' name
 to execute them
 
- python....
  - python console: open terminal, write python
  - python file execution: in terminal goto file location> write python ./<filename.py>
							(if file name is "spaced name.py", write "spaced\ name.py")
												
-  creating studio icon
   - studio in unity launcher:launch your app. in the ide, goto tool>create desktop entry.  
                                          **(IMP: do NOT check for "create for all users )**
   - studio in bottom bar: left click on your working project>lock to launcher  
   - studio icon on desktop:  
      - create a textfile and write the following lines in it(changing the paths to exec and icon): 
```
[Desktop Entry]
Version=1.0
Type=Application
Name=Android Studio
Exec="/home/username/Programs/AndroidStudio/bin/studio.sh" %f
Icon=/home/username/Programs/AndroidStudio/bin/idea.png
Categories=Development;IDE;
Terminal=false
StartupNotify=true
StartupWMClass=jetbrains-android-studio
Name[en_GB]=android-studio.desktop
```  
  - rename to somename.desktop and save to desktopopen terminal on desktop  and write following commands:  
```cd ~/Desktop && chmod a+x somename.desktop```

