---
layout: post
title: "Build Your Own Personal Voice Assistant Part 2"
permalink: "/media/technews/build-your-own-personal-voice-assistant-part2"
category: technews
image: "/images/technews/personal-voice-assistance-head.jpg"
---

![personal voice assistant](/images/technews/personal-voice-assistance-head.jpg)


In the previous post, we covered how to set up the Raspberry Pi and how to set up your microphone and speaker for your DIY voice assistant. In this last part of the tutorial, we will go through the process of setting up the Google Assistant and linking up the light for the finished product. 
---



> Hardware and software you need and where to find them:
 
 ![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)
  
 
 <table class="table-h">
  <tr>
    <!-- <th>TRANS Lab</th> -->
    <th>Hardware/Software</th>
    <th>What does it do?</th>
    <th>Where can I buy it?</th>
  </tr>
  <tr>
	  <td colspan="3"><b>FOR VOICE CONTROL SETUP</b></td>
  </tr>	
  <tr>
    <!-- <td colspan="3">TRANS Lab: A*STAR</td> -->
    <td>Browser Logged Into a Google Account</td>
    <td>To download the required software developer kit from Google and link the smart WiFi plug</td>
    <td>[nil]</td>
  </tr>
  <tr>
	  <td colspan="3"> <b>FOR LIGHT CONTROL CIRCUIT</b></td>
  </tr>	
  <tr>
    <!-- <td rowspan="5">NTU</td> -->
    <td>Lightbulb With Power Plug</td>
    <td>A home appliance that can be controlled by the smart WiFi plug</td>
    <td>Any hardware store</td>
  </tr> 
   <tr>
    <td>Sonoff WiFi UK Plug (We use Sonoff in this tutorial but feel free to use any brand of WiFi plug)</td>
    <td>The plug that links with the Google account for controlling the lights</td>
    <td>Tech stores or online</td>
  </tr> 
  <tr>
    <td>2.4Ghz WiFi</td>
    <td>Required to link the WiFi plug</td>
    <td>[nil]</td>
  </tr>
</table>
 
*Retailers are provided for reference only. GovTech TechNews is not affiliated with any of the retailers listed in this tutorial.*


**Step 1: Setting up the Google Project**

1. On the Pi's browser, sign in to your Google Account. Then go to Google [Action Console](https://console.actions.google.com/) and click "New Project". Give a name to the project and click "Create Project". This will take a while so proceed with the next steps while leaving the tab open. 

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)

2. Go to [Google Assistant API website](https://console.developers.google.com/apis/api/embeddedassistant.googleapis.com/overview?pli=1) and click Enable. This will allow us to use the Google Assistant on our Pi.

3. Go to [Activity Control Panel](https://myaccount.google.com/activitycontrols?pli=1) and make sure the following activities are turned on.
   * Web and App Activity (including Chrome history checkbox)
   * Device Information
   * Voice and Audio Activity

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)

4. Go back to Google [Action Console](https://console.actions.google.com) and find the Device Registration button.<br>
*(If you can't find the page for device registration, use https://console.actions.google.com/u/0/project/{project-name}/deviceregistration/. Note that you should change the {project-name} in the link to the project name you have created in Step 1)*<br>
<br>
Click on it and then click on Register Model. You can enter any name you like for your Pi and Manufacturer name* (the latter is not important but you still need to key in something), and set the Device type to Auto.

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)

5. Click on Register Model and download the OAuth2.0 Credentials. After the file has been downloaded, move it to the /home/pi directory

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)

6. Check if python is installed on the Pi by typing in the Terminal

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)
Exit out of the python programme by pressing ctrl-d.<br>
<br>
If it is not installed, install it by typing

![parts to build a personal voice assistant](/images/technews/personal-voice-assistant1.jpg)

7. Recommended by Google: Make a Python virtual environment for Google Assistant by first installing virtual environment and virtual environment wrapper
        pip install virtualenv
        pip install virtualenvwrapper
        

**Step 2: Boot Raspbian for the first time**

1. Login with the following credentials

    User: pi<br />Password: raspberry 

    (You can change the password by running Terminal and typing sudo raspi-config and selecting the Change Password option)

> Testing the microphone and speaker

2. Plug in the microphone and speaker
3. Open Terminal and run the following commands

        arecord -l
        aplay -l

arecord -l displays the list of input hardware devices

aplay -l displays the list of output hardware devices


4. Note down the card and device number of the microphone and speaker 

![a screenshot of a bus route](/images/technews/personal-voice-assistant2.jpg)

In the above screenshot, the recording device is USB PnP Sound Device and the playback is bcm2835 ALSA. So, the card no. and device no. for recording device is 1 and 0 respectively, and for playback it is 0 and 0.

5. Create a new file by typing the following command in Terminal

        sudo nano .asoundrc

6.   Copy the following code into the file, and adjust the card and device number according to what card and device numbers you have recorded down in step 3.

	pcm.!default {
	    type asym
	    capture.pcm "mic"
	    playback.pcm "speaker"
	}
	pcm.mic {
	    type plug
	    slave {
		pcm "hw:1,0"	# "hw:card,device (for recording)"
	    }
	}
	pcm.speaker {
	    type plug
	    slave {
		pcm "hw:0,0"	# "hw:card,device (for playback)"
	    }


![a screenshot of a bus route](/images/technews/personal-voice-assistant3.jpg)

7.   To test the audio setup, run the following code in Terminal

	arecord --duration=5 test.wav
	
The raspberry pi will record audio through the microphone for 5 seconds.
Then, run:


	aplay test.wav

to listen to the audio recorded.

![a screenshot of a bus route](/images/technews/personal-voice-assistant4.jpg)

And there you have it, your microphone and speaker are working and all that’s left is to set up the Google Assistant and link up the WiFi plug and lightbulb. 

Be sure to look out for part 2 coming next week where you learn to light up your room with a simple voice command!