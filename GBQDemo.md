# Google BigQuery Demo
Produced by Dave Lusty
# Introduction
In this demo we’ll configure a Google BigQuery instance, add some data, and then query that data from Azure Data Factory using a copy job. The copy job will use a schedule and only copy data matching certain parameters.
# Configure Google
## Project Setup
Log into Google Cloud at [https://console.cloud.google.com](https://console.cloud.google.com) and create a new project by clicking the project name on the menu and selecting “new project”.
![1.1 select project dialog.png](images/1.1selectprojectdialog.png)
Name the project “GBQDemo”, a project ID will be created for you. Copy this project ID to your notes for later as we’ll need it to connect from Azure Data Factory.
![1.2newproject](1.2newproject.png)
If you didn’t copy the project ID you can do so later by clicking the ellipsis at the top right of the Google portal and selecting Project Settings.
![1.3ellipsis](1.3ellipsis.png)
This will allow you to change the project name and copy the ID and project number if you need to.
![1.4projectsettings](1.4projectsettings.png)
## Set up Google BigQuery Tables
Browse to BigQuery in the menu. From here, select your project and click Create Dataset.
![2.1createdataset](2.1createdataset.png)
Create a dataset with a suitable name, here we’ll use GBQDemo
![2.2newdataset](2.2newdataset.png)
Next we’ll add tables and fill them with data. For this demo create two columns, one with a datetime stamp and the other a string. This allows us to query based on date for the tumbling window in Azure Data Factory.
![2.3addtable](2.3addtable.png)
Alternatively use the following query from the query editor:
'''SQL
CREATE TABLE gbqdemo.testtable (
   name STRING,
   date DATETIME
)
'''
To create some data, I used the following query:
'''SQL
INSERT gbqdemo.testtable (date, name)
VALUES (datetime(2019, 01, 13, 07, 22, 00), "January 13th 2019"),
(datetime(2018, 12, 21, 05, 45, 00), "December 21st 2018"),
(datetime(2018, 12, 20, 10, 35, 00), "December 20th 2018"),
(datetime(2018, 12, 19, 01, 31, 00), "December 19th 2018"),
(datetime(2018, 12, 18, 02, 32, 00), "December 18th 2018"),
(datetime(2018, 11, 05, 05, 37, 00), "November 5th 2018"),
(datetime(2018, 11, 01, 06, 34, 00), "November 1st 2018")
'''
## Create a Query
Use the query editor in BigQuery to create a query for later and test that it returned suitable rows. For this use the following query:
'''SQL
SELECT * FROM gbqdemo.testtable WHERE date >= DATETIME "2017-12-01 00:00:00" AND date <= DATETIME "2019-01-01 00:00:00"
'''
While not complex, this gives enough information to demo with and will select rows with a timestamp between two values.
