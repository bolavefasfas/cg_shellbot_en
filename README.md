fclone shell bot FAQ
Note: Windows is not currently supported.

1. What is the difference between using rclone/gclone/fclone?
All are based on rclone, gclone adds sa switch, fclone optimizes the use of multiple sa

In terms of speed, rclone and gclone are basically the same, fclone is much faster, whether it is several times faster, tens of times or hundreds of times, depending on the [number and structure of sa] [computer & VPS performance] [flag setting]

2. How fast is fclone? My sa is less, can I not experience this speed advantage if my VPS is poor?
According to the official instructions of rclone, the average speed of rclone and gclone is 2 files/s, while the minimum speed of fclone is 4-5 files/s, which is twice as fast as the guarantee!

As for the number of SAs and the performance of VPS, I am not an internal google staff member. I cannot give you a rigorous formula. I can only enumerate the situation of some friends in the internal test group:

Serial number sa quantity sa structure vps cpu vps memory transfer parameter—checker transfer parameter—transfer transfer target situation speed
01 400 100 sa/project E3 1C 512M 32 32 479T 10M or more files 50 files/s
02 2400 100 sa/project R9 1C 1G 128 128 479T 10M or more files 98 files/s
03 5000 20 sa/project R9 1C 1G 256 256 479T 10M or more files 160 files/s
04 5000 10 sa/project R9 1C 1G 320 326 479T 10M or more files 200 files/s
05 15000 100 sa/project 2C 4G 1000 2500 479T 10M or more files 350 files/s
05 20000 100 sa/project 2C 4G 3000 3000 479T 10M or more files 600 files/s
Note: The maximum ratio of 100sa/proj, sa and checker transfers is 10:1, and it is recommended to copy a large number of files at 20:1, that is, if there is 2000sa, checker transfer is not more than 100! Pacerburst 5000 The consequences of not listening to persuasion are: slow down the speed | missing files | redundant files Suggestion: sa structure 10 sa/project, sa number: 10000~15000

3. Is it necessary for the clone series transfer tool to set up a client id for personal use?
>This problem is actually quite troublesome. >Use your own client id, low concurrency; >Use the default rclone public client id, high concurrency, but if more than N people use it, there may be traffic jams; >The official explanation is this-original address: https://rclone.org/drive/#making-your-own-client-id ``` --drive-client-id It is recommended that you set your own Google Application Client ID. For how to create your own example, see https://rclone.org/drive/#making-your-own-client-id. **If you leave this blank blank, it will use the low-performance internal key** ``` According to the official statement of rclone, it is recommended to use your own, use it and the public one, and it can't stand it!
4. The magic fclone command?
rclone version-display the version number

rclone listremotes- lists all remote user names in the configuration file

rclone tree-List the contents of the remote control in file tree format

rclone ls-list objects in path with size and path

rclone lsd-list all directories in the path

rclone lsf-list remote: path used for parsing directories and objects

rclone lsjson-List directories and objects in the path in JSON format

rclone move-move files from source to target

rclone copy-copy files from source to dest, skip copied files

rclone sync-make the source and target the same, only modify the target

rclone size-print remote: the total size and number of objects in the path

rclone check-check if the files in the source and target match

rclone dedupe-interactively find duplicate files and delete/rename them

rclone delete-delete the contents of the path

rclone rmdirs-delete empty directories in the path

rclone mount-mount the remote as a file system on the mount point

5. fclone parameters-speed
``` --drive-server-side-across-configs --stats=1s --stats-one-line -P --ignore-checksum --checkers=1800 --transfers=1800 --drive-pacer-min -sleep=1ms --drive-pacer-burst=3000 --check-first --log-level=DEBUG --log-file=/root/fclone_debug.log ```
--drive-server-side-across-configs allows server-side operations (for example, replication) to work across different drive configurations. Please note that this feature is not enabled by default.

