<h1>SAP ABAP Work Log Tracker</h1>

<h2>1. Application Entry Point</h2>

<p>
The application starts from Transaction Code <strong>ZTRACK_995</strong>,
which opens the Module Pool Program <strong>ZNN_TASK_995</strong>.
</p>

<p>
The user is navigated to <strong>Screen 0100</strong>, which acts as the main menu
and selection screen of the application.
</p>

<hr>

<h2>2. Screen 0100 - Main Selection Screen</h2>

<p>
Screen 0100 allows the user to work with four different functionalities:
</p>

<ul>
<li>CR Tracking</li>
<li>INC Tracking</li>
<li>Additional Task Tracking</li>
<li>T-Code Navigation</li>
</ul>

<p>
The screen contains the following action buttons:
</p>

<ul>
<li>Display</li>
<li>Create</li>
<li>Exit</li>
</ul>

<hr>

<h2>3. Display Functionality</h2>

<h3>3.1 CR Tracking Display</h3>

<p>
The user enters a CR Number in the CR Tracking field and clicks
<strong>Display</strong>.
</p>

<p>
The application reads the entered CR Number and identifies the
selection type as <strong>CR</strong>.
</p>

<p>
The system fetches matching records from:
</p>

<pre>
ZCRTRACKER_995
</pre>

<p>
The retrieved records are stored in an internal table and displayed
on Screen 0110 using ALV Grid.
</p>

<h3>Flow</h3>

<pre>
Screen 0100
     |
     | Display
     v
Read CR Number
     |
     v
Select Data From ZCRTRACKER_995
     |
     v
Screen 0110
     |
     v
ALV Output
</pre>

<hr>

<h3>3.2 INC Tracking Display</h3>

<p>
The user enters an Incident Number and clicks Display.
</p>

<p>
The application identifies the selection type as INC and reads data from:
</p>

<pre>
ZINCTRACKER_995
</pre>

<p>
The corresponding records are displayed in ALV on Screen 0110.
</p>

<h3>Flow</h3>

<pre>
Screen 0100
     |
     | Display
     v
Read INC Number
     |
     v
Select Data From ZINCTRACKER_995
     |
     v
Screen 0110
     |
     v
ALV Output
</pre>

<hr>

<h3>3.3 Additional Task Display</h3>

<p>
The user enters a task description and clicks Display.
</p>

<p>
The system reads data from:
</p>

<pre>
ZOTHER_TASK_995
</pre>

<p>
Matching records are displayed in ALV on Screen 0110.
</p>

<h3>Flow</h3>

<pre>
Screen 0100
     |
     | Display
     v
Read Task Description
     |
     v
Select Data From ZOTHER_TASK_995
     |
     v
Screen 0110
     |
     v
ALV Output
</pre>

<hr>

<h2>4. T-Code Navigation Functionality</h2>

<p>
The user enters a valid SAP Transaction Code and clicks Display.
</p>

<p>
The application validates the transaction code using SAP table:
</p>

<pre>
TSTC
</pre>

<p>
If the T-Code exists, the application opens:
</p>

<pre>
SE93
</pre>

<p>
and automatically populates the entered transaction code.
</p>

<h3>Flow</h3>

<pre>
Screen 0100
     |
     | Enter T-Code
     |
     | Display
     v
Validate T-Code
     |
     v
SE93
     |
     v
Transaction Definition
</pre>

<hr>

<h2>5. Screen 0110 - ALV Display Screen</h2>

<p>
Screen 0110 is the reporting screen of the application.
</p>

<p>
It displays records in ALV format using:
</p>

<pre>
CL_GUI_ALV_GRID
</pre>

<p>
The screen contains:
</p>

<ul>
<li>Change</li>
<li>Back</li>
<li>Exit</li>
</ul>

<hr>

<h2>6. Change Functionality</h2>

<p>
When the user clicks Change from Screen 0110, the application
loads the selected business object based on the active tracking type.
</p>

<hr>

<h3>6.1 Change CR Record</h3>

<p>
If the active type is CR:
</p>

