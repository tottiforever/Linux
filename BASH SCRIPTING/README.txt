#start your script with the line #!/bin/bash

#an exit statement if you don't want to use the exit status of the last command in the script.
#“0” means the program has executed successfully.
#“1” means the program has minnor errors.

#echo $? >> shows the exit status of last command

#script_example_1:

#!/bin/bash
#
# This is a demo sc ript that greets the world.
# Usage: ./hello

clear
echo hello world

exit 0

#consider storing the script in a directory that is in $PATH, sucha as /usr/local/bin or $USER/bin
#in orde to run the script, it must have the executed permission applied:
chmod +x myscript

echo $PATH

#Intenal command does not have to be loaded from disk and therefore is faster while external command is a command that is loaded from an executable file on disk, so they are slower.
#use help to get a list of internal commands.
#internal commands always go before external commands.

man bash
help command

#script_example_2:
#create a script that copeis the contents of the log file /var/log/messages to /var/log/messages.old and deletes all contests of the /var/log/messages file:

#!/bin/bash
#This script copies /var/log contents and clears current contents of the file
#Usage: ./clearlogs

cp /var/log/messages /var/log/messages.old
cat /dev/null > /var/log/messages
echo log file copeid and cleaned up

exit 0

or

#!/bin/bash
#This script copies /var/log contents and clears current contents of the file
#Usage: ./clearlogs

cp /var/log/syslog /var/log/syslog.old
cat /dev/null > /var/log/syslog
echo log file copeid and cleaned up

exit 0


#>/dev/null it is a special device which discards the information written to it. it immediately discards anything written to it and only returns an end-of-file EOF when read.
echo 'text' > /dev/null

#an argument is anything that can be used after a command.
ls -l /etc has two arguments
#an option is an argument that was specifically developed to change the command behavior.
#a parameter is a name that is defined in a script to which a specific value is granted.
#a variable is a label that is stored in memory and contains a specific value.
#it is recommended to put all variables together in the beginning of script.

#using variables:
#script_example_3:

#!/bin/bash
#This script copies /var/log contents and clears current contents of the file
#Usage: ./clearlogs

LOGFILE=/var/log/syslog

cp $LOGFILE $LOGFILE.old
cat /dev/null > $LOGFILE
echo log file copeid and cleaned up

exit 0

#it is recommended to use uppercase for variable names.
#there are three ways to define variables:
#METHOD1:static: VARNAME=value
#METHOD2:as an argument to a script, handled using $1, $2 etc. within the script.
#METHOD3:interactively, using read.

#METHOD1:
COLOR=blue
echo $COLOR

#to remove variable, terminal should be reopened or we can use word 'bash' to clear variables. (bash word creates a subshell)
#to persistent variables in subshell we should use export.

export COLOR=red
echo $COLOR

#METHOD3:
#script_example_4

#!/bin/bash
#
#Ask what to stop using the kill command and then kill it

echo which process do you want to kill?
read TOKILL

kill $(ps aux | grep $TOKILL | grep -v grep | awk '{ print $2 }')


#awk '{print $2}' >> print the sencond column
#grep -v grep >> exclude grep from result
#to run script step by step we can use:
bash -x scriptname

#read will stop the script.
#if and argument is provided to read, this argument will be treated as a variable and the entered value is stored in the variable.
#multiple arguments can be provided to enter multiple variables.
#use read -a somename to write all words to an array with the name somename.
#if no input is provided, the script will just pause until the user presses the Enter key.

kill $(ps aux | grep http | grep -v grep | awk '{ print $2 }')

#a variable is effective only in the shell where it was defined.
#use export to make it also available in subshells.
#there is no way to make variables available in parent shells.

#script_example_5

#!/bin/bash
#
#showing variable use between shells

echo which directory do you want to activate?
read DIR

cd $DIR
pwd
ls

exit 0

#/etc/profile is processed when opening a login shell.
#all variables defined here are available in all subshells for that specific user.
#user specific version can be used in ~/.bash_profile
#/etc/profile is read in automatically only if the shell is a login shell.
#etc/profile is global for all users.

#/etc/bashrc is processed when opening a subshell
#variables defined here are included in subshells from that point on.
#user specific version can be used in ~/.bashrc
#~/.bashrc is per user login where you can set up your favorite environment.
#/etc/bashrc or /etc/bash.bashrc is the systemwide functions and aliases. However, environment stuff goes in /etc/profile file.
#~/.bashrc is the individual per-interactive-shell startup file stored in your home directory $HOME. 

#sourcing
#by using sourcing the contents of one script can be included in another scripts
#use the source command or the . command to source scripts.
#do not use exit at the end of a script that needs to be sourced.

#bash shell characters:
~ home directory
` command substitution
# comment
$ variable expression
& background job
* string wildcard
( start subshell
) end subshell
\ quota next character
[ start character set wildcard
] end character set wildcard
{ start command block
} end command block
; shell command seperator
'' strong quota
< input
> output
? single character wildcard

#quoting:
#when a command is interpreted by the shell, the shell interprets all special characters >> command line parsing
#to ensure that a special character is interpreted by the command and not by the shell, use quoting.
#quoting is used to treat special characters literally.
#if a string of characters is surrounded with single quotation marks, all characters are stripped of the special meaning they may have.
#double quotas are weak quotas and treat only some special characters as special
#a backslash can be used to escape the one following character.

echo The value of the $PATH variable is $PATH
echo 'The value of the $PATH variable is $PATH'
echo "The value of the $PATH variable is $PATH"
echo "The value of the \$PATH variable is $PATH"
echo The value of the \$PATH variable is $PATH

echo 2 * 3 > 5
echo '2 * 3 > 5'

#double quotas ignore pipe characters, aliases, tilda substitution, wildcard expression, and splitting into words using delimiters.
#double quotas do allow parameter substitution, command substitution, and artithmetic expression evaluation.
#best practice:
#user single quotas, unless you specifically need parameter, command or artithmetic substitution.

#use $1, $2 and so on to refer to the first, the second, etc. argument.
#$0 refers to the name of the script itself.
#use ${nn} or shift to refer to arguments beyond 9.
#arguments that are stored in 1$ etc. are read only and cannot be changed from whihin the script.

#script_example_6

#!/bin/bash
#
#simple demo script with arguments
#run this script with the names of one or more people

echo "Hello $1 how are you"
echo "Hello $2 how are you"

exit 0

./script_example_6 name1 name2

#the previous example works only if the amount of arguments is known on beforehand
#if this is not the case, use for to evaluate all possible arguments.
#use $@ to refer to all arguments provided, where all arguments can be treated one by one. (means list of all provided arguments)****
#use $# to count the amount of arguments provided.
#use $* if you need a single string that contains all arguments (use in specific cases only)

#script_example_7:

#!/bin/bash
#script that shows how arguments are handled

echo "\$* gives $*"
echo "\$# gives $#"
echo "\$@ gives $@"
echo "\$0 is $0"

#showing the interpretataion of \$*
for i in "$*"
do
	echo $i
done

#showing the interpretataion of \$@
for j in "$@"
do
	echo $j
done
exit 0

./script_example_7 a b c

#for i in "$*" >> read it as: for each element in $*

#script_example_8:

#!/bin/bash
#script that shows how to make sure that user input is provided

if [ -z $1 ]
then
	echo provide filenames
	read FILENAMES
else
	FILENAMES="$@"
fi

echo the following filenames have been provided: $FILENAMES

#if [ -z $1 ] >> to test if $1 is empty or not
#shift:
#shift removes the first arguments from a list so that the second argument gets stored in $1
#shift is useful in order shell versions that do not understand ${10} etc.

#script_example_9:

#!/bin/bash
#
#simple demo script with arguments.
#run this script with the names of one or more people.

echo "Hello $1 how are you"
echo "Hello $2 how are you"
echo "Hello ${10} how are you"

exit 0

./script_example_9 a b c d e f g h i j

script_example_10:

#!/bin/bash
#showing the use of shift

echo "The arguments provided are $*"
echo "The value of \$1 is $1"
shift
echo "The new value of \$1 is $1"

exit 0

or

#!/bin/bash
#showing the use of shift

echo "The arguments provided are $*"
echo "The value of \$1 is $1"
echo "The entire string is $*"
shift
echo "The new value of \$1 is $1"
echo "The entire string now is $*"

exit 0

or

#!/bin/bash
#provide 10 arguments or more when running this script

echo "Hello $1"
echo "Hello $2"
echo "Hello $10"
echo "Hello ${10}"

exit 0

#command substitution allows using the result of a command in a script
#two allowed syntaxes:
`command` (deprecated)
$(command) (preffered)
ls -l $(which passwd)


#script_example_11:

#!/bin/bash
#This script copies /var/log contents and clears current contents of the file
#Usage: ./clearlogs

cp /var/log/messages /var/log/messages.$(date +%d-%H:%M:%S)
cat /dev/null > /var/log/messages
echo log file copeid and cleaned up

exit 0

date +%d-%m-%y
date +%d-%H:%M:%S
touch ~/file-$(date +%d-%m-%y)

#string verification
#when working with arguments and input, it is useful to be able to verify availablity and correct use of a string
#use test -z to ckeck if a string is empty
test -z $1 && exit 1

#use [[ ... ]] to check for specific patterns
[[ $1=='[a-z]*' ]] || echo $1 does not start with a letter

# || means logical or

#here document
#I/O redirection is used to feed a command list into an interactive program or command, such as for instance ftp or cat
#use it in scripts to replace echo for long texts that need to be displayed.


script_example_12:

#!/bin/bash
#script that shows how here documents can be used 
#as an alternative to the echo command

cat <<EndOfMessage
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

This is line 1 of the message
This is line 2 of the message
This is the last line of the message
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

EndOfMessage

script_example_13:

#!/bin/bash
#script that shows how text to be sent by wall 
#is feeded through a here document

wall <<EndOfMessage

The menu for today is:
1. Healthy soup and salad
2. Chips and fish
3. Steak with carrots
EndOfMessage


script_example_14:

#!/bin/bash
#example of a scripted FTP session

lftp localhost <<EndOfMessage
ls
get hosts
bye
EndOfMessage

echo the file is now downloaded

#write a script with the name totmp. this script should copy all files which the names are provided as arguments on the command line to the users home directory. 
if no files have been provided, the script should use read to ask for file names, and copy all files names provided in the answer to the users home directory.

#!/bin/bash
#exercise_2

if [ -z $1 ]
then
	echo which file do you want to copy?
	read FILENAMES
else
	FILENAMES="$@"
fi

echo the following filenames have been provided: $FILENAMES
for i in $FILENAMES
do
	cp $i $HOME
done 

#substitution Operator
#${VAR:-word}: if $VAR exists, use its value, if not, return the value "word". this does NOT set the variable. (just print the word not set it as variable)
#${VAR:=word}: if $VAR exists, use its value, if not, set the default value to "word".
#${VAR:?message}: if $VAR exists, show its value. if not, display VAR followed by message. if message is omitted, the message VAR: parameter null or not set will be shown.
#${VAR:offset:length}: if $VAR exists, show the substring of $VAR starting at offset with a length of length.

DATE=
echo $DATE

echo ${DATE:-today}
today
echo $DATE
<empty>

echo ${DATE:=today}
today
echo $DATE
today

DATE=
echo ${DATE:?variable not set}
-bash: DATE: variable not set
echo $DATE

DATE=$(date +%d-%m-%y)
echo the day is ${DATE:0:2}
the day is 15

#pattern matching is used to remove paterns from a variable.
#it's an excellent way to clean up variables that have too much information.
#for example, if $DATE contains 05-01-19 and you just need today's year.
#or if a file has the extension *.doc and you want to rename it to use the extenstion *.txt
#the problem with pattern matching is that you cannot grep out something in the middle of pattern. so you need to go in two steps.

${VAR#pattern}: search for pattern from the beginning of variable's value, delete the shortest part that matches, and return the rest.
FILENAME=/usr/bin/blah
echo ${FILENAME#*/}
usr/bin/blah

${VAR#pattern}: search for pattern from the beginning of variable's value, delete the longest(last) part that matches, and return the rest.
FILENAME=/usr/bin/blah
echo ${FILENAME##*/}
blah

${VAR%pattern}: if pattern matches the end of the variable's value, delete the shortest part that matches, and return the rest.
FILENAME=/usr/bin/blah
echo ${FILENAME%/*}
/usr/bin

${VAR%pattern}: if pattern matches the end of the variable's value, delete the longest(last) part that matches, and return the rest.
FILENAME=/usr/bin/blah
echo ${FILENAME%%/*}

#!/bin/bash
BLAH=rababarabarabarara

echo BLAH is $BLAH
echo 'The result of ##*ba is' ${BLAH##*ba}
echo 'The result of #*ba is' ${BLAH#*ba}
echo 'The result of %%ba* is' ${BLAH%%ba*}
echo 'The result of %ba* is' ${BLAH%ba*}

#regular expressions are search patterns that can be used by some utilities (grep and other text processing utilities, awk, sed)
#regular expressions are not the same as shell wildcards.
#when using regular expressions, put them between strong quotas so that the shell won't interpret them.

^test >> line starts with text
test$ >> line ends with text
. >> wildcards(matches any single character)
[abc],[a-c] >> matches a,b or c
* >> matches 0 to an infinite number of the previous character
\{2\} >> matches exactly two of the previous character
\{1,3\} >> match a minimum of one and a maximum of three of the previous character
colou?r >> match 0 or 1 of the previous character 

#bash calculating
#bash offers different wayt to calculate in a script.
#internal calculation: $(( 1 + 1 ))
#external calculation with let:

#!bin/bash
# $1 is the first number
# $2 is the Operator
# $3 is the second number
let x="$1 $2 $3"

echo $x

#external calculation with bc
#bc is developed as a calculator with its own shell interface
#it can deal with more than just integers.
#use bc in non-interactive mode:

#ot in a variable:
VAR=$(echo "scale=9; 10/3" | bc)


#write a script that puts the result of the command date +%d-%m-%y in a variable. use pattern matching on this variable to show 3 lines, displaying the date, month and year. so the result should look as follows:

The day is 05
The month is 01
The year is 15


#!/bin/bash
#exercise_3

echo the date is $(date +%d-%m-%y)

DATE=$(date +%d-%m-%y)
echo The day is ${DATE%%-*}
MONTH=${DATE%-*}
echo The month is ${MONTH#*-}
echo The year is ${DATE##*-}

#grep
grep -i:case insensitive
grep -v:exclude lines that match the pattern
grep -r:recursive
grep -e(egrep):matches more regular expressions >> grep -e 'what' -e 'else' *
grep -A5:shows 5 lines after the matching regex
grep -b4:shows 4 lines before the matching regex

#example:
grep -i -e date -e year *

#test
#test allows for teting of many items
#expressions: test (ls /etc/hosts)
#string: test -z $1
#integers: test $1 = 6
#file comparisons: test file1 -nt file2
#file properties: test -x file1
#test -z $1: old method, using an internal bash command
#[-z $1]: eqquivalent to test, using a bash internal
#[[-z $1]]: new improved version of [...]. not as universal as [...]; it has && and || built in.
#best practice: if it doesn't work using [...] try using [[ ...]]

#cut is used to filter a specific column or field out of a line
#sort is used to sort data in a specific order
#cut and sort are often seen together
cut -f 1 -d : /etc/passwd
sort /etc/passwd
cut -f 2 -d : /etc/passwd | sort -n
du -h | sort -rn (r for reverse)
du -h | sort -rn | head
sort -n -k3 -t : /etc/passwd (t for delimiter, k for column)
sort -n -r -k3 -t : /etc/passwd | head

#head and tail
head -5 /etc/passwd | tail -1

#sed
sed -n 5p /etc/passwd (line 5 printed)
sed -i s/old-text/new-text/g ~/myfile
sed -i -e '2d' ~/myfile ( i for interactively, e for edit, 2d delete line number 2)
sed -i -e '2d;20,25d' `/myfile ( delete line number 20 till 25)

