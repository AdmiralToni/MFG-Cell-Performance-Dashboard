# MFG-Cell-Performance-Dashboard
Compiling manufacturing data to be used for cell performance analysis

This repo is dedicated to replicating a data pipelining project that I undertook in a real aerospace manufacturing setting.
In accordance with a NDA, ALL of the scripts, data, charts etc. were created independently using only my personal resources.
NONE of the part tolerences or performance standards reflect any real engineered part designs.
EVERYTHING in this repo was built to simulate part flow within a manufacturing cell, ETL raw part data, and display it for business and engineering analysis. 

    Quick Scenario Description:
        In the hypothetical cell, two parts recieve cooling hole features, undergo a dimensional inspection, and a performance inspection.
        Process machines conduct up to 3 different machining operations to complete the cooling hole features and store their data on a dedicated network drive.
        Inspection machines generate part inspection reports and store individual part reports on a separate dedicated network drive.
        Performance machines output part performance results to a database on a separate dedicated network drive.
    
    End Goal:
        1. Organize raw machining, dimensional, and performance data originating from 3 different network locations into one central database.
        2. Refresh the database on a schedule.
        3. Display weekly cell thruput, avg. cycle time per operation, dimensional and performance trends based on process machine, weekly machine utilization, etc.
    
    Detailed Scenario Description:
        In this hypothetical cell, there are two hypothetical types of hardware (PN1xxxx and PN2xxxx) that recieve four rows of cooling holes, each with different hypothetical drawing specs. 
        In the cell, there are:
          3 Process Machine lines with tooling dedicated to a specific manufacturing operation (OP1, OP2 or OP3). Some machines, not all, are capable of completing two different OPs.
                OP1 = 'MachA', 'MachB', 'MachC', 'MachH'
                OP2 = 'MachD', 'MachE', 'MachF', 'MachA'
                OP3 = 'MachG', 'MachH', 'MachI', 'MachD'
          1 line of dimensional inspection machines capable of inspecting either PN1xxxx or PN2xxxx.
                'Insp. Mach 1', 'Insp. Mach 2', 'Insp. Mach 3', 'Insp. Mach 4'
          1 line of performance inspection machines capable of testing either PN1xxxx or PN2xxxx.
                'Test Stand 1', 'Test Stand 2'
        
        To create the hole features:
          PN1xxxx undergoes 3 machining operations (OP1, OP2 and OP3), 1 dimensional inspection operation, and 1 performance inspection operation.
          PN2xxxx undergoes 2 machining operations (OP1 and OP3), 1 dimensional inspection operation, and 1 performance inspection operation.
        
        Data Recorded for Every Part:
          OP1, OP2, and OP3 machines export the following data on a drive with folders dedicated to the individual machine (EX. 'MachineA' folder on X drive):
            Part Number
            Operation Number (OP1, OP2, or OP3)
            Machine Name
            Cycle Time
          
          Inspection machines inspect 5 dimensions on every hole of every row and write .csv inspection files to part specific folders 
          on a dedicated inspection drive (EX. 'PN10008_raw_inspection_data.csv' in 'PN10008' folder on W drive):
            Part Number
            Inspection Machine, Inspection Date	
            Row	
            Hole	
            Feature X Position Measured,	Feature Y Position Measured,	Feature Position Measured,	Feature Position Out of Tolerance
            Angle 1 Measured,	Angle 1 Out of Tolerance,	Angle 1 Lower Bound,	Angle 1 Upper Bound
            Angle 2 Measured,	Angle 2 Out of Tolerance,	Angle 2 Lower Bound,	Angle 2 Upper Bound
            Angle 3 Measured,	Angle 3 Out of Tolerance,	Angle 3 Lower Bound,	Angle 3 Upper Bound
            Area 1 Measured,	Area 2 Measured,	Area Ratio Measured,	Area Ratio Out of Tolerance,	Area Ratio Lower Bound,	Area Ratio Upper Bound	
            Diameter Measured,	Diameter Out of Tolerance,	Diameter Lower Bound,	Diameter Upper Bound
        
          Test Stands inspect the performance of each row on the part individually. These machines export part performance data to a 
          database on a dedicated dirve (EX. PN10001's performance results are stored in 'Performance_data.csv on Z drive):
            Part Number
            Row
            Avg. Dia of the row (from dim. inspection)
            Comparison of avg. row dia. to nominal spec
            Performance test results
            Test stand number
            Test Datetime
    
