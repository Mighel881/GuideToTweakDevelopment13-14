# How to get started with tweak development for complete beginners on MacOS by Kanns103






## Section 1 introduction:

So, you want to learn how to get started with iOS tweak development? Let’s hope that this guide will make it much easier for you to do so, make sure you have a mac. Let’s get started.






## Section 2 what you need:

To start off with, you need a MacBook or iMac. This tutorial is not for people who don’t own a Mac. Let me list the things required to get started with tweak development which we will set up next:


-	THEOS (A mac/windows environment that lets you create a tweak template and allows you to compile your tweaks).

-	XCode. If you are installing it off the AppStore, then you will automatically be prompted to install XCode 12, which breaks compiling for arm64e (iPhone XS and above). This is due to the XCode 12 toolchain. Luckily for you, I have created a bash script that lets you swap the toolchains easily without typing out the commands yourself and ruining your XCode. Do not use this script until prompted to do so.

-	A code editing software. I used to use Atom and it’s just brilliant, but it heated up my Mac a lot, so I switched to Sublime text. Another good one is Visual Studio Code.

-	Objective C knowledge would help, but it’s not necessary as we shall learn on the way.

-	Homebrew. Homebrew is the missing package manager that is required for tweak development in MacOS and many other things.

-	Latest SDK. SDK’s are what allow you to compile your tweaks.







## Section 3 basic knowledge:

Now that you understand what we need to create a tweak, let’s do some simple terminology within tweak development that most developers should know. If you already know these, you may skip it.


-	iPhone chips. For example, A12, A13, A14 etc.

-	Templates for your tweak. When you make a simple tweak template, you will be presented with 4 files, Tweak.x, .plist, makefile and control file. Your tweak.x is where your whole main code goes for your tweak, all your hooking and methods. Your plist file is where you target your tweak. For example, If you want to hide the dock, you would target com.apple.springboard because that is where you dock is located, on the springboard. But if you would like to make a tweak that targets all sections of your phone, like inside an app and the home screen, you can either use two plists, like com.apple.springboard or the bundle id of an app. Or, if you are locating many places and apps on your phone, the most realistic case would be to use com.apple.UIKit, which targets everything, But don’t worry, this doesn’t mean that if you want to change the dock that it will change everything, it means that you are able to hook into everywhere. 

-	Bundle ids and plists. The bundle id of an app is the unique identifier that is mainly used for creating themes and apps. Each app and tweak that somebody makes, has to be given a unique bundle id. This can be anything, but they usually look like this com.name.tweak. So if I wanted to make a tweak that hides the dock, I would use my developer name and the tweak name like com.kanns.hidedock.

-	How to create new files through terminal. I’m not sure what version apple breaks this on, but you used to just be able to create a new file by naming it a specific thing, for example, if you wanted to create a control file, you used to just be able to name the file ‘control’ and it would change it to a control file. But, now on the latest versions, you can no longer do that. To create a file like that now, you need to go into terminal and use the word ‘touch’, then the file name, but without the quotes. So, like this, touch control.







## Section 4 setting up:

So now that we covered the boring parts, let’s take a look into the juicier sections of tweak development, installing theos and everything that you need to create a tweak. Follow the steps below.


- Install XCode from the Mac Appstore. It’s a meaty download, but it is required.

