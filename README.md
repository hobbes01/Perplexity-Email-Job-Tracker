# Perplexity-Email-Job-Tracker
Description of a Perplexity automation that scans and organizes offers from job board emails.

## Requirements

* A Perplexity Pro account (untested without Pro account)
* A Google account (process-dedicated is recommended to isolate the automation on a restricted set of messages).
* Job boards alerts sent to the Gmail address associated with the Google account.

## Goals of the Job Tracker automation

* Spend less time scanning emails, offers... and more time communicating with hiring teams.
* Apply to the best matching offers, faster.
* Improve the performance of job alerts based on collected statistics.

## Features

* Daily scanning of incoming job board emails.
* Sorting of embedded offers according to your CV, profile, project and a 5-level matching scale.
* Attribute detection (if advertised): company, city, job title, job description, min-max salary, offer URL.
* Proposition of reasons for applying and reasons for not applying.

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

* Watch closely how your automation consumes tokens. We observed an average
  consumption of 2 to 300 tokens per day.
* Results provided in the spreadsheet are provided by the AI model you have
  chosen. AI can make mistakes. Closely review results such as matching levels,
  reasons for applying and not applying. Adapt the automation if needed.


