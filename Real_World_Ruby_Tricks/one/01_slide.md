This is a static copy of the presentation. Showoff is a bit challenging to get actually working ATM so you might just want to make do with this.

To try the show off route:

    git clone https://github.com/robie1373/RWRT
    cd RWRT/
    bundle install
    cd Real_World_Ruby_Tricks/
    showoff serve

Then just point your browser at http://localhost:9090 If everything worked you will see the preso. 
If you 

    export SHOWOFFEVALRUBY=1

you might even be able to click on some of the code samples and see the output.


<!SLIDE center incremental transition=fade>
# Real world Ruby tricks.

*No plan survives engagement with the enemy and no script survives engagement with the environment. I'll show a couple of Ruby-isms that I've found make my scripts more robust, safer, or more useful in messy situations.*

<!SLIDE code incremental small transition=fade>
## File.open() {}
###What I always used to do:
    @@@ ruby
    Dir.chdir '/tmp'
    2.times do
        file = File.open("File.txt", 'a')
        file.write "drat"
        # file.close (I always forget this part)
    end
###Yields:
    @@@
    $ lsof /tmp/File.txt
    COMMAND   PID      USER   FD   TYPE DEVICE SIZE/OFF     NODE NAME
    ruby    29609 robie1373    8w   REG    1,4        0 53522465 /private/tmp/File.txt
    ruby    29609 robie1373    9w   REG    1,4        0 53522465 /private/tmp/File.txt

<!SLIDE code small execute incremental transition=fade>
## File.open() {}
###What I do now:

    @@@ ruby
    Dir.chdir '/tmp'

    %w{blocks are closures and close the file for me!}.
    each do |word|
        File.open("afile.txt", 'a+') do |f|
            f.write "#{word} "
        end
    end
    File.read "afile.txt"

<!SLIDE code execute transition=fade>
## Tempfile library
###Scenario - Respecting your users' ~ directory

    @@@ ruby
    require 'tempfile'

    temp = Tempfile.new(['rwrt_', '.txt'])
    File.absolute_path temp

<!SLIDE code execute small transition=fade>
## Tempfile library 
###Scenario - Download data from Internet for future use

    @@@ ruby
    def get_all_the_twitters
        "lots of tweets"
    end

    temp = Tempfile.new('rwrt_')
    File.open(temp, 'w') do |f|
        f.write get_all_the_twitters
    end
    p "#{File.read temp} in #{File.absolute_path temp}"

<!SLIDE code execute small incremental transition=fade>
## SHA and/or MD5 hashing
###Scenario - Webserver file integrity

    @@@ ruby
    require 'digest/md5'
    temp = Tempfile.new('rwrt_')
    hash1 = Digest::MD5.hexdigest(File.read(temp))
    File.open(temp, 'a') { |f| f.write "Evil!" }
    hash2 = Digest::MD5.hexdigest(File.read(temp))
    hash1 == hash2

<!SLIDE code small transition=fade>
## SHA and/or MD5 hashing
###Scenario - Crummy links

    @@@ ruby
    data_from_Pottsylvania = ["2GB file", "md5sum"]
    # wget data_from_Pottsylvania
    unless Digest::MD5.hexdigest(
        data_from_Pottsylvania[0]) ==
        data_from_Pottsylvania[1]
        # wget data_from_Pottsylvania
    end


<!SLIDE code incremental transition=fade>
## Begin, Rescue, Ensure, End
###Scenario - Exercise hardware infinitely
Messy

    @@@ ruby
    Dir.chdir("/Volumes/new_drive")
    (0..1_000_000).each do |i|
      File.open("junk#{i}.junk", 'w') do |f|
        f.write "#{"junk" * 1_000_000}"
      end
      Digest::MD5.hexdigest(File.read(file))
    end

<!SLIDE code small incremental transition=fade>
## Begin, Rescue, Ensure, End
###Scenario - Exercise hardware infinitely
Neat

    @@@ ruby
    begin
        Dir.chdir("/Volumes/new_drive")
        (0..1_000_000).each do |i|
          File.open("junk#{i}.junk", 'w') do |f|
            f.write "#{"junk" * 1_000_000}"
          end
          Digest::MD5.hexdigest(File.read(file))
        end
    rescue SystemExit, Interrupt
        puts "\nExiting script."
        # rm -rf /Volumes/new_drive/junk*.junk
    ensure
        # rm -rf /Volumes/new_drive/junk*.junk
    end

<!SLIDE code small incremental transition=fade>
## Begin, Rescue, Ensure, End
###Scenario - Services

    @@@ ruby
    class Logger
      def double_log
        begin
          # write to remote
        ensure
          # write to local
        end
      end
    end

<!SLIDE center transition=fade>
Nothing is new anymore. Everything in these slides came from somewhere else. I'm just not smart enough to remember who or where.

Avdi Grimm did a Ruby Tapa on Tempfiles recently which is much better. You should check them out anyway.

The Ruby community is full of excellent people and ideas. Thanks to all.

<!SLIDE center transition=fade>
## Thanks for listening!

robie1373@gmail.com

@robie1373

https://github.com/robie1373
