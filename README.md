# <img src="https://user-images.githubusercontent.com/70295997/222833381-a3c37bfd-8c71-47a4-9e73-207cdee54d10.png" width=40> I want to capture a video. How can I do it with an <code>adb</code> command?

I use the <code>screenrecord</code> command, which is an ADB shell utility, to capture a video of the screen on an Android device (Android 4.4 (API level 19) and higher). The utility records screen activity to an MPEG-4 file. To do this, follow these steps:

1. Connect the device to the machine: Use a USB cable to connect the device to the machine. Make sure that USB debugging is enabled on the device.

2. Start the ADB shell: Open a terminal or command prompt and navigate to the directory where the Android SDK is installed. Then, use the following command to start the ADB shell:

            adb shell

3. Start the recording. Within the ADB shell, use the following command to start the screenrecord:

            screenrecord /sdcard/<video_name>.mp4

      I can combine the above commands into one:
      
            adb shell screenrecord /sdcard/<video_name>.mp4

      This will start capturing a video of the screen and saving it to the device's SD card. Replace <code>video_name</code> with the desired name for the video file.
      
      The following errors might occur:
      
            Unable to get output buffers (err=-38)
            Encoder failed (err=-38)
      
            ERROR: Unable to configure video/avc codec at 1440x3120 (err=-22)
            WARNING: failed at 1440x3120 retrying at 720x1280

      To [resolve](https://android.stackexchange.com/questions/168944/unable-to-get-output-buffers-err-38-when-attempting-to-screen-record-emulator) it, try to use the size option <code>--size widthxheight</code>.

      Another error might occur:
      
            /system/bin/sh: screenrecord: inaccessible or not found
            
     [Check](https://stackoverflow.com/questions/46482178/adb-screenrecord-command-not-found) if the device has a built-in recorder. Most devices (with recent Android versions) should have the <code>screenrecord</code> included, but it seems some older devices have it missing. To record a video in this case, use Android Studio's built-in options to record. The Play Store also avails many free recording apps, such AZRecorder.


4. Perform the actions that I want to capture in the video: Once the video capture is started, perform the actions that I want to capture in the video.
      
5. Stop the screenrecord command: To stop the video capture and save the video, press CTRL+C in the terminal or command prompt. Alternately, use the following ADB command:
      
            adb shell killall screenrecord

6. Retrieve the video from the device: To retrieve the video from the device, use the following ADB command:

            adb pull /sdcard/<video_name>.mp4

      This will copy the video file from the device's SD card to the current working directory on the local machine.         Alternatively, pull the video file to a specific folder:
      
            % adb pull /sdcard/video_name.mp4 ~/Documents
            /sdcard/video_name.mp4: 1 file pulled, 0 skipped, 33.3 MB/s (2176197 bytes in  0.062s)
      
      If there are any issues with the file retrieval, verify the file has been saved in the device's <code>/sdcard</code> folder:

            & adb shell
            emulator64_arm64:/ $ ls
            … sdcard …
            emulator64_arm64:/ $ cd sdcard
            emulator64_arm64:/sdcard $ ls
            … video_name.mp4 …

7. Remove the file from the device with the <code>rm</code> command:
	
            adb shell rm /sdcard/ErrorMsgRegistrationScreen.mp4

Also make sure to <code>exit</code> the device <code>shell</code> prior to pulling the file to your computer. Because ADB is a server that lives on your machine, not inside the device directory.
	
You can pass various options into the screenrecord command:

            adb shell screenrecord --size 1280x720 /sdcard/<video_name>.mp4
            adb shell screenrecord --bit-rate 4000000 /sdcard/<video_name>.mp4
            adb shell screenrecord --time-limit=120 /sdcard/<video_name>.mp4
            adb shell screenrecord --verbose /sdcard/<video_name>.mp4
            adb shell screenrecord --help /sdcard/<video_name>.mp4

Limitations of the screenrecord utility:
- Time limit is 3 minutes.
- Audio is not recorded with the video file.
- Video recording is not available for devices running Wear OS.
- Some devices might not be able to record at their native display resolution. If you encounter problems with screen recording, try using a lower screen resolution.
- Rotation of the screen during recording is not supported. If the screen does rotate during recording, some of the screen is cut off in the recording.

Overall, using the ADB screenrecord command can be a convenient way to capture a video of the screen on an Android device. You can use this technique to record video during testing or debugging, or to create demos or tutorials. Using ADB to capture a video of the screen on an Android device can be a useful way to record and analyze the behavior of an app or to troubleshoot issues.

----

[Kill Processes in Linux – Kill, Pkill, Killall Commands](https://linuxnightly.com/kill-processes-in-linux-kill-pkill-killall-commands/)

[Record a Video](https://developer.android.com/studio/command-line/adb#screenrecord)

[adb shell screenrecord](https://adbshell.com/commands/adb-shell-screenrecord)
