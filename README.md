# Perplexity-Email-Job-Tracker
Description of a Perplexity automation that scans and organize offers from job board emails.

## Requirements

* User has a Perplexity Pro account (untested without account)
* User has a Google account (process-dedicated is recommended).
* User has directed job boards alerts to the Gmail address
  associated to the Google account.

## Goals

* Save time, scanning incoming job alert emails automatically.
* Get a daily first sort and analysis of all incoming job offers.
* Get overall statistics of how well the job alerts perform to improve
  the job board list, filters, etc.

## Initialization

1. In Google Drive, create a folder `JobSearch` with content such as:
    ```
    JobSearch
    |_ JOBNAME
      |_ CV1.md, CV2.md, Job Tracker - JOBNAME.
    ```
    where CV1.md and CV2.md are files describing your job search and 
    Job Tracker - JOBNAME is the Google spreadsheet that will be used
    in the automation. The template is [here](https://docs.google.com/spreadsheets/d/1VLBjfaw47AQ5sM7i7M2YKbkXVaLLNezClutV5u4km-E/copy).
1. Customize the [automation_description.md](automation_description.md)
   file with your paths and Google IDs.
1. Create a Perplexity automation. NB - We used GLM 5.3 because,
   at the time of our implementation, it allowed a free
   automation creation. Use:
    1. [automation_description.md](automation_description.md).
    2. Google Email & Calendar connector.
    3. Google Drive connector.
1. Once automation is created, Perplexity will offer to attach it to
   a project. Accept as it will record daily execution results.

## Usage

Every day, the automation will update the spreadsheet with new offers 
found in incoming job alerts.

A set of pivot tables in the spreadsheet provides insights about how the 
process is going : last incoming offers, number of offers by salary ranges,
number of offers and match levels per job board, etc. Of course you can customize
theses views to fit your process and requirements.

## Warnings

* Watch closely how your automation consumes tokens. We observe an average
  consumption of 2 to 300 tokens per day.
* Results provided in the spreadsheet are provided by the AI model you have
  chosen. AI can make mistakes. Closely review matching levels, reasons for
  applying and not applying. Adapt the automation accordingly if needed.


