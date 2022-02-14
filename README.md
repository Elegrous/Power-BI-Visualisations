# Power BI "Exercise Analysis"
&#x1F4D7;
Power BI Visualisation from XML file

### DataSourse: Apple Health App
- Open the Health app on iPhone.
- Tap on your profile icon in the top right corner.
- Scroll down to the bottom of the Health profile and tap on “Export Health Data”
- Tap on “Export” to confirm that you want to export Health data and start the exporting process, it may take a while to complete.
- It saves the files in .xml format inside a zip file.

### Import XML File to Excel (only Windows)
- Open the Excel file where you want to get the data from the XML file.
- Click the Data tab.
- In the 'Get & Transform' data group, click on the 'Get Data' option.
- Go to the 'From file' option.
- Click on 'From XML'

### Data Preparation
1. From export.xml choose "Workout" data.
2. Create a Table "Calendar": <br />
```diff
Calendar = CALENDAR(MIN('Workout'[Attribute:creationDate]),MAX('Workout'[Attribute:creationDate]))
```
3. Create a Table "ActivityTypes":
```diff
ActivityTypes = DISTINCT('Workout'[Attribute:workoutActivityType])
```
Add New column to the Table:
```diff
ActivityType = RIGHT('ActivityTypes'[Attribute:workoutActivityType],LEN('ActivityTypes'[Attribute:workoutActivityType])-LEN("HKWorkoutActivityType"))
```
4. Add relationships between tables.
