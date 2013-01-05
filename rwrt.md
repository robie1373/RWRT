<!SLIDE bullets incremental transition=fade>
# Real world Ruby tricks.

*No plan survives engagement with the enemy and no script survives engagement with the environment. I'll show a couple of Ruby-isms that I've found make my scripts more robust, safer, or more useful in messy situations.*


<!SLIDE bullets incremental transition=fade> 
## Tempfile library 
* Scenario - Download data from Internet for future use
* Scenario - Respecting your user's ~ directory
* Lazy interprocess communication

<!SLIDE bullets incremental transition=fade>
## SHA and/or MD5 hashing
* Scenario - Webserver file integrity
* Scenario - Crummy links

<!SLIDE bullets incremental transition=fade>
## Begin, Rescue, Ensure, End 
is very nice for scripts you are likely to kill mid-run. 
* Scenario - Exercise hardware infinitely
* Scenario - Services

<!SLIDE bullets incremental transition=fade>
## File.open() {} 
prevents leaving files hanging around open and taking up memory.
* What I always used to do:
* What I do now:

<!SLIDE bullets incremental transition=fade>
## Extra
 I have been working on a project based on some of this code which is a tripwire-alike. I could talk about that for some time if you need to fill more time. There are lots of lessons (AKA mistakes) in that code :)  As you may have guessed I like talking so you can stow this away for rainy day too.
