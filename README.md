imdb-parse
==========
Description:

The goal of this project is to have an event based parser for IMDB alternative interfaces (plaintext files) available from
http://www.imdb.com/interfaces

Implementation:

A php script intended to be run via php-cli parses an entire file from begining to end. It converts the data into instances of classes (objects). These classes are defined in im-include folder and are structured to be as simple as possible. An event is triggered when an object is "ready". In the im-config.php file, one or more folders with a set of php files with expected names (such as actress-ready.php) can be configured; this was the choice made to have a very simplistic method of registering a 'hook' to an event.

Usage Steps:

1. copy im-config-sample.php to im-config.php
2. download actresses.list or actors.list from above link
3. run from command line: #$php actress-cli.php actresses.list 
4. it will just display the data with im-config-sample.php values as included.
if you want to do anything else with the data, like put it into a database, you need to add another eventdir in the config file. For example, to have it insert into mongodb add the line:
	$cfgIMEventDir[] = 'im-event-mongoinsert';
to im-config.php
5. you may be interested in NOT having the data displayed (dumped to screen) as it parses, in this case just REMOVE the line
	$cfgIMEventDir[] = 'im-event-vardump';
6. you may desire to have some visual indication of progress, there is an eventdir included for this purpose.
	$cfgIMEventDir[] = 'im-event-justprogress';  
7. if you want to have the data be stored somewhere other than Mongo, say MySQL,start by making a copy of the folder im-event-vardump and add code there to do the inserts. There are events that fire before and after parsing occurs (parse-start.inc.php, parse-done.inc.php) in addition to a persistent object variable that can be used to store things like your db connection variables ($this->eventdata). see the im-event-mongoinsert folder contents for an example on how that works.