--drive-pacer-min-sleep=1ms minimum sleep time between API calls

--drive-pacer-burst=xxx Allows the number of API calls that do not sleep, pay attention not to fully open, otherwise cycle erro, it is recommended to open the number=sa*25%

--checkers=1800 --transfers=1800 fclone gearbox, check and transfer threads, recommended thread number=sa number/20 (provided that the vps performance can be supported)

--check-first fclone is fast, the default is no check first, without this tag, fclone=gclone=rclone

--disable ListR Turn off the default fast list, avoid the bug prompt of listr, and the whole world is clean

--ignore-checksum

When to use/not use --no-traverse: Suppose your destination has 6 files {a, b, c, d, e, f}.

If {a} is copied to the destination, there is no traversal, rclone will load in the definitions of all files {a, b, c, d, e, f}, and then find out whether it is necessary to upload {a}. If you use --no-traverse, rclone will only check {a} on the remote.

So, why not always use --no-traverse?

If you want to copy {a, b, c, d, e, f} to the target location, rclone will check each file individually. This will require at least 6 transactions, and you may have completed a list of all objects in 1 list.

So there is a trade-off! The new synchronization method implemented in version 1.36 makes --no-traverse less usable than before, but it still comes in handy, especially when moving or copying files into a deeper hierarchy.

How to run on a micro instance RClone on a micro instance with less than one gigabyte of memory may crash. You can do the following:

Type export GOGC=20 before running rclone. Remove --fast-list and reduce --transfers=

6. fclone parameters-functions
Rclone optimization

7. Deal with [debian/ubuntu] python3 version issues
Copy line by line regardless of your previous version
1. Upgrade and installation

apt update -y
apt upgrade -y
apt install python3 python3-pip --upgrade
pip3 install --upgrade pip
2. View version

python3 --version
pip3 --version
If you find that python3 is not the version that prompted you to install successfully, it may be that there is an old python3 in your system, execute the following command to confirm the existence of python3 version

For example, 3.7.7 you just treat it as 3.7 and ignore it after the second decimal point

whereis python3

3. Enable the python version

rm -rf /usr/bin/python3
ln -s `which python3.x` /usr/bin/python3
The x in 3.x above is the version number that you can confirm at the end of the second step, and only retain 1 decimal place.

Check the python3 version number again

python3 --version

8. Deal with the problem of too many open files
####step 1)

nano /etc/sysctl.conf

Add the following line

fs.file-max = 6553500

Save and exit execute the following command

sysctl -p

####step 2)

nano /etc/security/limits.conf

Add the following line

* soft memlock unlimited
* hard memlock unlimited
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535

root soft memlock unlimited
root hard memlock unlimited
root soft nofile 65535
root hard nofile 65535
root soft nproc 65535
root hard nproc 65535
Save and exit

step 3)

nano /etc/pam.d/common-session

Add the following line

session required pam_limits.so

Save and exit, and finally restart the system to log in to view

ulimit -a

fclone shell bot FAQ
Note: Windows is not currently supported.

1. What is the difference between using rclone/gclone/fclone?
All are based on rclone, gclone adds sa switch, fclone optimizes the use of multiple sa

In terms of speed, rclone and gclone are basically the same, fclone is much faster, whether it is several times faster, tens of times or hundreds of times, depending on the [number and structure of sa] [computer & VPS performance] [flag setting]

2. How fast is fclone? My sa is less, can I not experience this speed advantage if my VPS is poor?
According to the official instructions of rclone, the average speed of rclone and gclone is 2 files/s, while the minimum speed of fclone is 4-5 files/s, which is twice as fast as the guarantee!

As for the number of SAs and the performance of VPS, I am not an internal google staff member. I cannot give you a rigorous formula. I can only enumerate the situation of some friends in the internal test group:

