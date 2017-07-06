# Group and Roster Generator
This is a simple [Pentaho PDI (Kettle)](http://community.pentaho.com/projects/data-integration/) Job and transformation that takes an xMatters Group Export file and generates a .csv file of containing one line for each xMatters Group with the Group Name in the first column, with the list of members across all shifts (roster) in the second column.
One point of note is that the xMatters Group Export only contains shifts that contain members.  So, if you have a Group with 1000 members, but none of them are in a shift, it will come out as an "Empty group".

# Pre-Requisites
* [Pentaho Data Integration, v7.x+](http://community.pentaho.com/projects/data-integration/)
* An xMatters Group Export in the local file system

# Files
* [GroupAndRoster.kjb](GroupAndRoster.kjb) - The Kettle Job that starts the process.  Takes one named argument `grpfname` that should point to the Group Export file.
* [GroupAndRoster.ktr](GroupAndRoster.ktr) - The Kettle Transformation that is called by GroupAndRoster.kjb that reads the export file and generates the resulting output report.

# How it works
The Kettle Job does some preliminary checking to make sure that the file exists. 
After that, the Transformation is called which reads in and parses, sorts, and deduplicates the Export file.
It then uses a Group By step to combine the members across all shifts into a single quote enclosed, comma separated list of members.
It then writes the output into the same directory as the input, with the same name, except that the extension becomes `.rpt.csv`.

# Installation
Download and install Pentaho.  Use the links above, and the full documentation is [here](http://wiki.pentaho.com/display/EAI/Latest+Pentaho+Data+Integration+%28aka+Kettle%29+Documentation).
Once Pentaho is installed, put the .kjb and .ktr files into the `data-integration` folder (where PDI was installed).

## Usage
1. Generate an Group Export from your xMatters instance (instructions are [here](http://help.xmatters.com/OnDemand/groups/groups-create-delete-export.htm))
2. Note where you put the Group Export file.
3. Go to the Pentaho `data-integration` folder.
4. Depending on whether you are using Windows, or a Linux/Unix derivative, you will need to run kitchen.bat or kitchen.sh respectively.
   ```
   cd /Pentaho/data-integration
   # Example: kitchen.sh GroupAndRoster.kjb -param:grpfname="<full path to yoru group export file>"
   ./kitchen.sh -file=GroupsAndRoster.kjb -param:grpfname="Group Export 20170705133635.csv"
   ```

# Testing
Testing is simple, just run the command as above.
If the process starts, you will see output that will look something like this...
   ```
2017/07/05 23:25:16 - GroupsAndRoster - Start of job execution
2017/07/05 23:25:16 - GroupsAndRoster - Starting entry [grpfname exists]
2017/07/05 23:25:16 - GroupsAndRoster - Starting entry [Log filename found]
2017/07/05 23:25:16 - grpfname exists - The Group Export Filename was found: Group Export 20170705133635.csv
2017/07/05 23:25:16 - GroupsAndRoster - Starting entry [Create Report]
2017/07/05 23:25:16 - Create Report - Loading transformation from XML file [file:///xMatters/OOTB20170523/Pentaho/data-integration/GroupsAndRoster.ktr]
2017/07/05 23:25:16 - Create Report - Using run configuration [Pentaho local]
2017/07/05 23:25:16 - Create Report - Using legacy execution engine
2017/07/05 23:25:16 - GroupsAndRoster - Dispatching started for transformation [GroupsAndRoster]
2017/07/05 23:25:16 - Get filename.0 - Finished processing (I=0, O=0, R=1, W=2, U=0, E=0)
2017/07/05 23:25:16 - Log grpfname.0 - 
2017/07/05 23:25:16 - Log grpfname.0 - ------------> Linenr 1------------------------------
2017/07/05 23:25:16 - Log grpfname.0 - Enterred transformation
2017/07/05 23:25:16 - Log grpfname.0 - 
2017/07/05 23:25:16 - Log grpfname.0 - grpfname = Group Export 20170705133635.csv
2017/07/05 23:25:16 - Log grpfname.0 - 
2017/07/05 23:25:16 - Log grpfname.0 - ====================
2017/07/05 23:25:16 - Log grpfname.0 - Finished processing (I=0, O=0, R=1, W=1, U=0, E=0)
2017/07/05 23:25:16 - Read Group Export.0 - Opening file: file:///xMatters/OOTB20170523/Pentaho/data-integration/Group Export 20170705133635.csv
Jul 05, 2017 11:25:18 PM org.apache.cxf.endpoint.ServerImpl initDestination
INFO: Setting the server's publish address to be /marketplace
Jul 05, 2017 11:25:18 PM org.apache.cxf.endpoint.ServerImpl initDestination
INFO: Setting the server's publish address to be /lineage
Jul 05, 2017 11:25:18 PM org.apache.cxf.endpoint.ServerImpl initDestination
INFO: Setting the server's publish address to be /i18n
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/xMatters/OOTB20170523/Pentaho/data-integration/launcher/../lib/slf4j-log4j12-1.7.7.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/xMatters/OOTB20170523/Pentaho/data-integration/plugins/pentaho-big-data-plugin/lib/slf4j-log4j12-1.7.7.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2017/07/05 23:25:19 - Read Group Export.0 - Finished processing (I=5865, O=0, R=1, W=5864, U=1, E=0)
2017/07/05 23:25:19 - Split Filename.0 - Finished processing (I=0, O=0, R=5864, W=5864, U=0, E=0)
2017/07/05 23:25:19 - Sort group members.0 - Finished processing (I=0, O=0, R=5864, W=4306, U=0, E=0)
2017/07/05 23:25:19 - Group by.0 - Finished processing (I=0, O=0, R=4306, W=1959, U=0, E=0)
2017/07/05 23:25:19 - Roster output.0 - Finished processing (I=0, O=1960, R=1959, W=1959, U=0, E=0)
2017/07/05 23:25:19 - GroupsAndRoster - Starting entry [Success]
2017/07/05 23:25:19 - GroupsAndRoster - Finished job entry [Success] (result=[true])
2017/07/05 23:25:19 - GroupsAndRoster - Finished job entry [Create Report] (result=[true])
2017/07/05 23:25:19 - GroupsAndRoster - Finished job entry [Log filename found] (result=[true])
2017/07/05 23:25:19 - GroupsAndRoster - Finished job entry [grpfname exists] (result=[true])
2017/07/05 23:25:19 - GroupsAndRoster - Job execution finished
2017/07/05 23:25:19 - Kitchen - Finished!
2017/07/05 23:25:19 - Kitchen - Start=2017/07/05 23:25:06.608, Stop=2017/07/05 23:25:19.451
2017/07/05 23:25:19 - Kitchen - Processing ended after 12 seconds.
   ```

# Troubleshooting
The easiest way to troubleshoot Pentaho projects is to bring up the Pentaho development environment, or Spoon.
To do that, in the `data-integration` directory, run `SpoonConsole.bat` or in linux `export SPOON_CONSOLE=1;spoon.sh`.
Once the IDE starts up, just use the File Open command to bring in the GroupAndRoster.kjb file, then hit the Run command.

Ask questions here if you get stuck.
