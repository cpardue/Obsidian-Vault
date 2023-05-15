 The Story

![AoC day 10 banner](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/cebaa8a8bc1647852b7488a611d3800d.png)

Check out Alh4zr3d's video walkthrough for Day 10 [here](https://www.youtube.com/watch?v=_ej3yMF31zg)!  

  

Santa's team have done well so far. The elves, blue and red combined, have been securing everything technological all around. The Bandit Yeti, unable to hack a thing, decided to go for eldritch magic as a last resort and trapped Elf McSkidy in a video game during her sleep. When the rest of the elves woke up, their leader was nowhere to be found until Elf Recon McRed noticed one of their screens, where Elf McSkidy's pixelated figure could be seen. By the screen, an icy note read: **"Only by winning the unwinnable game shall your dear Elf McSkidy be reclaimed"**.

Without their chief, the elves started running in despair. How could they run a SOC without its head? The game was rigged, and try after try, the elves would lose, no matter what. As struck by lightning, Elf Exploit McRed stood up from his chair and said to the others: **"If we can't win it, we'll hack it!"**.

![Elves in despair](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8ef207ff54fb2726ed9791aaa1e16f03.png)  

Learning Objectives

-   Learn how data is stored in memory in games or other applications.
-   Use simple tools to find and alter data in memory.
-   Explore the effects of changing data in memory on a running game.

The Memory of a Program

Whenever we execute a program, all data will be processed somehow through the computer's RAM (Random Access Memory). If you think of a videogame, your HP, position, movement speed and direction are all stored somewhere in memory and updated as needed as the game goes. 

![Game memory layout](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/e73c41674384b537096f1e800b3edd95.png)  

  

If you can modify the relevant memory positions, you could trick the game into thinking you have more HP than you should or even a higher score! This sounds relatively easy, but a program's memory space is vast and sparse, and finding the location where these variables are stored is nothing you'd want to do by hand. Hopefully, some tools will help us navigate memory and find where all the juicy information is at.

  

