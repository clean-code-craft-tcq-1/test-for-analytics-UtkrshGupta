# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked 'enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. csv file on the server with data
2. Read Access to the Server containing the telemetrics in a csv file
3. Read and Write access authorizations for PDF generation and saving on server 
4. Python Libraries such as numpy, Pandas, matplotlib, third party PDF generation libraries, smtp (if notification is sending via email)
5. Sending Authorization and access to SMTP server ( Assuming Notification is being sent via Email )
6. All pre-defined constants/thresholds

(add more if needed)

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No            | Since this item is Off-the-shelf, it is expected that it has already passed all its unit tests 
Counting the breaches       | Yes           | This is part of the software being developed
Detecting trends            | Yes           | This is part of the software being developed
Notification utility        | No            | This is External interface, should be tested using mocks

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write/Return minimum and maximum to the PDF from a csv containing positive and negative readings
2. Write/Return "Invalid input" to the PDF when the csv doesn't contain expected data
3. Write/Return "No Data Found" to the PDF when the csv doen't have any data
4. Write/Return "Standard server Return Code" when the server is not reachable such as 500, 501
5. Write/Return "File Not Found" when csv file if not found on server
6. Write/Return "Invalid Breach Count" when count of breaches are incorrectly calculated
7. Write/Return Count of Breaches to the PDF when the CSV have some reading which are crossing threshold.
8. Write/Return 0 as Breach Count when there are no breach seen
9. Write/Return TimeStamp Details when the CSV readings are increasing continuously for 30 min.
10. Write/Return "Not Available" in trends when there is no continuous reading increase available in CSV File.
11. Write/Return "Successfully Saved" when the generated file is stored/saved in specified location of server every week.
12. Write/Return "Sender/Recepient Invalid" when sender/recepient email address are invalid or not found. 
13. Write/Return "Standard Email success/error Return code" whenever an email is sent or not for the new/updated available report such as 420, 421, 422

(add more)

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | Report file path | invoke notification utility if file exits else takes error measures               | Fake Notification Ulitity
Report inaccessible server | Server Path  | Standard 5xx server error Return codes eg. 500, 501                    | Fake the HTTP client
Find minimum and maximum   | csv data     | Minimum/Maximum             | None - it's a pure function
Detect trend               | csv data     | Trend TimeStamp Details     | None - it's a pure function
Write to PDF               | Analysis Readings which needs to record in file     | PDF File                    | Fake the PDF Conversion