- Installing THEOS and homebrew. So, seeing as you are a beginner, I am trying to make this guide as simple as possible for you. I have created a bash script on my GitHub named ‘QuickTHEOS’, which quickly installs homebrew and theos, without you having to type out all the commands yourself. Copy and paste the whole QuickTHEOS.sh file, apart from the top line (#!/bin/sh) and paste It into your terminal window, this will do some downloading. Let it run and don’t quit your terminal. 

- Once that is done installing, you need to run my script that I created which can be found on my GitHub named Xcode11Toolchain. This script fixes compiling for arm64e like we talked about earlier. Copy and paste everything in the Xcode11Toolchain.sh file apart from the top line (#!/bin/sh) into your Mac terminal window, then press enter. It will run and download some stuff, then when you are presented with an option menu, select number 1 and press enter, this will run some more commands. If you are unsure as to what is happening, the code you are pasting is all there. If anything goes wrong, or you just want the XCode 12 toolchain back for some reason, you can run the script again and select option 2. This will bring back the Xcode 12 toolchain and remove the 11 toolchain. Right, so now that you have selected option one, after it has run, just quit terminal.

- Downloading the SDK’s. Go into your name in finder, find the theos folder, go into the sdk folder and delete everything in there. Now download the file called 137SDK in the github page at the top. You can do this by making your safari page full screen, then clicking the download button. Once that is done, open it, drag the 137SDK folder inside your sdk folder in theos. If it is zipped, unzip it to a folder.







## Section 5 creating tweak:

So, now you have everything set up for creating a tweak, let’s get into it. We are going to create a new project without a preference bundle. Open up a fresh terminal window and type this $THEOS/bin/nic.pl and this should bring up some options (If you have done the steps before correctly. This is theos in action. Find the option that has iPhone/Tweak next to it, type the number next to it and press enter. 

- Next, the project name can be anything, obviously your tweak name. For the tweak we will make, we will be hiding the dock, so let’s call it HideDock14. Once you have typed that, press enter, and you will be presented with another option. 

- This time, it’s the package name. This is the bundle id that I was talking about earlier. So, type your unique bundle id. Note that this all HAS to be lower-case.

- Next, the Author/Maintainer is your developer name. If you don’t have one, just use your actual name. Type that and press enter. 

- Next, the bundle filter is what fills out the .plist file I was talking about above. So because we will be targeting the dock, it will be com.apple.springboard. By default, it is already that, so just press enter. 

- Then the next option which says Springboard, just press enter. And that’s it, your template has been created. Locate your name in finder and within the list of folders, there should be your tweak, titled hidedock14. Drag it to your desktop so it is easier to work with. 







## Section 6 creating the code:

Let’s actually create the code now. Open up your code editor on a fresh window and drag your whole tweak folder into it. 

- Click your control file. Your control file is the information and restrictions of your tweak. Most things should be filled out so just leave it be. 

- Next, open up your makefile and we need to add in a few things. At the top, put this. This will allow your tweak to work on A12 + devices. Remember, if you get confused, take a look at my files on FastCC on my github.

```
archs = arm64 arm64e
```

- Next, you need to install OpenSSH on your iPhone located in your package manager (cydia, zebra etc). This just makes it easier for you to test the tweak and install it on your device. 

- Once that is done, go back into your makefile and put this in it nearer the top and replace yourip with your IP address of the phone you just installed OpenSSH on and you want to test your tweak on.

```
THEOS_DEVICE_IP=yourip
```

- In your makefile again, in-between your THEOS_DEVICE_IP and archs = arm64 arm64e, put export SDKVERSION=13.7. This will help theos determine what sdk you are using. 


- Again, in your makefile, put this at the very bottom.

```
after-install::

	install.exec "sbreload"
 ```

- Next, open up your tweak.x and delete absolutely everything within it, this is just blank text. Go ahead and take a quick red if you like. In most cases, your code editor won’t recognise it as objective c so it will recognise it as plain text, but this won’t affect anything. Just go down to the bottom right, select plain text and select objective C or C from the list, now it will be coloured. 

- So, the piece of code that we are about to write will only be a few lines. Let me break it down bit by bit. To start off with, you need a hook and an end. A hook and end are important. A hook is the unique identifier on the method you want to change, and end is how you stop the code. To start off with, we are going to hide the dock on non-notched devices. So, type this,

```
%hook SBDockView
```

- We are doing this because we need to hook into the non-notched dock and hide it. We can find out what we need to hook with a tweak called Flexall. Flexall allows you to hold on the status bar then tap on what you want to change, it will then give you the view. After that, we can put that view into limneos header website and It will give you all the methods that come with it. Because we are hiding the dock, we need to use layoutSubviews. So, after layoutSubviews, it should look like this:

```
%hook SBDockView
-(void)layoutSubviews
```

- If you’re a beginner, don’t worry, this is all very confusing and will continue to be until you get a hold of this a bit better, just try not to get overwhelmed.

- After that, we need to hide it. So we need to use self.hidden and set the value to YES and finish it with %end. And the {} that you see is where you put the main piece of code. Without that, it won’t work and when you go to compile, it will spit out an error. The reason why we are setting it to YES is because we want to hide it. We can use self.hidden to do so. The final main code should look like this.

```
%hook SBDockView
-(void)layoutSubviews {
self.hidden = YES;
}
%end
```





- Congratulations, you have just hidden the dock on non-notched devices. But now, we need to hide it on notched devices. Just place this a couple lines underneath the other code. To do this, we just need to replace SBDockView with SBFloatingDockPlatterView. It will look like this:


```
%hook SBFloatingDockPlatterView
-(void)layoutSubviews {
self.hidden = YES;
}
%end
```






## Section 7 defining:

- So now we have our code. Simple enough? I hope so. Now comes the defining. Personally, I don’t like this as you have to define a lot with certain methods, especially when hiding objects. If we compile without the following code, it will fail. So, type this a couple lines underneath the code you just typed:



- This one is for non-notched: 

```
@interface SBDockView
@property (nonatomic, assign, readwrite, getter=isHidden) BOOL hidden;
@end
```



- This one is for notched:

```
@interface SBFloatingDockPlatterView
@property (nonatomic, assign, readwrite, getter=isHidden) BOOL hidden;
@end
```



- Now, we can compile. To compile, we need to cd into the project. But first, after making any changes, we need to save all changes, in Sublime text to save all, its option + command + s, but just to make sure, you can go to file and save all. 

- Next, open terminal, type cd then a space and drag your project folder and press enter. Once that is done, type in “make package” without quotes. If everything goes to plan, you will get a bunch of coloured writing. If not, you will get a red error. If you have, go back and make sure everything is typed correctly then try to compile again. If you still get the error, contact me on Twitter @kanns103 and we will resolve it.

- So, if everything went correctly and you compiled fine, you can now compile to your device. Type this “make package install”, but without the quotes and some of you will be presented with yes/no/fingerprint, type yes and press enter. 

- Next it will ask for a password. This is your root password for your iPhone. If you have not changed it, it is alpine. Because we told it to respring after, it will ask for it twice, so type it again when it asks you. When your device resprings, your dock should be hidden! To delete it off your phone, go into your package manager, refresh and it should be there as a package like all the others.







## Help, I have no idea how to continue from now!

Don’t panic, listen to the advice I am about to give.

I can’t stress this enough, LOOK AT OPEN-SOURCED TWEAKS ON GITHUB!!

Looking at open-source projects is the best way to learn tweak development because you can get an idea of how tweaks are made and the layout that comes with them. On my website which is linked at the bottom you can find a link to my GitHub and open-sourced tweaks. Currently there is a tweak called FastCC on there which speeds up your Control Centre for iOS 12, 13 and 14 devices, and you can see the code for it so you can practice.

### How did I make the code?

Well first I went onto Limneos header website and typed in CC. I typed in CC because I wanted to get a start on what I needed to hook as I had no idea. After I experimented with a few hooks, I found the correct view, which was CCUI2AnimationParameters, I was able to look at the methods that came with it which turned out to be a speed, delay and tension. I tried all of them and returned the value to either 0 or 100. I did this because I needed to test out which works, and which doesn’t. I first tried the speed as it looked like the most logical thing to do but soon realised that it had a weird animation when pulling down to access their Control Centre. So, I went onto tension and I wasn’t going to increase it, so I reduced it to 0 and it worked completely fine. If I were to increase the tension to 100 or 50 it would be really slow.

### How did I start?

I started about seven months ago by creating themes. I started making themes because I had no idea how to make a tweak and there were no good tutorials out there, so I decided to work my way up. At the time I had no idea where I was going to be now, so I just took it in my stride. I searched up how to make a theme for iOS 13 and a good guide from PinPal came up. I followed his guide correctly and made my first theme. After that, I started making respring packs and extending my themes, but It got to the point where It was way too easy, so I started to learn how to create tweaks. 

I have already mentioned all of this on my website but for those of you who haven’t read it, the first thing I did like many people was to search up how to make a tweak on YouTube. Take it from me this is completely useless, and they do not teach you anything at all. Of course, they teach you how to install Theos and everything needed to make the tweak, but they just started randomly coding and I had no idea what they were on about. So, instead I decided to look at open-source projects and that’s when things started to take off for me. I looked at how tweaks were created and how people found the code, and the pieces started joining together. One of the main places where I learnt was the jailbreakdevelopers sub reddit. They helped me with any errors I was getting and helped me understand how to find the methods and hooks for my tweaks, which I told you about earlier. Of course, reddit and other users help is great, but you will learn best through trial and error.






## Got errors you can’t solve? 

Feel free to drop me a message on [Twitter](https://twitter.com/kanns103) or by [Email](mailto:kannsbusiness@yahoo.com)




