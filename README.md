# Power BI "Exercise Analysis" with data from Apple Health App
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
Add New columns to "Calendar":
```diff
Weekday = FORMAT('Calendar'[Date],"ddd")

Weekday Number = WEEKDAY('Calendar'[Date])

Week Number = WEEKNUM('Calendar'[Date])
```

3. Create a Table "ActivityTypes":
```diff
ActivityTypes = DISTINCT('Workout'[Attribute:workoutActivityType])
```
Add New column to "ActivityTypes":
```diff
ActivityType = RIGHT('ActivityTypes'[Attribute:workoutActivityType],LEN('ActivityTypes'[Attribute:workoutActivityType])-LEN("HKWorkoutActivityType"))
```
4. Add relationships between tables. 

![image](https://user-images.githubusercontent.com/44158648/153913961-601c2745-021a-489c-b0d2-35b1c7a022ea.png)

### Create Measures
<img src="https://user-images.githubusercontent.com/44158648/153914674-73b93321-0312-4f44-82c9-12a725c037f9.png" width="300" >

```diff
Average Duration, min = AVERAGEX(Workout,[Total Activity Duration, min])

Average Energy Burned, kcal = AVERAGEX('Workout',[Total Energy Burned, kcal])

Best Daily Activity = MAX('Workout'[Attribute:totalEnergyBurned])

Best Day in Burned kcal = 
VAR Best = MAX('Workout'[Attribute:totalEnergyBurned])
Return CALCULATE(MAX('Workout'[Attribute:creationDate]),'Workout'[Attribute:totalEnergyBurned] = Best)

Distance Walking, km = CALCULATE([Total Distance, km], 'ActivityTypes'[ActivityType]="Walking")

Total Activity Duration, min = SUM(Workout[Attribute:duration])

Total Distance, km = sum('Workout'[Attribute:totalDistance])

Total Distance, km (Cumulative) = CALCULATE([Total Distance, km],FILTER(ALLSELECTED('Calendar'),'Calendar'[Date] <= MAX(Workout[Attribute:creationDate])))

Total Energy Burned, kcal = SUM(Workout[Attribute:totalEnergyBurned])

Walking Distance % of Total = divide(calculate([Total Distance, km],'ActivityTypes'[ActivityType]="Walking"),[Total Distance, km])

Weekly Burned kcal Changes in % = 
VAR CurrentWeek = CALCULATE([Total Energy Burned, kcal],FILTER(ALL('Calendar'[Date]),WEEKNUM('Calendar'[Date]) = SELECTEDVALUE('Calendar'[Week Number])))
VAR PreviousWeek = CALCULATE([Total Energy Burned, kcal], FILTER(ALL('Calendar'[Date]),WEEKNUM('Calendar'[Date]) = SELECTEDVALUE('Calendar'[Week Number]) - 1))
RETURN
DIVIDE((CurrentWeek - PreviousWeek), PreviousWeek)
```

### Data Visualization
![image](https://user-images.githubusercontent.com/44158648/153914148-586313c3-5ce7-41bf-bfd2-05617e44e919.png)

File "Exercise_Analysis.pbix" is attached