Be sure to hit the **Start Machine** button before continuing. The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page. All you need for this challenge is available in the deployable machine. If you prefer to do so, however, you can download and install Cetus on your own machine by downloading it [from here](https://github.com/Qwokka/Cetus/releases/download/v1.03.1/Cetus_v1.03.1.zip).

![Cetus Logo](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/e4960f1f69afa78cb4d0aded93e8dc08.png)  

The Mighty Cetus

Cetus is a simple browser plugin that works for Firefox and Chrome, allowing you to explore the memory space of Web Assembly games that run in your browser. The main idea behind it is to provide you with the tools to easily find any piece of data stored in memory and modify it if needed. On top of that, it will let you modify a game's compiled code and alter its behaviours if you want, although we won't need to go that deep for this task.

Cetus is already installed on Chrome in your deployed machine, so you can use it straight away for the rest of the task. If you find the game runs slowly when using the in-browser machine, you can always install Cetus on your machine and do the task from there, following the indications given below.

_Installing Cetus on Firefox (Click to read)_  

Installing Cetus on Firefox 

To run Cetus on Firefox, you will need to load it as a temporary extension since it isn't available on the add-ons website. To do this, open Firefox and type `about:debugging` in the address bar. Once in there, go to `This Firefox`, where you'll find a button to load temporary add-ons. Temporary add-ons will only be loaded for as long as the browser remains open, so you'll need to load it again if you close it for some reason:

![Temporary Firefox Add-ons](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/af555c27570e44ae1bf8d39676fafb1b.png)

To load the extension, you will need to browse to the directory where you downloaded and extracted Cetus and select the `manifest.json` file inside the main folder and click OK:

![Firefox Cetus Loaded](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/c4034e1b27dc90e06b00c9a057e4e6a8.png)

Now we are ready to save Elf McSkidy!

_Installing Cetus on Chrome (Click to read)_  

Installing Cetus on Chrome

To install Cetus on Chrome, type `chrome://extensions` in your address bar. This will take you to the extensions manager. You will need to enable the `Developer mode`, enabling additional buttons to load the extension. Click the `Load unpacked` button and point to the directory where you uncompressed the plugin.

![Chrome Developer Mode](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/0d33d33a53c1a9e04febc644514a80e6.png)  

This will open an explorer window where you'll need to go to the directory where Cetus was uncompressed, and press the Open button from there without selecting any file:

![Chrome Open Cetus](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/82361c8235a71220e8fe22421a77daa0.png)

Once you are done, you should see Cetus loaded in your extensions:

![Chrome Cetus Loaded](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/abe3ea1a686f5b65484ff825e120501d.png)  

Accessing Cetus

To open the game, go to your deployed machine and click the "Save Elf McSkidy" icon on the desktop. This will open Google Chrome with Cetus already loaded for you.

![Game Icon in Machine](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/9b706d58ce17041d4708ad2d640bd589.png)  

To find Cetus, you need to open the `Developer tools` by clicking the button on the upper-right corner of Chrome, as shown in the figure below:

![Developer Tools](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/e50b53bdb7f272a9b88f850197c90275.png)  

Cetus is located in one of the tabs there:

![Finding Cetus](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/3a8732a7ac9e05b0f2bceed318142a35.png)  

With Cetus open, hit the refresh button to reload the game. If you installed Cetus on your machine, you can find the game at [https://MACHINE_IP/](https://machine_ip/). Cetus should detect the web assembly game running and show you the available tools:

![Cetus Interface](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/faaa4e72884c28021071797bc81e003c.png)  

**Note:** If Cetus shows the "Waiting for WASM" message, just reload the game, and the tools should load.

Guess the Guard's Number

If you walk around the game, you will find that the guard won't let you leave unless you guess a randomly generated number. At some point, the game must store this number in memory. Cetus will allow us to pinpoint the random number's memory address quickly.

As a first step, talk to the guard and try to guess the number randomly. You probably won't guess it first try, but take note of the guard's number.

![Guard random number](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/86780fce97ef5e28dcbb3ca5d0948859.png)  

You can use Cetus to find all the memory addresses used by the game that match the given value. In this case, the guard's number is probably a regular integer, so we choose `i32` (32-bit integer) in Value Type.

Cetus also allows you to search for numbers with decimals (usually called floats), represented by the `f32` and `f64` types, and for strings encoded in `ascii`, `utf-8` or `bytes`. You need to specify the data type as part of your search because, for your computer, the values `32` (integer) and `32.0` (float) are stored in different formats in memory.

We will use the `EQ` comparison operator, which will search for memory addresses which content is equal to the value we input. Note that you can also search values using any of the other available operators. For reference, this is what other operators do:

**Operator**

**Description**

EQ

Find all memory addresses with contents that are **equal** to our inputted value.

NE

Find all memory addresses with contents that are **not equal** to our inputted value.  

LT

Find all memory addresses with contents that are **lower than** our inputted value.  

GT

Find all memory addresses with contents that are **greater than** our inputted value.  

LTE

Find all memory addresses with contents that are **lower than or equal to** our inputted value.  

GTE

Find all memory addresses with contents that are **greater than or equal** to our inputted value.  

Since the guard uses a random number, you will likely find the memory address on the first try. Once you do, click the bookmark button on the right of the memory address:  

![Searching the guard's number](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/9f0ec4e56f19e3ff32c3fe24261c39fe.png)  

You can then go to bookmarks to see your memory addresses:

![Cetus Bookmarks](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/351ece705759930b84a3a1b6f021a54c.png)  

Note that Cetus uses hexadecimal notation to show you the numbers. If you need to convert the shown numbers to decimal, you can use [this website](https://www.rapidtables.com/convert/number/hex-to-decimal.html).

With Cetus on the bookmarks tab, talk to the guard again and notice how the random number changes immediately. You can now guess the number:

![Guessing the number](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/febcff5c18091088aa94fc54bb3a0a5a.png)  

Convert the number from hexadecimal to get the guard's number (0x005c9d35 = 6069557). You defeated the guard (sort of)!

**Note:** You can also modify the memory address containing the random number from the bookmarks tab. Try restarting the game and changing the guard's number right before the guard asks you for your number. You should now be able to change the guard's number at will!

Getting through the bridge

You are now out of your cell, but you still have to overcome some obstacles. Can you figure out how?

![The Bridge](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/d874327f5f22f557ab32cdd5a6671d6b.png)

While you are wondering what other data in memory could be changed to survive the bridge, Elf Recon McRed tells you that he read about **differential search**. Differential Search, he said, allows you to run successive searches in tandem, where each search will be scoped over the results of the last search only instead of the whole memory space. Elf Recon thinks this might be of help somehow.

To help you better understand, he used the following example: suppose you want to find an address in memory, but you are not sure of the exact value it contains, but you can, however, manipulate it somehow by doing some actions in the game (you could manipulate the value of your position by moving, for example). Instead of doing a direct search by value as before, you can use differential search to look for memory positions based on specific **variations on the value**, rather than the value itself.

To start the differential search mode, your first search needs to be done with an empty value.  

![ElfRecon1](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/7d3f7a832da5cfe8d01245c6a4957daf.png)![Differential Search 1](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/21eb5bd08fe1523e02ef337b55ad9b78.png)

This will return the total number of memory addresses mapped by the game, which is `458753` in the image above. Now, suppose you want to know which memory addresses have decreased since the last search. You can run a second search using the `LT` operator without setting a value to search:

![ElfRecon2](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/0f87691ac4d57ba14dca67c3229f6b29.png)![Differential Search 2](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/098df9dbaf2afef7a7659b3ed0caeb99.png)

The result above tells us that only `44` memory positions of the total of `458753` have decreased in value since the last search. You can of course, continue to do successive searches. For example, if you now wanted to know which of the `44` resulting memory addresses from the first search have increased their value, you could simply do another search with the `GT` operator with no value again.

![Differential Search 3](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/ac51390fdf80ae16f0b96a23f8808e1e.png)![ElfRecon3](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/ccc56738fc0010c83a6619415dfa9588.png)

The result tells us that from the `44` memory addressed from the last search, only `26` have increased in value. If you are searching for a particular value, you can continue to do more searches until you find the memory address you are trying to get.