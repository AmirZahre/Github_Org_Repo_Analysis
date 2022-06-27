<!-- PROJECT SHIELDS -->
[![GPL License][license-shield]][license-url]
[![Forks][forks-shield]][forks-url]
[![Issues][issues-shield]][issues-url]
[![Code Size][cSize-shield]][cSize-url]


<!-- PROJECT LOGO -->
<br />
  <h3 align="center">An Analysis of Github Incident Tickets Submitted to the Astronomer Repository</h3>

  <p align="center">
    This was created in hopes to motivate others to be curious about analyzing their company repository.
    <br />
    <a href="https://github.com/AmirZahre/Github_Org_Repo_Analysis/"><strong>Check out the code »</strong></a>
    <br />
    <br />
    <a href="https://github.com/AmirZahre/Github_Org_Repo_Analysis/releases/tag/Astronomer">Download</a>
    ·
    <a href="https://github.com/AmirZahre/Github_Org_Repo_Analysis/issues">Report Bug</a>
    ·
    <a href="https://github.com/AmirZahre/Github_Org_Repo_Analysis/issues">Request Feature</a>
  </p>
</p>


<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
  * [Built With](#built-with)
* [License](#license)
* [Contact](#contact)
* [Acknowledgements](#acknowledgements)


<!-- ABOUT THE PROJECT -->
## About The Project
  
[![diagram]](#)  

A company's Github repository contains a plethora of insights. If leveraged, it can be a powerful tool towards understanding the needs of clients and prospects. This project is a brief dive into the ways one can analyze their company's repository. Enjoy.

  
### Built With
* [Python](https://www.python.org/)


### Built For
* [Astronomer](https://www.astronomer.io/) as my interview assignment.
  
### Important Files (i.e. my code)
* [Python Code](https://github.com/AmirZahre/Github_Org_Repo_Analysis/blob/main/astronomer_V1.ipynb)



<!-- PROCESS -->
## Use Case
  
Imagine a Data Analyst who works for an investment management firm, helping clients make good decisions about their investment portfolios. To do so, the Data Analyst retrieves market data regularly, and for each client provides an analysis of how the industries they are invested in perform.

The Data Analyst persists the transformed data from analyses, sends automated notifications to clients to take action when relevant, and keeps a dashboard up to date for them to check their investment health at a glance.

Let’s look into this Data Analysts workflow.
  
  
  
 
  
  
## Process
### Part 1: Airflow Triggers Databricks Notebook While Passing Parameters.
  <b>Step 1:</b>
  Pass parameters from Airflow using <b>notebook_params = portfolio</b>
  
```python
  portfolio = {
               "stocks": "MSFT AAPL IBM WMT SHOP GOOGL TSLA GME AMZN COST COKE CBRE NVDA AMD PG"
               }

   # Run the Databricks job and retrieve the job Run ID
   run_databricks_job = DatabricksRunNowOperator(
       task_id="Run_Databricks_Job",
       databricks_conn_id=DATABRICKS_CONNECTION_ID,
       job_id=137122987688189,
       do_xcom_push=True,
       notebook_params = portfolio
   )
```
<br>
  
  <b>Step 2:</b>
  Use dbutils.widgets.text(param, default_value) to load params pushed by Airflow into the Databricks notebook.
  [![param_get]](#)  

  
### Part 2: Data ingestion & Transformations
  <b>Step 1:</b>
  Invoking the API, we pull today's market cap data from Yahoo Finance using the yfinance Python package.
  [![invoke_api]](#) 
  
  <b>Step 2:</b> Aggregate today's market cap data by Industry Sector<br>
  [![aggregate_mkt_cap]](#) 

  
### Part 3: Enjoying the View, A (Delta) Table on a (Delta) Lake
  <b>Step 1:</b>
  Transform the pandas dataframe into a Spark dataframe. Write that dataframe into a temporary Delta Table.
  [![to_spark]](#) 
  
  <b>Step 2:</b>
  Upsert the temp Delta Table (containing today's data) into the permanent Delta Table containing all previous historic data.
  [![upsert]](#) 
  
  <br><b>Going forward, you can now link this table to Tableau for analysis.</b>
  
### Part 4: Monitoring portfolio performance & Email Notifications
  <b>Step 1:</b>
  Determining the Percentage Change from Day Prior
  [![percent_change]](#) 
  
  <b>Step 2:</b>
  Exit the Databricks Notebook with output data, which is subsequently captured by Airflow and passed around via. XCOM.
  [![xcom]](#) 
  
  <b>Step 3:</b>
  Ingesting results in Airflow: This data is picked up using the DatabricksHook and assigned to the variable model_uri.
  ```python
  @task
   def Retreive_Databricks_Output(id):

       # retrieve xcom data using DatabricksHook
       databricks_hook = DatabricksHook()
       model_uri = databricks_hook.get_run_output(id)['notebook_output']['result']

       return model_uri

   # Variable "Output" contains the xcom data from Databricks
   retreive_databricks_output = Retreive_Databricks_Output(run_databricks_job.output['run_id'])
  ```
  
  <b>Step 4:</b>
  Using the BranchPythonOperator to decide whether to notify
  ```python
     # Decide as to whether or not an email should be sent based on the content of Output
   branching = BranchPythonOperator(
       task_id='Check_if_Email_is_Needed',
       op_args = [retreive_databricks_output],
       python_callable=_split,
   )

   def _split(data):
       if data == "No Email Required":
           print("LOG: No big movers, no email was sent")
           return 'No_Email_Required'
       else:
           return 'Send_Email'
  ```
  
  <b>Step 5:</b>
  Send email notification.
  ```python
     # Send email containing the content of the xcom
   mail = EmailOperator(
       task_id='Send_Email',
       to='your_email@gmail.com',
       subject='Daily Movers',
       html_content=retreive_databricks_output,
       )
  ```
  
  
<!-- LICENSE -->
## License

Distributed under Apache License 2.0. See `LICENSE` for more information.



<!-- CONTACT -->
## Contact

Amir Zahreddine - zahreddi@ualberta.ca

Project Link: [https://github.com/AmirZahre/Data_Analyst_DAG](https://github.com/AmirZahre/Data_Analyst_DAG)



<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
* [Santona Tuli](https://www.linkedin.com/in/santona-tuli/) for being an awesome mentor towards my introduction to DevOps!
* [The team @ Astronomer](https://www.astronomer.io/) for help with any questions that arose while learning Airflow.
* [Ran Aroussi](https://pypi.org/user/ranaroussi/) for creating a fabulous Yahoo Finance API library for Python.

  
<!-- MARKDOWN LINKS & IMAGES -->
[license-shield]: https://img.shields.io/github/license/AmirZahre/Data_Analyst_DAG
[license-url]: https://github.com/AmirZahre/Data_Analyst_DAG/blob/main/LICENSE.md
[issues-shield]: https://img.shields.io/github/issues/AmirZahre/Data_Analyst_DAG
[issues-url]: https://github.com/AmirZahre/Data_Analyst_DAG/issues
[forks-shield]: https://img.shields.io/github/forks/AmirZahre/Data_Analyst_DAG
[forks-url]: https://github.com/AmirZahre/Data_Analyst_Dag/network/members
[cSize-shield]: https://img.shields.io/github/languages/code-size/AmirZahre/Data_Analyst_Dag
[cSize-url]: https://github.com/AmirZahre/Data_Analyst_DAG
[diagram]: images/workflow.png
[tasks]: images/task_dependencies.png
[param_get]: images/param_get.png
[param_check]: images/param_check.png
[invoke_api]: images/invoke_api.png
[aggregate_mkt_cap]: images/aggregate_mkt_cap.png
[to_spark]: images/to_spark.png
[upsert]: images/upsert.png
[percent_change]: images/percent_change.png
[xcom]: images/xcom.png
  
  

