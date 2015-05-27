Vim “No Identifier Under Cursor” Arrow Keys Problem
===================================================

Recently, I started to have problems with Vim on my machine at work (a 24″ iMac), running OS X 10.5.7 and SSH’ing into a CentOS Linux server.  The arrow keys did not have the correct functionality.  Instead they would give errors such as “no identifier under cursor” and the session would freeze momentarily.  I noticed this in insert mode, the only mode I use arrow keys in, but I did a little testing and found that it occurred in all modes.  I wondered if the wrong key code was being sent, so I started doing some searching.

My search queries were coming up with nothing helpful.  The string “no identifier under cursor” was uncommon and many results were related to translations.  Including “Mac keyboard” or “Apple keyboard” didn’t help nor did the model number (A1243).  Eventually, I had the random thought to check the settings in the terminal application.

Under Terminal -> Preferences, there is a Settings tab.  Within that section, there’s a Keyboard tab, which didn’t seem to have the arrow keys listed, but there is also an Advanced tab.  One of the options is “Declare terminal as:” and was set to xterm-color.  I changed it to ansi and restarted the terminal app.  Now my arrow keys actually work how they should in Vim.