#awk
#advances filtering utility.
#in scripts you'll appreciate it as an alternative to cut to filter information from text files based on regular expression-based patterns.
# awk '/search pattern/ {action}' file
awk -F: '{print $4}' /etc/passwd
awk -F: '/user/ {print $4}' /etc/passwd
awk -F: '{print $1,$NF}' /etc/passwd ($1 is the first field, $NF is the last field)
awk -F: '$3 > 500' /etc/passwd (print any line that has value more than 500 in field 3)
awk	-F: '$NF ~/bash/' /etc/passwd

#tr <translate>
echo hello | tr [a-z] [A-Z]
echo hello | tr [:lower:] [:upper:]

#exercise_4
#create a script that transforms the string cn=lara,dc=example,dc=com in a way that the user name (lara) is extracted from the string.
#also make sure that the result is written in all uppercase. store the username in the variable USER and at the end of the script, echo the value of this variable.

#!/bim/bash
#exercise_4

USER=cn=lara,dc=example,dc=com
USER=${USER%%,*}
USER=${USER#*=}
USER=$(echo $USER | tr [:lower:] [:upper:])
echo the username is $USER

#if then fi
used to perform actions based on a specific expression

if expression
then
	command1
	command2
fi

#else can be included to define what happens if expression is not true

if expression
then
	command1
else
	command2
fi

#elif is used to nest a new statement within the if statement

if expression
then
	command1
elif expression 2
then
	command12
fi

if [ -d $1 ] >> means if $1 is directory
if [ -f $1 ] >> means if $1 is file

#script_example_12

#!/bin/bash
if [ -d $1 ]
then
	echo $1 is a directory
elif [ -f $1 ]
then
	echo $1 is a file
else
	echo $1 is not a file, nor a directory
fi


#using && and ||
#are the logical AND and OR
#use them as a short notation for if ... then ... fi constructions.
#when using &&, the second command is executed only if the first returns an exit code zero.

[-z $1] && echo $1 is not defined

#when using ||, the second command is executed only if the first command does not return an exit code 0

[-f $1 ] || echo $1 is not a file

#!/bin/bash
[ -z $1 ] && echo no argument provided $$ exit 2
[ -f $1 ] && echo $1 is a file && exit 0
[ -d $1 ] && echo $1 is a directory && exit 0


#script_example_13

#!/bin/bash
[ -d $1 ] && echo $1 is a directory
[ -f $1 ] && echo $1 is a file || echo $1 is not a file, nor a directory

#for
#for statements are useful to evaluate a range or series

for i in something
do
	command1
	command2
done

for i in `cat /etc/hosts`; do echo $i; done
for i in {1..5}; do echo $i; done

#it is common to use a variable i in a for loop, but any other variablecan be used instead.

vim users (put some user name in this file per each line)
for i in `cat users`; do echo $i; done
for i in `cat users`; do useradd $i; done

for i in {200..212}; do ping -c 192.168.1.$i; done
for i in {200..212}; do ping -c 192.168.1.$i >/dev/null && echo 192.168.1.$1 is available; done


or

for i in {200..212}
do
	ping -c 192.168.1.$i
done;

#script_example_14

#!/bin/bash
#script that counts files
echo which directory do you want to count?
read DIR
cd $DIR
COUNTER=0

for i in *
do
	COUNTER=$((COUNTER+1))
	echo I have counted $COUNTER files in this directory
done


for i in * >> means all contents of that directory

#case
#case is used if specific values are expected
#the most common example is in the legacy system V / Upstart init scripts in /etc/init.d

#script_example_15

#!/bin/bash
VAR=$1
case $VAR in
yes)
	echo ok;;
no|nee)
	echo too bad;;
*)
	echo try again;;
esac


#while is used to execute commands as long as a condition is true
#until is used to execute commands as long as a condition is false

while|until condition
do
	command
done


#!/bin/bash
COUNTER=0

while true
do
	sleep 1
	COUNTER=$(( COUNTER + 1 ))
	echo $COUNTER seconds have passed since starting this script
done


#!/bin/bash
until users | grep $1 > /dev/null
do
	echo $1 is not logged in yet
	sleep 5
done

echo $1 has just logged in
mail -s "$1 has just logged in" root < .

#a customer has exported a long list of LDAP user names. these usernames are stored in the file ldapusers.
#in this file, every user has a name in the format cn=lisa,dc=example,dc=com
#write a script that extracts the username only (lisa) from all of these lines and write those to a new file. based on this new file, create a local user account on your linux box.
#note: while testing its not a really smart idea to create the user accounts directly. find a solution that proves that the script works, without polluting your syste, with many usernames.

#!/bin/bash
#exercise_5

for i in $(cat ldapusers)
do
	USER=${i%%,*}
	USER=${USER#*=}
	echo $USER >> users
done

for j in $(cat users)
do
	useradd	$j
done

exit 0

#options
#an option is an argument that changes the script behavior
#options are not often used in scripts
#getopts is used to deal with options.

#!/bin/bash
#script that creates users using preferred options
#usage: use -a to add a home directory
#		use -b to make the user member of group 100
#		use -c to specify a custom shell. this option is followed by the shell name.

while getopts "abc:" opt
do
case  $opt in
	a ) VAR1=-m ;;
	b ) VAR2="-g 100";;
	c ) VAR3="-s $OPTARG";;
	* ) echo 'usage: makeuser [-a] [-b] [-c shell] username'
esac
done

