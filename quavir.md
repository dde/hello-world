# VoiceXML Quality Assurance (VQA)
## Execution

VQA tests the execution of any VoiceXML 2.1 application under the control of a test plan.

### Test Plans

VQA will read and execute one or more test plans in the order they are listed on the command line. See [[sandbox:quavir_test_plan|test plans]] for a description of a test plan file.

#### Execution
A test plan contains the starting URL of a VoiceXML server. The test plan executer is a modified VoiceXML interpreter. It reads and follows the XML test plan to
*   request a VoiceXML document from the server
*   verify arbitrary elements of the received document, matching structure, attribute values, and element CDATA, using regular expressions
*   provide the input expected by the VoiceXML document
*   execute the VoiceXML document to determine the next URL expected by the server
*   connect to the server and post a request for the next document
*   receive the next document from the server
*   continue following the plan until a disconnect test plan step is executed
*   execute any subsequent dialogs in the plan until the plan is complete.

### Command Line
Using the distributed <tt>.jar</tt> file, the command to start the test tool in a Terminal or Command Tool window is:

 <tt>java -jar distribution.jar com.voxeo.application.Driver [options] [test-plans]</tt>
#### Options
*   -l the output logging level; can be ''trace'', ''debug'', ''info'', ''warn'', or ''error'', in increasing importance; log messages will be written for the selected level and any higher importance; the default is ''info''; the ''trace'' level writes VXML and &lt;data&gt; responses from the server(s)
*   -t the number threads to start; the default is 1; see Multiple Threads below about thread logging
*   -h a list of the options is displayed without further execution

### Graphical Interface
Using the distributed <tt>.jar</tt> file, the command to start the test tool graphical interface is:

 `java -jar distribution.jar`

Options can be specified through the Preferences window, and changed as needed.

When started, the program displays the Edit tab, one of three tabs, Edit, Execution, and Regression, that are available.
The split display of the Edit tab shows the currently loaded test plan in the left panel, and the elements of the currently selected test plan statement in the right.  The right panel has a tab for each statement type.  The buttons at the bottom load a test plan and save it after editing.

[[Image:tt_start_2014-02-03_0918.png|thumb|center|alt=The initial window upon start, the Edit tab with its split panel display.|The initial window showing the Edit tab with its split panel display after the New Plan button has been clicked and the test plan statement selected.]]

### Test Plan Buttons
*   The ''Select Plan...'' button displays a File Chooser window to allow the selection of an XML test plan.  The selected test plan is loaded and displayed in the left panel.

*   The ''New Plan'' button will initialize the left panel with a test plan template.

*   The ''Save'' and ''Save As...'' buttons are enabled after a loaded test plan has been modified.  ''Save'' will save the loaded test plan back to the file from which it was loaded.  ''Save As...'' will display a File Chooser window so that the edited test plan can be saved to a different file.

*   The File menu also includes the test plan buttons as an alternate way to invoke the same function.

### Editing
#### Change an Existing Statement
Select a test plan statement to be edited by clicking the statement.  The entire statement will be highlighted, the statement tab will be selected in the right panel, and the elements of the statement will be displayed in the statement tab's controls.  The statement elements can be changed with the controls.  After the desired changes have been made, the ''Update'' button reconstructs the statement in the test plan with the changed values.
#### Add a New Statement
To add a new statement, select the desired statement tab, update the controls, then click either the ''Add Before'' or ''Add After'' button.  A new statement will be constructed from the controls and added before or after the currently selected statement.  If the new statement is to be added before or after a statement of the same type, selecting the current position loads the controls, so selecting the position should be done first, before the controls are modified.  As an alternative, press the control button while selecting the current statement.  In this case, the statement is selected, but the controls are not loaded.
#### Delete a Statement
The ''Delete'' button operates on he currently selected statement.  Clicking ''Delete'' will remove the selected statement from the test plan.
### Executing a Test Plan
To execute a test plan, select the ''Execution'' tab.  The Execution panel is a display of the execution log and some control buttons.

[[Image:tt_exec_2014-02-03_1010.png|thumb|center|alt=The execution window displays the execution log.|The execution window displays the execution log.]]

The name of the test plan to be used for the test is displayed in the lower left on the boundary of the log panel.  If a test plan was loaded into the Edit tab, the name of that plan is initially displayed.  A plan can be selected with the ''Select Plan...'' button which will display a File Chooser window.

