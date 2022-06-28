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

##### Important Note: Part 1 and Part 2 of the code were predominantly written by [Santona Tuli](https://www.linkedin.com/in/santona-tuli/), albeit for a few changes I added here and there.

#### Points of Interest:
1. <b>Cell 2</b> introduces a method of locally caching the .json responses derived from the repository. This allows us to run the code without needing to scrape the data each time, as previously pulled data is saved to <b>airflow_issues.feather</b>.
2. <b>Cell 12</b> derives some valuable insights pertaining to the volume of tickets submitted over the last ~7 years. This is touched on below.
3. <b>Cell 14</b> utilizes a Binary Search Tree (BST) to quickly and efficiently organize User ID's in order to create a list of unique repository visitors. Starting from 2015 and moving towards the present, each User ID will be compared to the User ID's in the BST. If the user ID exists, that user is <b>not</b> a unique submitter; however, if it does not exist, it's the user's first time submitting a ticket -- and therefore, they're unique.
4. <b>Cell 17</b>, in a similar trend to the volume of tickets submitted, shows some interesting changes in the number of unique accounts submitting tickets within the same time frame.
  
### Built With
* [Python](https://www.python.org/)


### Built For
 * [Astronomer](https://www.astronomer.io/) as my interview assignment.
  
### Important Files (i.e. my code)
 * [Python Code](https://github.com/AmirZahre/Github_Org_Repo_Analysis/blob/main/jupyter_notebook.ipynb)
 
 
## Notable Insights
### 
[![volume]](#)
 * On a month to month basis, the volume of tickets hovered at around the same levels for the years 2015 to 2018.
 * Beginning 2019, ticket volume begins to increase above historic levels.
 * 2020 is where things begin to get interesting, as ticket volume begins to substantially increase.
 * Overall, there is a substantial increase in volume for the entire periods of 2020 and 2021 vs. the years prior.
##### How does the volume of tickets match with the number of unique visitors? Is it the same people just asking more tickets, or can this partially be attributed to a greater number of unique individuals experiencing issues?
 
[![unique]](#)

* The increase in volume of tickets follows the trend of the increase of unique submitters.
* Number of unique users held steady each year for the 2015 to 2019 period.
* March of 2020, <b>something</b> happened to the community - the level of unique users submitting tickets rose sharply, from ~55 to    ~130 users for the month of March.
* Since that period, the levels of unique users has continued to be maintained.
* Assumptions as to why the levels rose drastically can be due to COVID-19, as the pandemic also began March 2020.
##### The reasoning behind this spike required further digging. I'd like to look into the number of enterprises utilizing Apache Airflow, as an influx of large corporations could also influence the large spike in activity from community members.

[![volume_complete]](#)
We're able to see an interesting (and positive) trend here: although the volume of tickets held at roughly the same level for 2021, the time it took from ticket creation to completion was on the decline! This is good news -- it may imply that community engagement is increasing (quicker response times from members) and documentation/knowledge quality is improving.
  
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
[volume]: images/volume.png
[unique]: images/unique.png
[volume_complete]: images/volume_complete.png

  
  