Serial number sa quantity sa structure vps cpu vps memory transfer parameter—checker transfer parameter—transfer transfer target situation speed
01 400 100 sa/project E3 1C 512M 32 32 479T 10M or more files 50 files/s
02 2400 100 sa/project R9 1C 1G 128 128 479T 10M or more files 98 files/s
03 5000 20 sa/project R9 1C 1G 256 256 479T 10M or more files 160 files/s
04 5000 10 sa/project R9 1C 1G 320 326 479T 10M or more files 200 files/s
05 15000 100 sa/project 2C 4G 1000 2500 479T 10M or more files 350 files/s
05 20000 100 sa/project 2C 4G 3000 3000 479T 10M or more files 600 files/s
Note: The maximum ratio of 100sa/proj, sa and checker transfers is 10:1, and it is recommended to copy a large number of files at 20:1, that is, if there is 2000sa, checker transfer is not more than 100! Pacerburst 5000 The consequences of not listening to persuasion are: slow down the speed | missing files | redundant files Suggestion: sa structure 10 sa/project, sa number: 10000~15000

3. Is it necessary for the clone series transfer tool to set up a client id for personal use?
>This problem is actually quite troublesome. >Use your own client id, low concurrency; >Use the default rclone public client id, high concurrency, but if more than N people use it, there may be traffic jams; >The official explanation is this-original address: https://rclone.org/drive/#making-your-own-client-id ``` --drive-client-id It is recommended that you set your own Google Application Client ID. For how to create your own example, see https://rclone.org/drive/#making-your-own-client-id. **If you leave this blank blank, it will use the low-performance internal key** ``` According to the official statement of rclone, it is recommended to use your own, use it and the public one, and it can't stand it!
4. The magic fclone command?
rclone version-display the version number

rclone listremotes- lists all remote user names in the configuration file

rclone tree-List the contents of the remote control in file tree format

rclone ls-list objects in path with size and path

rclone lsd-list all directories in the path

rclone lsf-list remote: path used for parsing directories and objects

rclone lsjson-List directories and objects in the path in JSON format

rclone move-move files from source to target

rclone copy-copy files from source to dest, skip copied files

rclone sync-make the source and target the same, only modify the target

rclone size-print remote: the total size and number of objects in the path

rclone check-check if the files in the source and target match

rclone dedupe-interactively find duplicate files and delete/rename them

rclone delete-delete the contents of the path

rclone rmdirs-delete empty directories in the path

rclone mount-mount the remote as a file system on the mount point

5. fclone parameters-speed
``` --drive-server-side-across-configs --stats=1s --stats-one-line -P --ignore-checksum --checkers=1800 --transfers=1800 --drive-pacer-min -sleep=1ms --drive-pacer-burst=3000 --check-first --log-level=DEBUG --log-file=/root/fclone_debug.log ```
--drive-server-side-across-configs allows server-side operations (for example, replication) to work across different drive configurations. Please note that this feature is not enabled by default.

--drive-pacer-min-sleep=1ms minimum sleep time between API calls

--drive-pacer-burst=xxx Allows the number of API calls that do not sleep, pay attention not to fully open, otherwise cycle erro, it is recommended to open the number=sa*25%

--checkers=1800 --transfers=1800 fclone gearbox, check and transfer threads, recommended thread number=sa number/20 (provided that the vps performance can be supported)

--check-first fclone is fast, the default is no check first, without this tag, fclone=gclone=rclone

--disable ListR Turn off the default fast list, avoid the bug prompt of listr, and the whole world is clean

--ignore-checksum

When to use/not use --no-traverse: Suppose your destination has 6 files {a, b, c, d, e, f}.

If {a} is copied to the destination, there is no traversal, rclone will load in the definitions of all files {a, b, c, d, e, f}, and then find out whether it is necessary to upload {a}. If you use --no-traverse, rclone will only check {a} on the remote.

So, why not always use --no-traverse?

If you want to copy {a, b, c, d, e, f} to the target location, rclone will check each file individually. This will require at least 6 transactions, and you may have completed a list of all objects in 1 list.

