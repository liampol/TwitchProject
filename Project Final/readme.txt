README
======
Group Name: Team 1
------------------
Members: Jamie Hutchings, Brett Ellis, Liam Pol, Connor Smith
-------------------------------------------------------------


DATA COLLECTION
===============

Folders
-------
- Raw_Data
	- Chat
		- chat.csv
	- Emotes
		- channelEmotes.csv
		- channelEmotesPlans.csv
		- thirdPartyEmotes.csv
		- twitchEmotes.csv
	- Scripts
		- bot.js
		- import_requests.py
		- get_emotes.py
	- Streams
		- [channel name].csv


Installation
------------
In order to run the data collection scripts you will need to make sure you have python 3
installed with pip and node js along with npm (nodes package manager).

Python3 - https://www.python.org/downloads/ (Should come with pip)
Node - https://nodejs.org/en/
Npm - https://www.npmjs.com/get-npm

For the node script 'bot.js' you will need to navigate to to the scripts folder and run
the following commands in your shell.

npm install tmi.js
npm install csv-write-stream
npm install date-and-time

For the python packages 'import_requests.py' and 'get_emotes.py' you will need to run
the following commands in yours shell.

pip3 install requests
pip3 install datetime
pip3 install beautifulsoup4


Instructions
------------
In order to begin collecting the data you have to run the following commands in the 
Scripts directory. NOTE: 'import_requests.py' and 'bot.js' will have to be run in their
own shells.

python3 get_emotes.py (this will run once after a few seconds)
python3 import_requests.py (this script will run until you close it manually)
node bot.js (this script will run until you close it manually)

See each individual file's documentation for more information.


DATA PROCCESSING
================

Folders
-------
- Final_Data
	- API_PING.csv
	- CHANNEL.csv
	- CONTAINTS.csv
	- EMOTE.csv
	- FCHAT.csv
	- SUBSCRIPTION.csv


Installation
------------
See first two cells of the 'Proccessing' workbook


Instructions
------------
Open jupyter-lab in the main 'Project Final' directory and open the 'Proccessing'
workbook.

Run each cell in the work book under 'Data Cleaning' once (this may take a while) and the plots can be run
as many times as needed under the 'Data Presentation'.


DATA MODEL
=========

The final data model is a relational database model. It is made up of 5 csv files:

1) API_PINGS.csv
2) CHANNELS.csv
3) CONTAINS.csv
4) FCHAT.csv
5) SUBSCRIPTION.csv
6) EMOTE.csv

DATA DICTIONARY
===============

Definitions
-----------

PK = Primary Key (The value used to identify a particular record)
FK -> (<Relation>.csv) = Foreign Key pointing to the PK of another .csv 
CK = A Candidate Key (A unique value across all records for the table)

-----------


1) API_PINGS.csv a relation where every record is a call to the Twitch API for
streamer metadata. The API requested data every 5 minutes.

Columns
-------

channel - The name of the streamer for which the API is getting the metadata 
for. (FK -> CHANNEL.csv)
channel_id - The ID of the streamer for which the API is getting the metadata 
for. (FK -> CHANNEL.csv.channel_id)
game_id - The ID of the game which the streamer is playing.
live_or_not - Whether the streamer was live or not when the API was called.
viewer_count - The amount of viewers the streamer when the API was called.
language - The language of the streamer.
request_time - The datetime for the particular API call (i.e when the data request was made).
start_time - When the streamer started streaming.

-------


2) CHANNEL.csv is a relation which stores all of the twitch channels in which data 
was recorded for.

Columns
-------

channel - The name of the streamer for which we have recorded data 
for. (PK)
channel_id - The ID of the streamer for which we have recorded data 
for. (CK)

-------


3) CONTAINS.CSV is a relation containing every instance of a message which 
contains an emote.

Columns
-------

observation - The message ID / sequential number. (PK)
text - The specific emote used. (FK -> EMOTE.csv)

-------


4) FCHAT.csv is a relation that contains every chat message sent for each of
the streamers in the database / CHANNEL.csv

Columns
-------

observation - The ID for the message, a sequential number for every 
message. (PK)
channel - The name of the channel for which the message was sent 
to. (FK -> CHANNEL.csv)
user - the name of the user who sent the message.
time - the time when the message was sent to the streamer.

-------


5) SUBSCRIPTION.csv is a relation that contains all the data about 
the subscription plans for each streamer.

Columns
-------

name - The name of the streamer who's emote set this is. (FK -> CHANNEL.csv)
cost - The price of the emote set. This is a monthly cost in $USD.
subscription_id - The id of the emote set. (PK)

-------


6) EMOTE.csv is a relation which contains all emotes from both Twitch and BTTV (Third party 
emote site.)

Columns
-------

channel - Name of the channel for which the emote 
belongs to. (FK -> CHANNEL.csv)
channel_id - ID of the channel for which the emote 
belongs to. (FK -> CHANNEL.csv.channel_id)
text - text used to produce the emote in Twitch chat. (PK)
emote_id - the ID of the emote. (CK)
subscription - the ID of the emote set for which the emote belongs 
to. (FK -> SUBSCRIPTION.csv) 