echo the current arguments are set to $*
shift $((OPTION -1))
echo now the current arguments are set to $*
echo useradd $VAR1 $VAR2 $VAR3 $1
exit 0


#functions
#functions are useful if code is repeated frequently.
#functions must be defined before they can be used.
#it is good practice to define all functions at the beginning of a script.

#syntax1
function help
{
	echo this is how you do it
}

#syntax2
help ()
{
	echo this is how you do it
}

#!bin/bash
noarg ()
{
	echo you have not provided an argument
	echo when using this script, you need to specify a filename
	exit 2
}

if [ -z $1 ]; then
	noarg
else
	file $1
fi

exit 0

#an array is a string variable that can hold multiple values.
#although appreciated by some, using arrays can often be avoided because modern Bash can contain multiple values in a variable as well.
#the amount of values that can be kept in arrays is higher, which makes them useful is some cases anyway.
#some samples:
names=(linda lisa laura lori)
names[0]=linda
names[1]=lisa
names[2]=laura
names[3]=lori
echo ${names[2]}
echo ${names[@]} >> shows the entire value
echo ${#names[@]} >> # counts everything in the array
names [4]=lucy
echo ${names[@]}

#menu interface
#the select statement can be used to create a menu interface.
select DIR in /bin /usr /etc
#in this, DIR is the variable that will be filled with the selected choice, and /bin /usr /etc are presented as numbered menu options
#notice the use of break in select, without it the script would run forever.


#script_example_16

#!/bin/bash
#demo script that shows making menus with select

echo 'select a directory: '
select DIR in /bin /usr /etc
do
	#only continue if the user has selected something
	if [ -n $DIR ]
	then
		DIR=$DIR
		echo you have selected $DIR
		export DIR
		break
	else
		echo invalid choice
	fi
done

if [ -n $DIR ] >> if the string is nonzero (means it has value)


#script_example_17

#!/bin/bash
#sample administration menu

echo 'select a task: '
select TASK in 'check mounts' 'check disk space' 'check memory usage'
do
	case $REPLY in
			1) TASK=mount;;
			2) TASK="df -h";;
			3) TASK="free -m";;
			*) echo ERROR && exit 2;;
	esac
	if [ -n "$TASK:" ]
	then
			clear
			$TASK
			break
	else
			echo INVALID CHOICE && exit 3
	fi
done


#trap
#trap can be used to redefine signals
#useful to disallow ctrl-c or other ways of killing a script
#consult man 7 signal for a list of available signals

#!/bin/bash
#the uninterruptable script

trap "echo 'forget it bro!'" INT
trap "logger 'Who tried to kill me?'" KILL
trap "logger 'Getting nasty huh?'" TERM

while true
do
	true
done


#create a user helpdesk. write a menu script that is started automatically when this user og in.
#the menue script should never be terminated, unless the user logs out (whicj is a menu option as well).
#from this menue, make at least the following options available: (1) reset password, (2) show disk usage, (3) ping a host, (4) Log out
#bonus question: modify this script and related configuration so that user can use sudo to set passwords for other users as well.


#exercise_6
#!/bin/bash

while true
do
	trap "echo NOPE" INT
	pinghost ()
	{
		echo which host do you want to ping
		read HOSTNAME
		ping -c 1 $HOSTNAME
	}

	echo 'select option: '
	select TASK in 'change password' 'monitor disk space' 'ping a host' 'logout'
	do
		case $REPLY in
			1) TASK=passwd;;
			2) TASK="df -h";;
			3) TASK=pinghost;;
			4) TASK=exit;;
		esac

		if [ -n "TASK" ]
		then
			$TASK
			break
		else
			echo invalid choice #, try again
		fi
	done
done

cp exercise_6 /usr/local/bin/menu
useradd -s /usr/local/bin/menu helpdesk
passwd helpdesk


#analyzing tools
#use bash syntax highlighting available in editors
#in vim user :set list to show hidden characters
#in the script code, insert echo or head at critical points
#use bash -v to show verbos output (including error messages)
#use bash -n to check for syntax errors
#use bash -x to show xtrace information
#use the DEBUG trap to show debugging information for everything between setting it on and off


#!/bin/bash
#
#Define the network that is tested

