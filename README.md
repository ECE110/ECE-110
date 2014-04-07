ECE-110
=======

Communication and stuff

This is longer than I meant it to be.

If you are having trouble understanding or reading my code use this.

It's entirely possible that there are errors in this and the code is kinda hard to read since it isn't particularly organized so...
I'm going to explain what I did.

first section is telling it what libraries to use and what pins to use for my LCD display and for the Xbee module, remember to set these to which ever pins you are using for your Xbee.

Xbee.begin(9600) was added in the "void setup" portion to setup the xbee module

The long portion in the setup is for the lcd display so you don't need to worry about that.

The variables that I use for the communication code are:

int TIE=0 <-- my spot in line

boolean Xb=true <-- a boolean(a variable that holds either the value true or false) saying whether or not the robot should be sending out a signal with its Xbee module

boolean halt=true <-- controls whether or not the bot stops when it gets to its staging area/black line

int W=0 <-- the number of hash marks/perpendicular black lines my bot has passed

PART 1:
the first if statement "if ((W==12-TIE))"; encompasses all of part 1 so if this condition is not satisfied none of part 1 runs, meaning this only occurs when the bot is at its final destination. 

the reason it is 12-TIE is that for me there are 11 hash marks along my path, this varies group by group, so if I am first in line I would want to finally stop at the 11th hash mark which would be 12-TIE

this statement says that once it gets to its final destination it should stop " servoLeft.writeMicroseconds(1500);
servoRight.writeMicroseconds(1500);" and that it should display my spot in line

it then says that "if (Xb == true)" , which is the default case that i set when i created the variable, that the Xbee module should send out its spot in line. The next if statement says that if the Xbee module recieves a signal that is 20 higher than its place in line(20 was an arbitrary increment) then Xb becomes false which stops the previous if statement from occuring which stops the bot from sending out its place in line

the random 5 second delay is there as a band-aid-esque solution to my robot randomly spinning, not sure why it spins but this shouldn''t be neccessary for other people. 

PART 2: 

the next part is an if statment within an else statement that encompasses the rest of the code(the bracket that ends this else statement is way at the end of my code) so whenever Part 1 doesn''t occur this code runs(so it runs at all times except when the bot gets to its final spot in line)

"  if (W==6 && TIE >1 && halt== true)";

says that when I am at my staging area(the 6th hashmark for me, this will vary by group) and when my spot in line is after the first person and when the boolean halt is true this portion of the code runs

I set the servomotors to stop in this portion b/c it has to wait at its staging area for a little bit of time before it can go.

the next nested if statement 
"if ((Serial.available() && Serial.read() == TIE-1))"

says that if the bot gets a signal that is one less than its spot in line, so a signal indicating that the bot before it has reached its spot(this signal is sent in part 1) then the bot should send a number that is equal to its signal + 19  ( "Xbee.print(TIE+19);") 
so for example if this were the second bot, after it recieved the signal 1, it would send out 2+19 = 21 which is the same as (TIE+20) for the first bot(21) so the first bot will stop sending its signal at this point/tells the first bot that the second bot has recieved the signal. 

it then sets halt to false "halt = false;" which stops part 2 from running

PART 3: 

My line following/sensing/counting code. This is where your code goes. This code runs only when parts 1 and 2 do not run, so before the staging area and between the staging area and the final spot in line.

NOTE: if you need code for a counter to count the black lines you can base it off of the code in my Part 3, particularly

" if (QTI == 1111)
{
  //Serial.println("BLACK");
  V=1; // QTI sensors see a perpendicular black line
  C=0; // reset C to 0 to add another line to the count
}
else if (C==0 && V==1)
{
  W=W+1; // add to the count of black lines
  //Serial.println("W");
  //Serial.println(W);
 // Serial.println(QTI);
  V=0;
  C=1;
  
} "

(QTI == 1111) means that my bot sees a black line/hash so use whatever signifies a black line in your code here

V and C function about the same as booleans but this code was written by Eeyi and I before I knew about booleans.
the else if statement says that as soon as the bot passes over the black line, if it was on a black line before((C==0 && V==1)) to add 1 to the counter("W+1") and then to reset V and C so that it only adds once per black line that it passes