After a test plan has been selected, click the ''Start'' button to begin execution.  This is equivalent to running the program at the command line, but the log output is captured and displayed in the Execution tab's logging panel.

Execution can be stopped before completion by clicking the ''Stop'' button.

When execution is complete, the ''Save Log...'' button is enabled, and will save the execution log to a file, which can then be used in regression comparisons.

#### Logging

Output during test plan execution is tagged with a level and written to the log if the log level option includes the level assigned to the output.  From lowest to highest, the log levels are ''debug'', ''info'', ''vxml'', ''warn'', and ''error''.  The default level is ''info''.  Thus, by default, all messages except ''debug'' messages are logged.

A log record begins with a time stamp, followed by the log level, and the logged data.  Test plan execution messages and application prompt output are logged at the ''info'' level.  Application output produced by the VoiceXML &lt;log&gt; element is logged at the ''vxml'' level.  The ''warn'' level is used for information that could affect the test plan execution.  The ''error'' level is for problems in the test plan or execution.  The ''debug'' level provides more detailed information about the execution as well as the source for all VoiceXML files executed by the program.

The first log record written for is test is the name of the test plan file and a time stamp, which together uniquely identify the log.

### Regression Comparison
The Regression tab compares two execution logs and displays the differences.  When testing an upgraded application, one normally begins by testing it against a previously successful test plan.  This new test produces a log which is then compared against the previous log from the same test plan plan.  A regression comparison that shows no differences is a demonstration that the upgrade did not affect the existing operation of the application.  Then, a test plan that has been enhanced for the new functions of the application is run, and when successful, establishes a benchmark for the next round of regression testing.

[[Image:tt_regress_2014-02-03_1053.png|thumb|center|alt=The regression window displays the comparison of two execution logs.|The execution window displays the comparison of two execution logs.]]

The ''Select Newer...'' button displays a File Chooser window to select the more recent log in the left panel for comparison.  The ''Select Older...'' button selects the older log in the right panel.  The selected log names are displayed in the panel borders at the bottom.  If a test plan has just been executed, the input to the ''newer'' panel is implicitly the output of that test plan, until another selection has been made.  If a selection has been made, the most recent execution log can be reselected by clicking ''Select Newer...'', then canceling the selection. 

When both logs have been selected, the ''Compare'' button is enabled.  Clicking it will start the comparison.  The results are show in the two panels.  Records that are different are highlighted in light green.  Records that appear in one file and not the other are highlighted in light red.  Equals records are compressed to a single line so that the displays reflect the differences between the files.  Time stamps are not part of the comparison.

### Additional Features
#### Preferences
Preferences for the programs are set from the Preferences window, which is a selection on the File menu.  Preferences take effect immediately after they are set.

#### Multiple Threads
One of the preference options is the number of threads of execution to start.  By default, this is one.  If the number is greater than one, the selected test plan will be executed on each thread simultaneously.

The output of the first thread will be displayed in the log window.  The output from threads 2 through n will be written to separate log files named from the application attribute of the &lt;test plan&gt; statement.  The names of the additional logs are displayed in the first thread's log.
#### Recording Utterances

If the `recordutterance property` is set to "true" in an application, the tool simulates/generates an utterance recording.  The test plan input utterance is divided into space-delimited words and the simulated recorded utterance is given one second for each word, plus 1 second leading and 1 second trailing time.  A byte array of silence representing a simulated recorded utterance is generated.  The ''lastresult$'' properties <tt>recording</tt>, <tt>recordingsize</tt>, and <tt>recordingduration</tt> are assigned from the generated audio and its characteristics.

For the `recordutterancetype` property, `audio/basic` and <tt>audio/x-wav</tt> are supported.  The default is <tt>audio/x-wav</tt>.
## Validation
The program has been validated by running the VoiceXML 2.0 and 2.1 Implementation Report tests.  The test plans and the output logs are examples of the test tool.  
*   VoiceXML 2.0 Validation - [[File:irtests_2.0.xml|VoiceXML 2.0 IR test plan]] - [[File:full_irtest_2.0_log_c.txt|VoiceXML 2.0 IR test output log]].
*   VoiceXML 2.1 Validation - [[File:irtests_2.1.xml|VoiceXML 2.1 IR test plan]] - [[File:full_irtest_2.1_log.txt|VoiceXML 2.1 IR test output log]].