echo On what network do you want to test? \(Please enter a 4 byte number\)
echo Press enter if you want to scan the local network
read NETWORK
[ -z $NETWORK ] && NETWORK=`ip a s | grep eth03 | head -n 3 | tail -n 1 | awk '{ print $2 }'` && \ NETWORK=${NETWORK%/*}
NETWORK=${NETWORK%.*}

#Remove old uplist file if existing
rm /tmp/uplist

#Run the program
for (( NODE=1; NODE<20; NODE++ )); do
	if ping -c 1 $NETWORK.$NODE
	then
	echo $NETWORK.$NODE >> /tmp/uplist
	fi
done

echo press enter to display the list 
read

cat /tmp/uplist
exit 0

##CPU Utilization script
#!/bin/bash
INTERVAL=$1

while sleep $INTERVAL
do
	USAGE1=`ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $1 }' `
	USAGE1=${USAGE1%.*}
	PID1=`ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $2 }' `
	PNAME1=`ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $2 }' `
	if [ $USAGE1 -gt 80 ]
	then
		sleep 7
		USAGE2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $1 }')
		USAGE2=${USAGE2%.*}
		PID2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $2 }')
		PNAME2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $3 }')
		[ $USAGE2 -gt 80 ] && [ $PID1 = $PID2 ] && mail -s "CPU load of $PNAME is above 80%" root < .
	fi
done



#!/bin/bash
INTERVAL=$1

while sleep $INTERVAL
do
	VALUE=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1)
	USAGE1=$(echo $VALUE | awk '{ print $1 }')
	USAGE1=${USAGE1%.*}
	PID1=$(echo $VALUE | awk '{ print $2 }')
	PNAME1=$(echo $VALUE | awk '{ print $3 }')

	if [ $USAGE1 -gt 80 ]
	then
		sleep 7
		USAGE2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $1 }')
		USAGE2=${USAGE2%.*}
		PID2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $2 }')
		PNAME2=$(ps -eo pcpu,pid -o comm= | sort -k1 -n -r | head -1 | awk '{ print $3 }')
		[ $USAGE2 -gt 80 ] && [ $PID1 = $PID2 ] && mail -s "CPU load of $PNAME is above 80%" root < .
	fi
done

#!/bin/bash

COUNTER=$1
COUNTER=$(( COUNTER * 60 ))

mineen () {
	COUNTER=$(( COUNTER - 1 ))
	sleep 1
}

while [ $COUNTER -gt 0 ]
do
	echo you still have $COUNTER seconds
	mineen
done

[ $COUNTER = 0 ] && echo out of time && mineen
[ $COUNTER = "-1" ] && echo you are one second late && mineen

while true
do
	echo you are ${COUNTER#*-} seconds late
	mineen
done

#!bin/bash
if [ -z $1 ]
then
	echo no argument provided
	exit 1
elif [ ! -e $1 ]
then
	echo $1 does not exist
	exit 2
elif [ -d $1 ]
then
	echo $1 is a directory
elif [ ! -f $1 ]
then
	echo $1 is a not a directory and not a file
elif [ -x $1 ]
then
	echo $1 is an executable file
elif grep '#!/bin/bash' $1
then
	echo $1 is a non-executable bash script
	chmod +x $1
else
	echo I don\'t know what this is
fi


or

[ -z $1 ] && echo no argument provided && exit 1
[ ! -e $1 ] && echo $1 does not exist && exit 2
[ -d $1 ] && echo $1 is a directory && exit
[ ! -f $1 ] && echo $1 is not a directory or file && exit
[ -x $1 ] && echo $1 is executable && exit
grep '#!/bin/bash' $1 && echo $1 is a non-executable bash script && chmod +x $1 && exit
echo I don\'t know what this is


#!/bin/bash
#
#Monitoring process
#
PROCESS=httpd
# make sure that $PROCESS is equal to the service script that needs starting 
COUNTER=0
while ps aux | grep $PROCESS | grep -v grep > /dev/null
do
	COUNTER=$((COUNTER+1))
	sleep 1
	echo COUNTER is $COUNTER
done

logger $PROCESSMONITOR: $PROCESS stopped at `date`
service $PROCESS start
mail -s "$PROCESS just stopped" mail@example.com < .



#!/bin/bash
#
#Monitoring process httpd
#

COUNTER=0
while ps aux | grep httpd | grep -v grep > /dev/null
do
	COUNTER=$((COUNTER+1))
	sleep 1
	echo COUNTER is $COUNTER
done

logger HTTPMONITOR: https stopped at `date`
/etc/init.d/apache2 start
mail -s "Apache service just stopped" mail@example.com < .

___________________________________________________________________________________________________________________________________________________________________________________________________

echo "cannot include exclamation - " !" within double quotas"
echo -e "\e[1;31m This is red text \e[0m"
\e[1;31m >> red color
\e[0m" >> return to white "

.bash_profile >> designed to execute when you login via console or local or ssh.
.bashrc  >> when you currently login and you open up a new window

vim .bash_profile
vim .bashrc

# .bashrc
# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

#source global definitions
if [ -f /etc/bashrc ]; then
		. /etc/bashrc

fi

env | grep HISTCONTROL

export HISTCONTROL=$HISTCONTROL:ignorespace

echo "" >> means a blank line

#Displaying environment var in a script:
#!/bin/bash

clear

echo "This script will give us environment informatio"
echo "==============================================="
echo ""
echo " Hello Username: $USER"
echo ""
echo "Your Home directory is: $HOME"
echo ""
echo "Your History file will ignore: $HISTCONTROL"
echo ""


#combining export and environment variable:
export SEARCHSITE="http://google.com"
env | grep SEARCHSITE
export TODAYSDATE=`date`
echo $TODAYSDATE
#but the result is relates to the time of export! so it is not usable.

#command substitution:

#!/bin/bash
# THis script is intended to show how to do simple substitution

TODAYSDATE=`date`
USERFILES=`find /home -user user`

echo "Today's Date: $TODAYSDATE"
echo "All Files owned by User: $USERFILES"

#how to find what the last command status was:
echo $?
0 means successfully completed.
1 means not successfully completed.

set -e >> if we put this in the begiining of the script this is going to tell shell if you recieve an error exit out of the shell.

#!/bin/bash
#this is to show exit status types
set -e

expr 1 + 5
echo $?

rm doodles.sh
echo $?

expr 10 + 10
echo $?


expr 2 + 2
expr 10 \* 8
expr \( 2 + 2 \) \* 4


printenv >> print all or part of environment
#env is the same as printenv

#set can see shell-local variables, env cannot.

/dev/null >> black hole of system

#read:

#!/bin/bash
#interactive script for user input

echo "Enter your First name."
read FIRSTNAME
echo ""
echo "Enter your Last name."
read LASTNAME
echo ""
echo "Your Full name is: $FIRSTNAME $LASTNAME"
echo ""
echo "Enter Your Age:"
read USERAGE
echo ""
echo "In 10 years, you will be `expr $USERAGE + 10` years old."


echo sh{ot,ort,oot}
echo st{il,al}l
echo "${!HO*}"

MYVAR=4
echo $MYVAR
echo `expr $MYVAR + 5`
declare -p MYVAR

declare -i NEWVAR=10
declare -p NEWVAR
NEWVAR="something"
echo $NEWVAR
declare -p NEWVAR
NEWVAR=110
echo $NEWVAR
declare +i NEWVAR
declare -p NEWVAR

declare -r READONLY="This is a string we cannot overwrite"
declare -p READONLY
READONLY="New Value"
declare +r READONLY

#array
MYARRAY= ("First" "Second" "Third")
echo ${MYARRAY[0]}
echo ${MYARRAY[*]}
MYARRAY[3]="Fourth"
NEWARRAY= ("First","Second","Third")
echo ${NEWARRAY[0]}
echo ${NEWARRAY[1]}


#!/bin/bash
#simple array list and loop for display

SERVERLIST=("websrv01" "websrv02" "webserv03" "websrv04")
COUNT=0

for INDEX in ${SERVERLIST[@]}; do
	echo "Processing Server: ${SERVERLIST[COUNT]}"
	COUNT="`expr $COUNT +1`"
done


#!/bin/bash
#demo of command line values passed in with our shell script

echo "The following item was passed in to the script at run time $!"

myscript.sh item01


#!/bin/bash
#demo of command line values passed in with our shell script

USERNAME=$1
PASSWORD=$2
echo "The following Username is $USERNAME and Password is $PASSWORD"

myscript.sh kavian Password@123

#if

#!bin/bash
#simple if script for guessing a number

echo "Guess the Secret Number"
echo "======================="
echo ""
echo "Enter a Number Between 1 and 5: "
read Guess

if [ $GUESS -eq 3 ]
	then
		echo "You Guessed the Correct Number!"
fi


#!/bin/bash
#tests for existence of indicated file name

FILENAME="mytext.txt"
echo "Testing for the existence of a file called $FILENAME"

if [ -e $FILENAME ]
	then
		echo "File $FILENAME does indeed exist!"
fi


# if [ -e $FILENAME ] = if [ -a $FILENAME ]
# if [ ! -e $FILENAME ] =  testing the file does not exist


#!/bin/bash
# test multiple expression in single if statement

FILENAME=$1

echo "Testing for file $FILENAME and readability"

if [ -f $FILENAME ] && [ -r $FILENAME ]
	then
		echo "File $FILENAME exists AND is readable"
fi


#loop

#!/bin/bash
#this is a demo of the for loop

echo "List all the shell scripts contents of the directory"

SHELLSCRIPTS=`ls *.sh`

echo "Listing is: $SHELLSCRIPTS"


#!/bin/bash
#this is a demo of the for loop

echo "List all the shell scripts contents of the directory"

SHELLSCRIPTS=`ls *.sh`

for SCRIPT in "$SHELLSCRIPTS"; do
	DISPLAY="`cat $SCRIPT`"
	echo "File: $SCRIPT - Contents $DISPLAY"
done


#case

#!/bin/bash
#demo of the case statement

clear

echo "MAIN MENU"
echo "========="
echo "1) Choice One"
echo "2) Choice Two"
echo "3) Choice Three"
echo ""
echo "Enter Choice: "
read MENUCHOICE

case $MENUCHOICE in
	1)
		echo "Congratulation for choosing the first option";;
	2)
		echo "Choice 2 chosen";;
	3)
		echo "Last Choice made";;
	*)
		echo "You chose unwisely";;
esac


#while loop
#!/bin/bash
# while loop example

echo "Enter the number of times to display the 'Hello World' message"
read DISPLAYNUMBER

COUNT=1

while [ $COUNT -le $DISPLAYNUMBER ]
	do
		echo "Hello World - $COUNT"
		COUNT="`expr $COUNT + 1`"
	done

#!/bin/bash
#execution operators example

echo "Enter a number between 1 and 5: "
read VALUE

if [ "$VALUE" -eq "1" ] || [ "$VALUE" -eq "3" ] || [ "$VALUE" -eq "5" ]
	then
		echo "you enterned the ODD value"
	else
		echo "You entered a value of $VALUE"
fi


#!/bin/bash
#simple file reading (non-binary) and displaying one line at a time

echo "Enter a filename to read: "
read FILE

while read -r SUPERHERO; do
	echo "Superhero Name: $SUPERHERO"
done < "$FILE"


#!/bin/bash
#demo of reading and writing to a file using a file dexcriptor

echo "Enter a file name to read"
read FILE

exec 5<>$FILE

while read -r SUPERHERO; do
	echo "Superhero Name: $SUPERHERO"
done <$5

echo "File was read on: `date`" >&5

exec 5>&-

#IFS (the default delimiter is " " (space))
echo $IFS

#!/bin/bash
#delimiter example using IFS

echo "Enter filename to parse: "
read FILE

echo "Enter the delimiter: "
read DELIM

IFS="$DELIM"

while read -r CPU MEMORY DISK; do
	echo "CPU: $CPU"
	echo "Memory: $MEMORY"
	echo "Disk: $DISK"
done <"$FILE"


#trap
#!/bin/bash
#example of trapping events and limiting the shell stopping

clear

trap 'echo " - Please press Q to Exit.."' SIGINT SIGTERM

while [ "$CHOICE" != "Q" ] && [ "$CHOICE" != "q" ]; do
	echo "MAIN MENU"
	echo "========="
	echo "1) Choice one"
	echo "2) Choice two"
	echo "3) Choice Three"
	echo "Q) Quit/Exit"
	echo ""
	read $CHOICE

	clear
done


#error handling
#!/bin/bash
#demo of using error handling with exit

echo "change to a directory and list the contents"
DIRECTORY=$1

cd $DIRECTORY

if [ "$?" = "0" ]; then
	echo "we can change into the directory $DIRECTORY, and here are the contents"
	echo "`ls -al`"
else
	echo "Cannot change directories, existing with error and no listing"
	exit 1
fi


#functions

#/bin/bash
#this is a simple function example

echo "starting the function definition..."

funcExample () {
	echo "We are now INSIDE the function..."
}

funcExample

#always put the function name after itself


___________________________________________________________________________________________________________________________________________________________________________________

touch test1
touch {test2,test3}
touch test{4,5}
touch test{6..8}

cat /var/log/message | uniq
cat /etc/passwd | cut -c3-8
cat /etc/passwd | cut -d ':' -f 7
cat << EOF > file1.txt
hello world
test
EOF

cat -E file1.txt (to append $ at the end of each line)

#to create a file with specified size:
fallocate -l 1G file3

#to split file based on size:
split -b 200M file3

#to find lines that the mentioned word is not listed:
grep -v "test" file2

#to find multiple words:
grep -e "test" -e "test" file

#to count the number of word:
grep -c "test" file

#to find the line that included the word:
grep -n "test" file

echo `date`
echo $(date)

MYNAME=kavian
echo $(#MYNAME) >> to show number of characters

touch file_`date +%d-%m-%y`

tail -f /var/log/messages
tailf /var/log/messages

sleep 10 &
jobs

ifconfig ens33 | grep netmask | awk '{print $2}'
ifconfig ens33 | grep netmask | awk '{print "IP address = " $2}'

sed '/s/hello/salam' file1
sed -i '/s/hello/salam' >> #in order to apply the change on file

alias
alias del='rm -rf'

ls && pwd >> #it means if the first command is successfull then run the second command as well.
pdw && pwd >> #since the first command is not successful it won't run the second command.
ls || pwd >> #it means the if the first command is successfull it won't run the second command.
pdw || pwd >> #if the first command is not successfull it will run the second command.

ls -a /etc/skel >> #in this location we can see all files that will be located in home directory of any new user. 

#the following changes will be assigned to every user (root and standard)
vim /etc/bashrc
MYNAME=kavian
export MYNAME

#if we want to put the special alias, function or variable for specific user we should change settings in user's home directory .bashrc file.
cd /home/kavian
vim .bashrc

#adding attributes to variables:
declare -i d=123		# d is an integer
declare -r e=456		# e is read-only
declare -l f="LOLCats"	# f is lolcats (lowercase)
declare -u g="LOLCats"	# g is LOLCATS (uppercase)

#!bin/bash
declare -i x=2
declare -i y=3
declare -l name=KavIaN
declare -u family=kaRimZadeh
declare -i c=$((x**y))
echo "x="$x "y="$y "name="$name "family="$family $c


[[ -z $a ]] >> is null?
[[ -n $a ]] >> is not null?

echo $@ >> to put different input
echo $# >> to show the number of input
echo `basename $0` >> to show the scriptname 

#if

#!/bin/bash
if [ 1 -eq 2 ]
	then
		echo "true"
	else
		echo "false"
fi

if [ -e "test.txt" ] >> it means if file test.txt exists then ...
if [ -d "mydir" ] >> it means if directory mydir exists then ...
if [ -z "$1" ] >> it means if $1 is null then ...

netstat -nlt >> to show opened ports
netstat -nlt | grep :25

#!/bin/bash
netstat -nlt | grep :25 > /dev/null
checksmtp="$?"
if [ $checksmtp -eq 0 ]
	then
		echo "smtp is running"
	else
		echo "smtp is stopped" | mail -s "SMTP is stopped" root
fi

#loop commands includes for, while and until.

#!/bin/bash
for name in name1 name2 name3 name4
do
	echo $name
done

#!/bin/bash
for user in `cat /etc/passwd | cut -d ':' -f 1`
do
	echo $user
done

#!/bin/bash
counter=0
for user in `cat /etc/passwd | cut -d ':' -f 1`
do
	echo $user
	counter=$((counter + 1))
done
echo "number of users: " $counter


#!/bin/bash
number=1
while [ $number -le 10 ]
do
	echo $number
	number=$((number + 1))
done

or

#!/bin/bash
number=1
while [ $number -le 10 ]
do
	echo $number
	let "number +=1"
done

#!/bin/bash
while [ l -eq l ]
do
	read -p "Enter your name : " name
	if [ $name != "mojtaba" ]
	then
		echo "Hello $name"
	else
		echo "Hello mojtaba"
		break
	fi
done
  
#function
#!/bin/bash
function showdate ()
{
	date +%F
}
function showtime ()
{
	date +%H:%M:%S
}
showdate
showtime

#!/bin/bash
function SendMail()
{
	echo "Hello World" | mail -s "test" root@localhost
}
SendMail


#select
#!/bin/bash
PS3='Enter a number : '
selec i in "Working_Directory" "Clean_Shell" "Quit"
do
	if [ $i = "Working_Directory" ]
	then
		pwd
	elif [ $1 = "Clean_Shell" ]
	then
		clear
	elif [ $1 = "Quit" ]
	then
		break
	fi
done

#to see open ports:
netstat -ntul | grep -v p6 | grep 0 | cut -d ':' -f 2 | awk '{print $1}'

#list of running service name:
systemctl list-unit | grep .service | grep running | awk '{print $1}' | cut -d '.' -f 1


#!/bin/bash
state=false
for i in `systemctl list-unit | grep .service | grep running | awk '{print $1}' | cut -d '.' -f 1`
do
	if [ $i = "firewalld" ]
	then
		state=true
		break
	fi
done
	if [ $state = "false" ]
	then
		echo "firewall is stopped" | mail -s "firewall_alert" root@localhost
	fi


systemctl list-unit | grep .service | grep running | awk '{print $1}' | cut -d '.' -f 1 | wc -l

systemctl status crond
vim /etc/crontab
crontab -e
/var/spool/cron
*/1 * * * * root echo "Hello" >> every minute
* * 1-7 * * root echo "Hello" >> first week of each month
0 0 * * * root echo "Hello" >> saat 12 har shab

crontab -l >> list of all created jobs
crontab -r >> remove cronjob

#to backup a special database:
mysqldump mysitedb -p243200 > mysitedb.sql

#to backup all databases:
mysqldump --all-database -p243200 > allDBs.sql

#
scp test.tar.gz 192.168.1.102:/root/

#!/bin/bash
backdir="/backup"
today=$(date +%F)

if [ ! -e $backdir ]
	then
		mkdir $backdir
	elif [! -e $backdir/$today ]
		then
			mkdir $backdir/$today
fi

count=$(ls -A $backdir | wc -l)

i f [ $count == 7 ]
	then
		rm -rf $backdir/*
fi

tar -czf $backdir/$today/testsite_$today.tar.gz /home/test/*

if [ $? != 0 ]
	then
		echo "backup is failed!"
		exit
	else
		#backup from database
		mysqldump mysitedb -p243200 > $backdir/$today/testdb_$today.sql
	
	if [ $? != 0 ]
		then
			echo "db backup is failed"
			exit
		else
			#compressing directory
			tar -czf $backdir/$today/backup_$today.gz $backdir/$today
	
		if [ $? != 0 ]
			then
				echo "compress backup is failed"
				exit
			else
				#transferiing compressed backuped file to another system
				scp $backdir/$today/backuo_$today.tar.gz <targetIP>:/backup

			if [ $? != 0 ]
				then
					echo "transfering backup is failed"
					exit
				else
					echo "backup is sent successfully"
					rm -rf $backdir/$today/test* $backdir/$today/Test* 

			fi
		fi
	fi
fi 

#to use keygen authentication to ignore asking for password in scp between two systems:
#SSH key-base authentication:
ssh-keygen ---> generate rsa key
ssh-copy-id -i ~/.ssh/id_rsa_pub root@<targetIP


systemctl list-units | grep .service | grep running | awk '{print $1}' | cut -d '.' -f 1 > services
#!/bin/bash
for i in `cat /root/services`
do
	state=`systemctl list-units | grep .service | grep $i | awk '{print $4}'`
	if [ "$state" != "running" ]
		then
			systemctl start $i
				state=`systemctl list-units | grep .service | grep $i | awk '{print $4}'`
 					if [ "$state" != "running" ]
 						then
 							echo "$USER $(date +%F) $(date +%H:%M:%S) $(hostname) $(basename $0): could not start $i service" >> /var/log/checkservice.log
 						else
							 echo "$USER $(date +%F) $(date +%H:%M:%S) $(hostname) $(basename $0): $i service is started" >> /var/log/checkservice.log 
					fi
	fi
done

 ___________________________________________________________________________________________________________________________________________________________________________________

echo $0
cat /etc/shells

read name
echo $name

#!bin/bash
echo "Enter the name: "
read name
echo $name

or

#!/bin/bash
read -p "Enter the name: " name
echo "Hello $name"

#to echo on seperate line:
echo -e "$a\n$A"

#!/bin/bash
read -p "Enter the name: " name
if [ "$name" == "kavian" ]
	then
		echo "OK!"
	elif [ "$name" == "amir" ]
		then
			echo "Hello Amir"
	else
		echo "Error!"
		exit
fi
clear
ls
read -p "Please enter a directory name: " directory
if [ -d "$HOME/$directory" ]
	then
		echo "directory exists"
	else
		echo "Error!"
		echo "Do you want to create #directory (y/n): "
		read answer
		if [ "$answer" == "y" ]
			then
				mkdir "$directory"
			else
				echo "Bye!"
fi

#!/bin/bash
read -p "Please enter a name: " name
if [ "$name" == "alireza" ]
	then
		echo "Hi Alireza"
	elif [ "$name" == "amir" ]
		then
			echo "good morning amir"
	elif [ "$name" == "hadi" ]
		then
			echo "Hi Hadi"
	else
		echo "error"
fi

or

#!/bin/bash
read -p "Please enter a name: " name
case "$name" in
	"alireza")
		echo "Hi alireza";;
	"amir")
		echo "Hi amir";;
	"hadi")
		echo "Hi hadi";;
	*)
		echo "error"
esac

#!/bin/bash
clear
read -p "Please enter a number: " number
if [ "$number" -lt "5" ]
	then
		echo "ok"
	else
		echo "error"
fi

#
echo `expr 2 + 2`
or
echo $((2 + 2))

#!/bin/bash
clear
read -p "Please enter a number: " number
resedu=$((number / 2))
if [ "$resedu" == "0" ]
	then
		echo "even"
	else
		echo "odd"
fi


#!/bin/bash
clear
read -p "Please enter a name" name
while [ "$name" != "alireza" ]
do
	read -p "Error! Please try again: " name
done
echo "OK!"

******** while [ "1" == "1" ] >> it means ta vaghti ke 1 = 1 hast felan karo anjam bede ke beyne do and done moshakhas kardim.

#!/bin/bash
clear
while [ "1" == "1" ]
do
	sleep 10
	echo " REST "
done

sudo apt-get install libnotify-bin

#!/bin/bash
clear
while [ "1" == "1" ]
do
	sleep 10
	notify-send " REST "
done

# to download a file in a specified time:
#!/bin/bash
clear
read -p "Please enter hour: " hour
read -p "Please enter minute: " minute
date_hour=`date +%H`
date_minute=`date +%M`
sigma_date=$((date_hour * 60 + date_minute))
sigma_date_input=$((hour * 60 + minute))
while [ "$sigma_date" != "$sigma_date_input" ]
do
	sleep 20
	date_hour=`date +%H`
	date_minute=`date +%M`
	sigma_date=$((date_hour * 60 + date_minute))
done
wget "http://path/movie.mpeg"
if [ "$?" == "0" ]
then
	notify-send "Download is completed"
else
	notify-send "Download is failed!"
fi

#!/bin/bash
clear
read -p "Please enter a number " number
if [ "$number" -gt "10" ] && [ "$number" -lt "25" ]
then
	echo "OK"
else
	echo "NO"
fi


#!/bin/bash
clear
number="1"
while [ "$number" != "10" ]
do
	echo $number
	number=$((number + 1))
done

or

#!/bin/bash
clear
number="1"
while [ "$number" != "10" ]
do
	echo $number
	((number++))
done

or

#!/bin/bash
clear
number="1"
until [ "$number" = "10" ]
do
	echo $number
	((number++))
done

or

#!/bin/bash
clear
for number in {1..9}
do
	echo $number
done

or

#!/bin/bash
clear
for ((i=0;i<=10;i++))
do
	echo $i
done

#create multiple users
#!/bin/bash
clear
for i in user{1..5}
do
	useradd
done

#!/bin/bash
clear
for i in ali reza omid hadi
do
	echo "hello $i"
done

#IFS
#!/bin/bash
IFS=':'
clear
name="ali:reza:hadi:omid"
for i in $name
do
	echo hello $i
done

#!/bin/bash
IFS=$'\n'
clear
mkdir "$HOME/Pictures2"
b="1"
files=`ls $HOME/Pictures/*png`
for i in $files
do
	convert "$i" -resize 50% "$HOME/Pictures2/$b.jpg"
	echo "$b.jpg created!"
	((b++))
done


#!/bin/bash
read -p "Please enter number 1 :" number 1
read -p "Please enter number 2 :" number 2
number3=$((number1 + number1))
echo "answer is $number3"

or

#!/bin/bash
read -p "Please enter number 1 :" number 1
read -p "Please enter number 2 :" number 2
echo "answer is " $((number1 + number2))

or

#!/bin/bash
number1= "$1"
number2= "$2"
echo "answer is " $((number1 + number2))

or

#!/bin/bash
echo "answer is " $(($1 + $2))

#to show the count of arguments
#!/bin/bash
echo $#

#!/bin/bash
echo "Hello $@"

#!/bin/bash
for i in "$@"
do
	echo "Hello $i"
done

#!/bin/bash
input="$1"
if [ "$input" == "" ]
then
	read -p "Please enter a name: " input
fi
echo "Hello $input"


#!/bin/bash
clear
while [ "1" == "1" ]
do
	read -p "Please enter your name: " name
	if [ "$name" == "ali" ]
		then
			echo "OK"
			sleep 2
			break
	fi
	echo "try again!!"
done
clear
ls

#!/bin/bash
clear
while [ "1" == "1" ]
do
	read -p "Please enter your name: " name
	if [ "$name" == "ali" ]
		then
			echo "OK"
			continue
	fi
	echo "try again!!"
done

#function

#!/bin/bash
ali (){
	echo "**********"
	date +%H:%MONTH
	echo "**********"
	date +%F
	echo "**********"
}
clear
echo "hello"
ali
echo "how are you"
ali
echo "hiiiiii"
ali

#!/bin/bash
add_function (){
	echo "**************"
	echo "$1 +$2 = "  $(($1 + $2))
	echo "**************"
}
clear
add_function 10 20

#!/bin/bash
function1 (){
	((x++))
	echo "function1 x= $x"
}

clear
x=1
function1

 
#!/bin/bash
function1 (){
	while [ "$x" -lt "15" ];do
		((x++))
		echo "function1 $x"
		sleep 1 
	done
}
function2 (){
	while [ "$x" -lt "50" ];do
		((x+=5))
		echo "function2 x = $x"
		sleep 1
	done
}
clear
x=1
function1 &
function2
 
#!/bin/bash
sigmatime(){
	"local_hour"="$1"
	"local_minutes"="$2"
	"local_hour"=${local_hour#0}
	"local_minutes"=${local_minutes#0}
	if [ -z "$local_hour" ];then
		"local_hour"="0"
	fi
	if [ -z "$local_minutes" ];then
		"local_minutes"="0"

	echo $(("local_hour" * 60 + "local_minutes"))
}
date_time(){
	date_hour=`date +%H:%M`
	date_hour=${date_hour/:/ }
	sigma_date=`sigmatime $date_hour` 
}

clear
start_time_input="16:30"
end_time_input="17:30"
link="http://address/file.png"


#to show the script PID:
echo $$

grep "^[a]"
grep "[d]$"

echo "day day day day day" | sed 's/day/night/'
echo "day day day day day" | sed 's/day/night/g'
echo "day day day day day" | sed 's/day/night/2'
echo "day day day day day" | sed 's/day/night/3'
echo "day day day day day" | sed 's/day/night/3g'
echo "day day day day day" | sed -e 's/day/night/1' -e -e 's/day/night/2 ' 
sed -i 's/a/r/g' text >> -t to apply the changes to the file
sed '3 s/day/night/' text >> only affect the third line
sed '3 !s/day/night/' text >> affects all execpt the third line
sed '1 d' text >> to remove the first line of text


gnu_linux=("xubuntu" "arch" "fedora" "debian" "ubuntu")
echo ${gnu_linux[0]}
echo ${gnu_linux[@]}
echo ${#gnu_linux[@]}
gnu_linux[8]="linux mint"
echo ${gnu_linux[@]:2:4}


#!/bin/bash
clear
echo "please enter 6 numbers"
for i in {0..5};do
	read -p "Please enter number No. $((i++)): " number
	number_array[i]="$number"
done
min=${number_array[0]}
for i in {0..5};do
	if [ "${number_array[i]}" -lt "$min" ];then
	min=${number_array[i]}
	fi
done
max=${number_array[0]}
for i in {0..5};do
	if [ "${number_array[i]}" -gt "$max" ];then
	max=${number_array[i]}
	fi
done

echo -e "minimum is $min\maximum is $max"

___________________________________________________________________________________________________________________________________________________________________________________

find /var/log -mtime 0 | cpio -o > log.cpio
cpio -t < log.cpio | sed 's/var/test/g' > file.txt
mkdir tarfolder
cp /bin/* tarfolder
tar -cJf file.tar.xz tarfolder
tar -tJf file.tar.xz > tar-list
bzip2 tar-list
for U in $(seg 5)
do
	useradd user$U
	echo "user$U" >> /etc/crondeny
	echo 'echo "you cannot use cron"' >> /home/user$U/.bashrc
	echo "/home/user$U" >> home.txt
	cp tar-list.bz2 /home/user$U
done
groupadd lpic2
chown :lpic2 tar-list.bz2
chmod 431 tar-list.bz2


echo $SHELLOPTS
set -o noclobber >> preventing to overwrite
#exception
date >| httpd.conf
vim /home/user1/.bashrc
set -o noclobber

lsmod >> list of module
dmesg >> list of logs during boot process
dmesg | grep -i error/warning


alias
alias rm=`rm -i`

export PATH=$PATH:.
mkdir ~/.HIST
export HISTFILE="/root/.HST/ `/bin/date '+%F_%H%M' .hstry.txt`"
mail -s "boot warning" "root" <dmesg.txt
echo "mail content" | mail -a dmesg.txt -s "boot warning" "root"

vim /etc/crontab
reboot root dmesg-checker.sh


find /home/kavian -name "*.pdf" 1>kavian.txt
find /home/kavian -name "*.pdf" 2>kavian.txt (error)
find /home/kavian -name "*.pdf" 1>kavian.txt 2>&1 (to put both result and error to a file)
find /home/kavian -name "*.pdf" 2>/dev/null

IFS=$'\n' >> it means delimeter is per line


#!/bin/bash
IFS=$'\n'
clear
files=(`find /home/kavian -name "*.pdf" 2>/dev/null`)
number=${#files[@]}
echo "Number of files: $number"
mkdir /home/kavian/mypdf 2>/dev/null
j=0
for ((i=0; i < $number; i++));do
	((j++))
	cp "${file[i]}" "/home/kavian/mypdf/$j.pdf"
	echo "$j - ${files[i]} copied!"
done

___________________________________________________________________________________________________________________________________________________________________________________

# Assign a value to a variable.
WORD='script'

# Append text to the variable.
echo "${WORD}ing is fun!"

# Show how NOT to append text to a variable.
# This doesn't work:
echo "$WORDing is fun!"

# Create a new variable
ENDING='ed'

# Combine the two variables.
echo "This is ${WORD}${ENDING}."

# Change the value stored in the ENDING variable.  (Reassignment.)
ENDING='ing'
echo "${WORD}${ENDING} is fun!"

___________________________________________________________________________________________________________________________________________________________________________________

# Display the UID
echo "Your UID is ${UID}"

# Display the username
USER_NAME=$(id -un)
echo "Your username is ${USER_NAME}"

# Display if the user is the root user or not.
# root is always has uid of zero.

if [[ "${UID}" -eq 0 ]]
then
  echo 'You are root.'
else
  echo 'You are not root.'
fi


# Only display if the UID does NOT match 1000.
UID_TO_TEST_FOR='1000'
if [[ "${UID}" -ne "${UID_TO_TEST_FOR}" ]]
then
  echo "Your UID does not match ${UID_TO_TEST_FOR}."
  exit 1
fi

# Display the username.
USER_NAME=$(id -un)

# Test if the command succeeded.
if [[ "${?}" -ne 0 ]]
then
  echo 'The id command did not execute successfully.'
  exit 1
fi
echo "Your username is ${USER_NAME}"

# You can use a string test conditional.
USER_NAME_TO_TEST_FOR='vagrant'
if [[ "${USER_NAME}" = "${USER_NAME_TO_TEST_FOR}" ]]
then
  echo "Your username matches ${USER_NAME_TO_TEST_FOR}."
fi

# Test for != (not equal) for the string.
if [[ "${USER_NAME}" != "${USER_NAME_TO_TEST_FOR}" ]]
then
  echo "Your username does not match ${USER_NAME_TO_TEST_FOR}."
  exit 1
fi

exit 0

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script creates an account on the local system.
# You will be prompted for the account name and password.

# Ask for the user name.
read -p 'Enter the username to create: ' USER_NAME

# Ask for the real name.
read -p 'Enter the name of the person who this account is for: ' COMMENT

# Ask for the password.
read -p 'Enter the password to use for the account: ' PASSWORD

# Create the user.
# -m, --create-home >> create the user's home directory if it does not exist. by defualt if this option is not specified and CREATE_HOME is not enabled, no home directories are created.

useradd -c "${COMMENT}" -m ${USER_NAME}

# Set the password for the user.
# NOTE: You can also use the following command:
#    echo "${USER_NAME}:${PASSWORD}" | chpasswd
echo ${PASSWORD} | passwd --stdin ${USER_NAME}

# Force password change on first login.
# -e >> the user will be forced to change the password during the next login
passwd -e ${USER_NAME}
  
___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
# without ##
read -p 'Enter the username to create: ' USER_NAME
read -p 'Enter the name of the person who this account is for: ' COMMENT
read -p 'Enter the password to use for the account: ' PASSWORD
useradd -c "${COMMENT}" -m ${USER_NAME}
echo ${PASSWORD} | passwd --stdin ${USER_NAME}
passwd -e ${USER_NAME}

___________________________________________________________________________________________________________________________________________________________________________________


#!/bin/bash
#
# This script creates a new user on the local system.
# You will be prompted to enter the username (login), the person name, and a password.
# The username, password, and host for the account will be displayed.

# Make sure the script is being executed with superuser privileges.
if [[ "${UID}" -ne 0 ]]
then
   echo 'Please run with sudo or as root.'
   exit 1
fi

# Get the username (login).
read -p 'Enter the username to create: ' USER_NAME

# Get the real name (contents for the description field).
read -p 'Enter the name of the person or application that will be using this account: ' COMMENT

# Get the password.
read -p 'Enter the password to use for the account: ' PASSWORD

# Create the account.
useradd -c "${COMMENT}" -m ${USER_NAME}

# Check to see if the useradd command succeeded.
# We don't want to tell the user that an account was created when it hasn't been.
if [[ "${?}" -ne 0 ]]
then
  echo 'The account could not be created.'
  exit 1
fi

# Set the password.
echo ${PASSWORD} | passwd --stdin ${USER_NAME}

# Check to see if the passwd command succeeded.
if [[ "${?}" -ne 0 ]]
then
  echo 'The password for the account could not be set.'
  exit 1
fi

# Force password change on first login.
passwd -e ${USER_NAME}

# Display the username, password, and the host where the user was created.
echo
echo 'username:'
echo "${USER_NAME}"
echo
echo 'password:'
echo "${PASSWORD}"
echo
echo 'host:'
echo "${HOSTNAME}"
exit 0
___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
# without ##

if [[ "${UID}" -ne 0 ]]
then
   echo 'Please run with sudo or as root.'
   exit 1
fi

read -p 'Enter the username to create: ' USER_NAME
read -p 'Enter the password to use for the account: ' PASSWORD
useradd -c "${COMMENT}" -m ${USER_NAME}

if [[ "${?}" -ne 0 ]]
then
  echo 'The account could not be created.'
  exit 1
fi

echo ${PASSWORD} | passwd --stdin ${USER_NAME}

if [[ "${?}" -ne 0 ]]
then
  echo 'The password for the account could not be set.'
  exit 1
fi

passwd -e ${USER_NAME}

echo
echo 'username:'
echo "${USER_NAME}"
echo
echo 'password:'
echo "${PASSWORD}"
echo
echo 'host:'
echo "${HOSTNAME}"
exit 0

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script generates a list of random passwords.

# A random number as a password.
PASSWORD="${RANDOM}"
echo "${PASSWORD}"

# Three random numbers together.
PASSWORD="${RANDOM}${RANDOM}${RANDOM}"
echo "${PASSWORD}"

# Use the current date/time as the basis for the password.
PASSWORD=$(date +%s)
echo "${PASSWORD}"

# Use nanoseconds to act as randomization.
PASSWORD=$(date +%s%N)
echo "${PASSWORD}"

# A better password.
PASSWORD=$(date +%s%N | sha256sum | head -c32)
echo "${PASSWORD}"

# An even better password.
PASSWORD=$(date +%s%N${RANDOM}${RANDOM} | sha256sum | head -c48)
echo "${PASSWORD}"

# Append a special character to the password.
SPECIAL_CHARACTER=$(echo '!@#$%^&*()_-+=' | fold -w1 | shuf | head -c1)
echo "${PASSWORD}${SPECIAL_CHARACTER}"

___________________________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script generates a random password for each user specified on the comand line.

# Display what the user typed on the command line.
echo "You executed this command: ${0}"

# Display the path and filename of the script.
echo "You used $(dirname ${0}) as the path to the $(basename ${0}) script."

# Tell them how many arguments they passed in.
# (Inside the script they are parameters, outside they are arguments.)
NUMBER_OF_PARAMETERS="${#}"
echo "You supplied ${NUMBER_OF_PARAMETERS} argument(s) on the command line."

# Make sure they at least supply one argument.
if [[ "${NUMBER_OF_PARAMETERS}" -lt 1 ]]
then
  echo "Usage: ${0} USER_NAME [USER_NAME]..."
  exit 1
fi

# Generate and display a password for each parameter.
for USER_NAME in "${@}"
do
  PASSWORD=$(date +%s%N | sha256sum | head -c48)
  echo "${USER_NAME}: ${PASSWORD}"
done

___________________________________________________________________________________________________________________________________________________________________________________

for X in Frank Claire Doug
do
	echo "Hi ${X}"
done
_________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
# Demonstrate the use of shift and while loops.
# Display the first three parameters.
echo "Parameter 1: ${1}"
echo "Parameter 2: ${2}"
echo "Parameter 3: ${3}"
echo

# Loop through all the positional parameters.
while [[ "${#}" -gt 0 ]]
do
  echo "Number of parameters: ${#}"
  echo "Parameter 1: ${1}"
  echo "Parameter 2: ${2}"
  echo "Parameter 3: ${3}"
  echo
  shift
done

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
#
# This script creates a new user on the local system.
# You must supply a username as an argument to the script.
# Optionally, you can also provide a comment for the account as an argument.
# A password will be automatically generated for the account.
# The username, password, and host for the account will be displayed.

# Make sure the script is being executed with superuser privileges.
if [[ "${UID}" -ne 0 ]]
then
   echo 'Please run with sudo or as root.' >&2
   exit 1
fi

# If the user doesn't supply at least one argument, then give them help.
if [[ "${#}" -lt 1 ]]
then
  echo "Usage: ${0} USER_NAME [COMMENT]..." >&2
  echo 'Create an account on the local system with the name of USER_NAME and a comments field of COMMENT.' >&2
  exit 1
fi

# The first parameter is the user name
USER_NAME="${1}"

# The rest of the parameters are for the account comments.
shift
COMMENT="${@}"

# Generate a password.
PASSWORD=$(date +%s%N | sha256sum | head -c48)

# Create the user with the password.
useradd -c "${COMMENT}" -m ${USER_NAME} &> /dev/null

# Check to see if the useradd command succeeded.
# We don't want to tell the user that an account was created when it hasn't been.
if [[ "${?}" -ne 0 ]]
then
  echo 'The account could not be created.' >&2
  exit 1
fi

# Set the password.
echo ${PASSWORD} | passwd --stdin ${USER_NAME} &> /dev/null

# Check to see if the passwd command succeeded.
if [[ "${?}" -ne 0 ]]
then
  echo 'The password for the account could not be set.' >&2
  exit 1
fi

# Force password change on first login.
passwd -e ${USER_NAME} &> /dev/null

# Display the username, password, and the host where the user was created.
echo 'username:'
echo "${USER_NAME}"
echo
echo 'password:'
echo "${PASSWORD}"
echo
echo 'host:'
echo "${HOSTNAME}"
exit 0

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
# without ##

if [[ "${UID}" -ne 0 ]]
then
   echo 'Please run with sudo or as root.' >&2
   exit 1
fi

if [[ "${#}" -lt 1 ]]
then 
  echo "Usage: ${0} USER_NAME [COMMENT]..." >&2
  echo 'Create an account on the local system with the name of USER_NAME and a comments field of COMMENT.'
  exit 1
fi

USER_NAME="${1}"

shift
COMMENT="${@}"

PASSWORD=$(date +%s%N | sha256sum | head -c48)

useradd -c "${COMMENT}" -m ${USER_NAME} &> /dev/null

if [[ "${?}" -ne 0 ]]
then
  echo 'The account could not be created.' >&2
  exit 1
fi

echo ${PASSWORD} | passwd --stdin ${USER_NAME} &> /dev/null

if [[ "${?}" -ne 0 ]]
then
  echo 'The password for the account could not be set.' >&2
  exit 1
fi

passwd -e ${USER_NAME} &> /dev/null

echo
echo 'username:'
echo "${USER_NAME}"
echo
echo 'password:'
echo "${PASSWORD}"
echo
echo 'host:'
echo "${HOSTNAME}"
exit 0

./script kaviank kavian karimzadeh
___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script demonstrates I/O redirection.

# Redirect STDOUT to a file.
FILE="/tmp/data"
head -n1 /etc/passwd > ${FILE}

# Redirect STDIN to a program.
read LINE < ${FILE}
echo "LINE contains: ${LINE}"

# Redirect STDOUT to a file, overwriting the file.
head -n3 /etc/passwd > ${FILE}
echo
echo "Contents of ${FILE}:"
cat ${FILE}

# Redirect STDOUT to a file, appending to the file.
echo "${RANDOM} ${RANDOM}" >> ${FILE}
echo "${RANDOM} ${RANDOM}" >> ${FILE}
echo
echo "Contents of ${FILE}:"
cat ${FILE}

# Redirect STDIN to a program, using FD 0.
read LINE 0< ${FILE}
echo
echo "LINE contains: ${LINE}"

# Redirect STDOUT to a file using FD 1, overwriting the file.
head -n3 /etc/passwd 1> ${FILE}
echo
echo "Contents of ${FILE}:"
cat ${FILE}

# Redirect STDERR to a file using FD 2.
ERR_FILE="/tmp/data.err"
head -n3 /etc/passwd /fakefile 2> ${ERR_FILE}
echo
echo "Contents of ${ERR_FILE}:"
cat ${ERR_FILE}

# Redirect STDOUT and STDERR to a file.
head -n3 /etc/passwd /fakefile &> ${FILE}
echo
echo "Contents of ${FILE}:"
cat ${FILE}

# Redirect STDOUT and STDERR through a pipe.
echo
head -n3 /etc/passwd /fakefile |& cat -n

# Send output to STDERR
echo "This is STDERR!" >&2

#This is generally the better place to send logging output that isn't the actual result of the computation.

___________________________________________________________________________________________________________________________________________________________________________________

# Discard STDOUT
echo
echo "Discarding STDOUT:"
head -n3 /etc/passwd /fakefile > /dev/null

# Discard STDERR
echo
echo "Discarding STDERR:"
head -n3 /etc/passwd /fakefile 2> /dev/null

# Discard STDOUT and STDERR
echo
echo "Discarding STDOUT and STDERR:"
head -n3 /etc/passwd /fakefile &> /dev/null

# Clean up
rm ${FILE} ${ERR_FILE} &> /dev/null

___________________________________________________________________________________________________________________________________________________________________________________

read X < /etc/centos-release
echo ${X}
CentOS Linux release 7.3.15.11 (core)
___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script demonstrates the case statement.

# Instead of an if statement like this, consider using a case statement instead.
if [[ "${1}" = 'start' ]]
 then
   echo 'Starting.'
elif [[ "${1}" = 'stop' ]]
 then
   echo 'Stopping.'
elif [[ "${1}" = 'status' ]]
 then
   echo 'Status:'
else
   echo 'Supply a valid option.' >&2
   exit 1
fi

# This ideal format of a case statement follows.
case "${1}" in
   start)
     echo 'Starting.'
     ;;
   stop)
     echo 'Stopping.'
     ;;
   status|state|--status|--state)
     echo 'Status:'
     ;;
   *)
     echo 'Supply a valid option.' >&2
     exit 1
     ;;
 esac


# Here is a compact version of the case statement.
case "${1}" in
  start) echo 'Starting.' ;;
  stop) echo 'Stopping.' ;;
  status) echo 'Status:' ;;
  *)
    echo 'Supply a valid option.' >&2
    exit 1
    ;;
esac

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

log() {
  local MESSAGE="${@}"
  if [[ "${VERBOSE}" = 'true' ]]
  then
    echo "${MESSAGE}"
  fi
}

log 'Hello'
VERBOSE='true'
log 'This is fun!'

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash

# This script demonstrates the use of functions.

log() {
  # This function sends a message to syslog and to standard output if VERBOSE is true.

  local MESSAGE="${@}"
  if [[ "${VERBOSE}" = 'true' ]]
  then
    echo "${MESSAGE}"
  fi
  logger -t luser-demo10.sh "${MESSAGE}"
}

backup_file() {
  # This function creates a backup of a file.  Returns non-zero status on error.

  local FILE="${1}"

  # Make sure the file exists.
  if [[ -f "${FILE}" ]]
  then
    local BACKUP_FILE="/var/tmp/$(basename ${FILE}).$(date +%F-%N)"
    log "Backing up ${FILE} to ${BACKUP_FILE}."

    # The exit status of the function will be the exit status of the cp command.
    cp -p ${FILE} ${BACKUP_FILE}
  else
    # The file does not exist, so return a non-zero exit status.
    return 1
  fi
}

readonly VERBOSE='true'
log 'Hello!'
log 'This is fun!'

backup_file /etc/passwd

# Make a decision based on the exit status of the function.
# Note this is for demonstration purposes.  You could have
# put this functionality inside of the backup_file function.
if [[ "${?}" -eq '0' ]]
then
  log 'File backup succeeded!'
else
  log 'File backup failed!'
  exit 1
fi
 
 ___________________________________________________________________________________________________________________________________________________________________________________

 #!/bin/bash

PID=$(echo $$)
top -H -p ${PID}

number=1000
for ((i=0; i < $number; i++))
do
        echo "TEST"&
        sleep 1
done

___________________________________________________________________________________________________________________________________________________________________________________

##script_1

#!/bin/bash
grep -i error /var/log/messages

##script_2

#!/bin/bash
grep -i warn /var/log/messages

##script_3

#!/bin/bash
grep -i fail /var/log/messages

##script_4

#!/bin/bash
hosts="192.168.1.1"
ping -c1 $hosts	&> /dev/null
	if [ $? -eq 0 ]
	then
		echo $hosts is reachable.
	else
		echo $hosts os NOT reachable.
	fi

##script_5

#!/bin/bash
hosts="/home/kavian/ps/IPlist.txt"
for ip in $(cat $hosts)
do
ping -c1 $ip &> /dev/null
	if [ $? -eq 0 ]
	then
		echo $ip is reachable.
	else
		echo $ip is NOT reachable.
	fi
done


##script_6

#!/bin/bash
find /home/kavian/ps -mtime +90 -exec rm {} \;


##script_7

#!/bin/bash
find /home/kavian/ps -mtime +90 -exec mv {} {}.old \;


##script_8

#!/bin/bash
tar cvf /tmp/backup.tar /etc /var
gzip /tmp/backup.tar
find /tmp/backup.tar.gz mtime -1 -typed f -print &> /dev/null
if [ $? -eq 0 ]
	then
		echo "Backup was created"
		echo ""
		echo "Archiving Backup"
		scp /tmp/backup.tar.gz root@192.168.1.100:/home/backup/
	else
		echo "Backup failed"
fi


##script_9

#!/bin/bash
#simple counts from 1 to 10
for i in {1..10}
	do
		echo $1
		sleep 1
	done


##script_10

#!/bin/bash

for i in {1..10}
	do
		touch file.$1
	done


##script_11

#!/bin/bash
echo "How many files do you want to be created?"
read total
echo ""
echo "Please enter the start name of the files"
read name

for i in $(seq 1 $total)
	do
		touch $name.$i
	done


##script_12

#!/bin/bash
#rename files
for filename in *.txt
do
	mv $filename ${filename&.txt}.none
done

##script_13

#!/bin/bash
for server in redhat1 redhat2 redhat3
do
	scp somefiles $server:/tmp
done

last | grep "Fri Aug 31"
date | awk '{print $1,$2,$3}'

##script_13

#!/bin/bash
today=`date | awk '{print $1,$2,$3}'`
last | grep "$today" | awk '{print $1}'


##script_14

#!/bin/bash
echo "Please enter day (e.g Mon)"
read day
echo ""
echo "Please enter month (e.g Aug)"
read month
echo ""
echo "Please enter date (e.g 28)"
read date
echo ""

last | grep "$day $month $date" | awk '{print $1}'

##script_15

#!/bin/bash
tail -fn0 /var/log/messages | while read line
do
	echo $line | egrep -i "refused|invalid|error|fail|lost|shut|down|offline"
	if [ $? = 0 ]
	then
		echo $line >> /tmp/filtered-messages
	fi
done


##script_16

#!/bin/bash
echo "please provide a username"
read user
echo ""
useradd $user
echo $user account has been created.


##script_17

#!/bin/bash
echo "please provide a username"
read user
echo ""
grep -q $user /etc/passwd
if [ $? -eq 0 ]
then
	echo "ERROR -- User $user already exists"
	echo "Please choose another name"
	echo ""
	exit 0
fi

echo "Please provide user description"
reade desc
echo ""

echo "Do you want to specify user ID (y/n)?"
read answer
echo ""
if [ $answer == y ]
then
	echo "Please enter UID?"
	read uid
		grep -q $uid /etc/passwd
		if [ $? -eq 0 ]
		then
			echo "ERROR -- userid $uid already exists"
			echo ""
			exit 0
		else
			useradd $user -c "$desc" -u $uid
			echo ""
			echo "$user account has been created."
elif [ $answer == n ]
	then
		echo "no worries, we will assign a uid"
		useradd $u -c "$desc"
fi


##script_18

#!/bin/bash
#disable inactive user accounts
user=`lastlog | tail -n+2 | grep -v Never | awk '{print $1}'`
for i in $user
do
	usermod -L $i
done

##script_19

#!/bin/bash
#disable inactive user accounts
user=`lastlog | tail -n+2 | grep -v Never | awk '{print $1}'` | xargs -I{} usermod -L {}


##script_20

#!/bin/bash
#check Process status and killing it
ps -ef | grep "sleep 600" | grep -v grep | awk '{print $2}' | xargs -I{} kill {}


##script_21

#!/bin/bash
#disk space status
a=`df -h | egrep -v "tmpfs|devtmpfs" | tail -n+2 | awk '{print $5}' | cut -d'%' -f1`
for i in $a
do
	if [ $i -ge 90 ]
	then
		echo "check disk space"
	fi
done


##script_22

#!/bin/bash
#Script to run for a number of times

c=1
while [ $c -le 5 ]
do
        echo "Welcone $c times"
        (( c++ ))
done


##script_23

#!/bin/bash
#for loop to create 5 files named 1-5

for i in {1..5}
do
   touch $i
done


##script_24

#!/bin/bash
#List all users one by one from /etc/passwd file

i=1
for username in `awk -F: '{print $1}' /etc/passwd`
do
 echo "Username $((i++)) : $username"
done

___________________________________________________________________________________________________________________________________________________________________________________
##Dean Davis 

##script_1
##anonymize IP address from LOG
#Apache LOG Output
#
LOG=$1
ANONYMIZED=$LOG.anonymized
if test -f $LOG; than
	echo "File $LOG exists";
	sed -e 's/\([0-9]*\.[0-9]*\.[0-9]*\.[0-9]*\)\(.*\)/anonymous\2/g' $LOG > $ANONYMIZED
else
	echo "$LOG file no exists"
fi


./script_1 apachelog.txt

##in single qutation we can't use variables, so we should use double quotations.
#for example:

LOG=$1
ANONYMIZED=$LOG.anonymized
REPL1="anonymous"
sed -e "s/\([0-9]*\.[0-9]*\.[0-9]*\.[0-9]*\)\(.*\)/$REPL1\2/g" $LOG > $ANONYMIZED



echo $#
if [ $# -ne 1 ]; then
	echo "USAGE: $0 LOGFILE";
	exit 200;
fi

for i in `seq 10`; do echo "NUMBER $i"; done



##script
#!/bin/bash
if [ ! $# -gt 1 ]; then
	echo "USAGE: $0 LOGFILE";
	exit 200;
fi

for i in $*; do

	LOG=$1
	ANONYMIZED=$LOG.anonymized
	REPL1="anonymous"

	if test -f $LOG; than
		echo "File $LOG exists";
		sed -e 's/\([0-9]*\.[0-9]*\.[0-9]*\.[0-9]*\)\(.*\)/anonymous\2/g' $LOG > $ANONYMIZED
	else
		echo "$LOG file no exists"
	fi
done

./script apachelog.txt


head -n 5000 data/10000hits.log > data/5000hits.log && ls -l data && wc -l date/5000hits.log

i=500; head -n $i data/10000hits.log > data/$ihits.log && ls -l data && wc -l date/$ihits.log


df kavian/ | tail -n 1 | awk '{print $ 4}'
df --output=avail kavian/ | egrep '^[0-9]*$'
du -k /kavian/log.txt |  awk '{ print $1 }'

#CHECK STORAGE:
AVAIL=`df --output=avail kavian/ | egrep '^[0-9]*$'`
LOGSIZE=$(du -k /kavian/log.txt |  awk '{ print $1 }')

___________________________________________________________________________________________________________________________________________________________________________________

#!/bin/bash
myscriptname=`basename $0`

for i in `ls -A`
do
	if [ $i = $myscriptname ]
	then
		echo "Sorry, can't rename myself"
		elsif [ $i != $myscriptname ]
		newname=`echo $i | tr A-Z a-z`
		mv $i $newname
	fi
done


#!/bin/bash
COUNT=0
PASSFILE="/etc/passwd"

for user in `cat $PASSFILE | cut -f 1 -d ':'`
do
	echo $user
	let "COUNT += 1"
done

echo There are $COUNT users registered on the system


#!/bin/bash
NUM=100
MIN=20

until [ "$NUM" -eq "$MIN" ]
do
	echo $NUM
	let "NUM -= 1"
done


#!/bin/bahs
netstat -ant | grep :80 > /dev/null
APACHESTATUS="$?"

if [ "$APACHESYATUS" -eq 0 ]
then
	echo Apache is runnung
fi


#!/bin/bash
BADPARAM=165
echo $#

if [ $# != 2 ]
then
	echo this script requires 2 arguments
	exit $BADPARAM
fi

seq $1 $2


##Menu

#!/bin/bash

PS3='Please select a choice: '
LIST="Choice1 Choice2 Quit"
select i in $LIST
do
	if [ $i = "Choice1" ]
	then
		echo Hello
	elif [ $i = "Choice2" ]
		then
		echo Goodbye
	elif [ $i = "Quit" ]
		then
			exit
	fi
	break
done


##renaming and moving
mv test{1,2} linux{1,2}


for file in `ls -A test*`
do
	mv $file $file.old
done



#!/bin/bash
BADARG=165
if [ $# != 2 ]
then
	echo "at least 2 positional parameter is required"
	exit $BADARG
fi

for file in `ls -A $1*`
do
	mv $file $file.$2
done


#!/bin/bash
BADARG=165
SITE="www.google.com"

ping -c 2 $SITE > /dev/null

if [ $? != 0 ]
then
	echo There seems to be Internet connectivity issues!
	exit $BADARG
fi


#!/bin/bash

netstat -ant | grep 3306 > /dev/null
MYSQLSTATUS=`echo $?`
SERVICENAME="mysqld"
COUNT=0
THRESHOLD=2

if [ $MYSQLSTATUS != 0 ]
then
	while [ $COUNT -le $THRESHOLD ]
	do
		service $SERVICENAME start
		if [ $? != 0 ]
		then
			let "COUNT +=1"
		else
			exit 0
		fi
	done
	echo Problems starting $SERVICENAME | mail -s "$SERVICENAME Failure" root

else
	echo $SERVICENAME is running
fi

lftp linuxdebian@192.168.1.20


#!/bin/bash
DESTBACKUPDIR=~/backup
SOURCEDIR="/temp2 /etc"
BACKUPFILE=final.backup.`date +$F`.tgz
DESTBACKUPHOST=192.168.1.20
THRESHOLD=7

function checkbackupdir() {
if [ ! -e $DESTBACKUPDIR ]
then
	echo creating backup directory because it doesn\'t exist!
	mkdir ~/backup
	COUNT=0
else
	COUNT=`ls $DESTBACKUPDIR/final.* | wc -l`
fi
}

function backup() {
if [ $COUNT -le $THRESHOLD ]
then
	tar -czvf $DESTBACKUPDIR/$BACKUPFILE $SOURCEDIR > /dev/null
	if [ $? != 0 ]; then echo Problems creating backup file; fi
	scp $DESTBACKUPDIR/$BACKUPFILE $DESTBACKUPHOST;
	if [ $? != 0 ]; then echo Problems copying backup file to backup host; fi
fi
}

checkbackupdir
backup
 

##public/private
cd ~/.ssh/
ls
cat id_rsa.pub
copy the string to the remote system

ssh 192.168.1.120
cd .ssh/

nano authorized_keys
paste the key

### CHANGE COLOR OF RESULT
print('\033[31m' + 'some red text')
print('\033[39m') # and reset to default color