<ul>
<li>Data is read from ZCRTRACKER_995</li>
<li>Screen 0200 is opened</li>
<li>Existing values are populated automatically</li>
</ul>

<h3>Flow</h3>

<pre>
Screen 0110
     |
     | Change
     v
Read CR Record
     |
     v
Screen 0200
</pre>

<hr>

<h3>6.2 Change INC Record</h3>

<p>
If the active type is INC:
</p>

<ul>
<li>Data is read from ZINCTRACKER_995</li>
<li>Screen 0300 is opened</li>
<li>Existing data is displayed</li>
</ul>

<h3>Flow</h3>

<pre>
Screen 0110
     |
     | Change
     v
Read INC Record
     |
     v
Screen 0300
</pre>

<hr>

<h3>6.3 Change Additional Task</h3>

<p>
If the active type is Additional Task:
</p>

<ul>
<li>Data is read from ZOTHER_TASK_995</li>
<li>Screen 0400 is opened</li>
<li>Existing values are populated</li>
</ul>

<h3>Flow</h3>

<pre>
Screen 0110
     |
     | Change
     v
Read Task Record
     |
     v
Screen 0400
</pre>

<hr>

<h2>7. Create Functionality</h2>

<p>
The application supports record creation for all three categories.
</p>

<hr>

<h3>7.1 Create CR</h3>

<pre>
Screen 0100
     |
     | Create
     v
Screen 0200
     |
     | Save
     v
Insert Into ZCRTRACKER_995
</pre>

<hr>

<h3>7.2 Create INC</h3>

<pre>
Screen 0100
     |
     | Create
     v
Screen 0300
     |
     | Save
     v
Insert Into ZINCTRACKER_995
</pre>

<hr>

<h3>7.3 Create Additional Task</h3>

<pre>
Screen 0100
     |
     | Create
     v
Screen 0400
     |
     | Save
     v
Insert Into ZOTHER_TASK_995
</pre>

<hr>

<h2>8. Save Functionality</h2>

<p>
The application determines the operational mode:
</p>

<ul>
<li>CREATE</li>
<li>CHANGE</li>
</ul>

<p>
Depending on the mode:
</p>

<ul>
<li>INSERT is executed for new records</li>
<li>UPDATE is executed for existing records</li>
</ul>

<p>
After successful processing:
</p>

<pre>
COMMIT WORK 
</pre>

<p>
is executed to permanently save the changes to the database.
</p>

<hr>

<h2>9. Database Processing</h2>

<h3>Create Mode</h3>

<pre>
INSERT
</pre>

<p>
Creates a new database record.
</p>

<h3>Change Mode</h3>

<pre>
UPDATE
</pre>

<p>
Updates an existing database record.
</p>

<h3>Commit</h3>

<pre>
COMMIT WORK
</pre>

<p>
Permanently saves all changes to the SAP database.
</p>

<hr>

<h2>10. Navigation Flow</h2>

<h3>Display Flow</h3>

<pre>
0100
 |
 | Display
 v
0110
</pre>

<h3>Change Flow</h3>

<pre>
0110
 |
 | Change
 v
0200 / 0300 / 0400
 |
 | Save
 v
0100
</pre>

<h3>Create Flow</h3>

<pre>
0100
 |
 | Create
 v
0200 / 0300 / 0400
 |
 | Save
 v
0100
</pre>

<h3>Back Flow</h3>

<pre>
0110
 |
 | Back
 v
0100
</pre>

<h3>Exit Flow</h3>

<pre>
Any Screen
 |
 | Exit
 v
Leave Program
</pre>

<hr>

<h2>11. Technical Components Used</h2>

<ul>
<li>Module Pool Programming</li>
<li>Screen Painter</li>
<li>PBO / PAI Processing</li>
<li>Form Routines</li>
<li>Internal Tables</li>
<li>Open SQL</li>
<li>ALV Grid (CL_GUI_ALV_GRID)</li>
<li>Database Tables</li>
<li>Transaction Navigation (SE93)</li>
<li>CRUD Operations</li>
</ul>