So there is a trade-off! The new synchronization method implemented in version 1.36 makes --no-traverse less usable than before, but it still comes in handy, especially when moving or copying files into a deeper hierarchy.

How to run on a micro instance RClone on a micro instance with less than one gigabyte of memory may crash. You can do the following:

Type export GOGC=20 before running rclone. Remove --fast-list and reduce --transfers=

6. fclone parameters-functions
Rclone optimization

7. Deal with [debian/ubuntu] python3 version issues
Copy line by line regardless of your previous version
1. Upgrade and installation

apt update -y
apt upgrade -y
apt install python3 python3-pip --upgrade
pip3 install --upgrade pip
2. View version

python3 --version
pip3 --version
If you find that python3 is not the version that prompted you to install successfully, it may be that there is an old python3 in your system, execute the following command to confirm the existence of python3 version

For example, 3.7.7 you just treat it as 3.7 and ignore it after the second decimal point

whereis python3

3. Enable the python version

rm -rf /usr/bin/python3
ln -s `which python3.x` /usr/bin/python3
The x in 3.x above is the version number that you can confirm at the end of the second step, and only retain 1 decimal place.

Check the python3 version number again

python3 --version

8. Deal with the problem of too many open files
####step 1)

nano /etc/sysctl.conf

Add the following line

fs.file-max = 6553500

Save and exit execute the following command

sysctl -p

####step 2)

nano /etc/security/limits.conf

Add the following line

* soft memlock unlimited
* hard memlock unlimited
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535

root soft memlock unlimited
root hard memlock unlimited
root soft nofile 65535
root hard nofile 65535
root soft nproc 65535
root hard nproc 65535
Save and exit

step 3)

nano /etc/pam.d/common-session

Add the following line

session required pam_limits.so

Save and exit, and finally restart the system to log in to view

ulimit -a

fclone shell bot v1.0
Shellbot can mobilize and run VPS commands on TG. This script is only a google drive dump application of shellbot. Of course, the dump tool is very important, fclone, 400 fils/s, yes, speed is on file, although there are others Advantages, but a speed, can already be worthy of its name fxxk clone, world martial arts, in order not to break fast, you use fclone, other clones can only see your back.



Note: Windows is not currently supported.

installation steps:
Step 1: Operating environment (Ubuntu/Debian)
1. Make sure that you have installed python3.6+, and run the following commands in sequence, because I don’t know what shellbot needs, so I will tell you everything I installed and pay attention to the error message:

pip3 install pipenv
pip3 install delegator.py
pip3 install python-telegram-bot
pip3 install pysocks
2. Install node-pty dependencies.

sudo apt install nodejs
sudo apt install -y make python build-essential
Step 2: Clone the library
git clone https://github.com/cgkings/gclone_shell_bot.git && cd /root/gclone_shell_bot
npm install
Step 3: Start the bot
Start bot

node server
Automatic start

1. After starting, you may want the bot to start automatically when the system starts, and regenerate when it crashes. To do this, you can run it:

sudo npm install -g forever
2. Then, from your /etc/rc.local script or initialization script, call:

forever start /path/to/shell-bot/server.js
Step 4: Configure bot
1. Get the token and user id of Telegram bot

Use Telegram's botfather to build a bot that belongs to you and get bot token

Use user id to get bot, get your own user ID

Copy the above information for use

2. When you run it for the first time, it will ask you some questions and automatically create a configuration file: config.json. You can also write it manually, see config.example.json.
After starting, it will display a message when Bot ready is started and running. For convenience, you may need to talk to BotFather and set the command list to the content commands.txt.

Step Five: Install fclone
fclone publish address page
One-click installation command:

wget https://raw.githubusercontent.com/cgkings/fclone_shell_bot/master/fclone/fclone.zip && unzip fclone.zip && mv fclone /usr/bin && chmod +x /usr/bin/fclone && fclone version
The authors are @fxxkrlab (F guy) and @Ip2N5M (K guy) on TG, they are very enthusiastic people, Xiaobai’s gospel, welcome everyone to harass them on TG, they are very eager for your Xiaobai questions!

Advantages of fclone? In fact, there is no advantage, it is dozens of times faster than all existing transfer tools, the speed is shown in the following figure:



This picture is of @asuka8 on the stolen TG, a famous quick shooter in the internal beta group!

Speed ​​graph

This is my own speed chart, 512M VPS performance is not good

Regarding fclone, in addition to asking F and K, you can also add @asuka8 and @waihoe89, they are very enthusiastic!

In addition, I would like to give a grand introduction to @Komeanx (Jason Wu) on TG. The avatar is often changed, and the name has not been changed. The famous little white profiteer in the Chinese circle of TG (now quit), enthusiastically helped me build gclone for free. The path of no return to the transfer script (inaccurate, in fact, it started from the sales of Huang Ass to me the pheasant university education account, in fact, there is no need to buy it at all, there are a lot of free applications for community colleges in the United States). . .

Step 6: Install fclone one-click dump script

Low profile (128 256 5000)

sh -c "$(curl -fsSL https://raw.githubusercontent.com/cgkings/fclone_shell_bot/master/script/fcloneinstall.sh)"

High configuration (256 400 10000)

sh -c "$(curl -fsSL https://raw.githubusercontent.com/cgkings/fclone_shell_bot/master/script/fclone_high/fcloneinstall.sh)"

Script configuration tutorial

When you are familiar with it, you should be able to modify the script according to your needs. If you have any questions, TG finds @onekings. He has gone further and further (crooked) on the road of customization of this script. If you are not there, the rising little white god TG asked him a few questions, but he didn't respect him!

Step 7: Use the dump bot
1. Enter /fc to the bot

Note: You can also find @BotFather in TG, enter /setcommands to define the command list, so you can click "/" on the dump bot and select "/fc"

2. The bot pops up the message "Please enter your sharing link", and reply to the sharing link you want to transfer in this message

The rest can be operated as shown in the figure. Pay attention, so the content that needs to be input must be valid after replying to the original information with the symbol "🔸"

power of attorney
When launched for the first time, the bot will only accept messages from your users. For security reasons: you don't want anyone to issue commands to your computer!
If you want to allow other users to use the bot, please use /token and provide a link to the result. If you want to use this bot on the online forum, /token will send you a message to be forwarded to the online forum

Final words
Sending you a thousand words, there will always be a difference. As a little white, I can publish shamelessly on github because of the open development atmosphere of github, and also because of the selfless help of open and enthusiastic Chinese technology bigwigs on TG. I would like to thank you all TG great gods, in no particular order:

fxxkrlab (professional adventurer) I hope that we can learn more languages ​​and compile the transfer bot textbook according to our needs. Unfortunately, it has not been researched yet.
aevlp (steve x), the originator of the dumping script, selflessly provided the dumping bot that uses mysql to realize the dumping task sequence. Unfortunately, it has not been researched yet.
Ip2N5M (Kali Aska) Little white gospel, no textbooks, no cases, answers and tools directly, thank him for the script core code and the modified version of gclone, the magic modified rclone gclone, experience it
shine_y (shine) The original author of the one-click dump script I modified, thank you very much, address
Onekingen (oneking) is a small partner on the road to the magic change of scripts. I have done a lot of custom scripts. Address
GreatPanoan (Panoan) took me to the guide who bought the chicken and did not return. Without him, I would not start this tossing journey. Anyway, I wish the college entrance examination a success, kid!
In addition, hrvstr on github, he provides a template of custom commands on shellbot, thank him very much
Of course, Botgram, the original author of shellbot, is indispensable
Finally, if you are a foreign friend, it is an honor, Sun Thief, use Google Translate!
