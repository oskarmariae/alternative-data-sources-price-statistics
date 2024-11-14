# Price Statistics Production Systems Survey

**Overview**: This document contains an initial set of candidate survey questions to present with the working group on Price Statistics Production Systems.

**Authors**:

- Collin Brown (@collinbrown95)
- Steve Martin (@marberts)

## Preamble and Definitions

This survey is concerned with the interaction between teams, software systems, and key processing steps of the Generic Statistical Business Process Model (GSBPM) used in production systems to make price indexes, namely the Consumer Price Index (CPI).

For the purposes of this survey, we define a **system** as any **indivisible (atomic) software component** that takes one or more data inputs and produces data outputs, and a **team** as a group of individuals who maintains one or more systems.

**Note**: when we refer to an indivisible software component, we mean that the component runs "entirely or not at all" with respect to transforming input(s) into output(s). If there is one Python script that reads a file and writes an intermediate file, and a second R script that reads the intermediate file and produces another output file, we would consider this to be two systems.

![system-definition-diagram](./diagrams/gsbpm-systems.drawio.svg)

To remain as generic as possible, we abstract the key sub-processess for the processing phase (phase 5) of the GSBPM as follows.

| Process | Explanation |
| ---------- | ----------- |
| **Data Ingestion** | Activities to bring acquired data into a machine-readable state where it is ready for further processing. |
| **Data Processing** | Activities to clean, validate, correct, impute, or otherwise adjust the data so that it is in a state where it is ready to be used to produce elementary indexes. |
| **Elementary Indexes** | Calculate indexes from the processed data for a given geography, time period, and product category (elementary aggregate). |
| **Aggregation** | Aggregate the elementary price indexes into higher-level indexes (e.g., all-items CPI). |
| **Finalization** | Store price indexes for subsequent use as part of analytical activities, and eventual dissemination. |

The flow of work (raw data to final outputs) in the diagram below can be read from left to right. The green ovals/circles represent systems as defined earlier in this document, and the team that owns one or more systems is represented as a box drawn around a system.

![flow-of-work-diagram-overview](./diagrams/gsbpm-systems-2.drawio.svg)

A system can handle a single step (shown in "Aggregation" and "Finalization"), multiple steps (shown in "Data Processing" and "Elementary Indexes"), or multiple systems can be involved in handling a single step (shown in "Data Ingestion").

These systems can be "piped" together, implying that the output of one system becomes the input to the next system. To simplify the notation, the intermediate input/output boxes are simply shown as an arrow from one system node to the next system node.

In the event that multiple teams collectively maintain a very large system, a team can be interpretted as the larger organizational unit that oversees the various sub-teams involved.

### Example

The figure below shows an example of how the diagrams presented above could look for a real National Statistics Organization (NSO).

![example-real-system](./diagrams/gsbpm-systems-3.drawio.svg)

This figure can be interpretted as follows.
- A corporate IT team maintains a large Java application with an application-specific database that handles ingestion of most data sources.
    - The corporate IT team boxes are drawn in blue, indicating that they are a **central organization team** that does not necessarily have detailed **domain knowledge**.
    - In the absence of domain knowledge, this team would need to engage in a requirements gathering process with the domain team in order to build a system that performs the required functionalities.

- A team of Economists and Statisticians maintains a system written in Python and [pandas](https://pandas.pydata.org/) to ingest data from one source that does not integrate with the corporate system.
    - The Economists and Statisticians team boxes are drawn in yellow, indicating that they are a **domain aligned team**.

- One or more teams of Economists and Statisticians maintain a series of systems, using some combination of R and Python, that perform data processing and computation of elementary indexes.
    - The top team has a single system that handles both data processing and index calculation. It is not straightforward to make significant changes to one of these steps without impacting the other.
    - The remaining teams have two systems to respectively handle data processing and calculating elementary indexes.
- A second corporate IT system aggregates the elementary indexes and stores the resulting price indexes in a database.

## A Note to the Respondent

This survey is designed assuming that we are addressing an individual from the **price statistics unit** of the organization.

If you are **not** familiar with the implementation details of a system (e.g., a corporate IT system that you do not directly develop or maintain), please answer the questions based on **your user-facing experience** with the system.

For example, if you interact with a single corporate IT system for the Data Ingestion and Data Processing steps of the GSBPM process, from your perspective, this is **one system**, even if "under the hood" it is composed of many smaller systems.

# Survey

## Section 1: Up-Front Administrative Questions

### 1.1: What NSO/Organization do you represent?

- Free text field

### 1.2: What price statistics output do you produce?

- [ ] Consumer Price Index (CPI)
- [ ] Producer Price Index (PPI)
- [ ] Something else

## Section 2: Archetype Survey Questions

For each question, we provide the option to answer separately for each GSBPM step, given that the system and team characteristics may be different for each.

### Question 1: Monolithic or Modular

In this question, we are trying to understand how centralized/decentralized system implementations are **within** a GSBPM step and **between** GSBPM steps. The two extremes are (1) a completely **modular** system where there is a separate system for every GSBPM step and every output, and (2) a completely **monolithic** system where a single system handles every GSBPM step for every output.

![modular-gsbpm-illustration](./diagrams/gsbpm-modular.drawio.svg)

![monolith-gsbpm-illustration](./diagrams/gsbpm-monolith.drawio.svg)

### 1.1 Approximately how many **distinct** systems exist for each GSBPM step?

**Note 1**: We'll present this as a range to reduce response burden.

- [ ] 1 system (special case)
- [ ] 2-5 systems
- [ ] 5-10 systems
- [ ] 10-20 systems
- [ ] 20-50 systems
- [ ] 50-100 systems
- [ ] more than 100 systems

**Note 2**: for the purposes of this question, the **size/complexity** of the system is **irrelevant**. If the system consumes and produces **any** material input/output involved in the production of the price index, it should be counted as a **distinct** system.

| Data ingestion | Data processing | Elementary indexes | Aggregation | Finalization |
| ---            | ---             | ---                | ---         | ---          |
| Select #       | Select #        | Select #           | Select #    |  Select #    |

#### 1.2: For **any** of your systems, is it the case that the **same system** handles more than one GSBPM step?

**Note**: for the purpose of this question, two or more GSBPM steps are handled by the same system if it would not be straightforward to decouple the capabilities for the various steps into two or more independent systems.

- [ ] yes
- [ ] no

#### 1.2.1 `(if 1.2 == 'yes')` In the event that systems handle more than one GSBPM step, please indicate which GSBPM steps are handled by the same system.

**Instructions**: For each distinct system that handles more than one GSBPM step for some set of inputs and outputs, indicate all steps that the system handles in the table below.

**Example**: If I had (1) one system that handled "Data Ingestion" and "Data Processing", (2) a second system that handled "Produce Elementary Aggregates" and "Aggregation", (3) a third system that handled "Produce Elementary Aggregates", "Aggregation", and "Dissemination", and (4) a fourth system that handled "Data Processing", "Produce Elementary Aggregates", and "Aggregation", I would answer this question as follows.

| System Name | Data Ingestion | Data Processing | Produce Elementary Aggregates | Aggregation | Dissemination |
| ----------- | -------------- | --------------- | ----------------------------- | ----------- | ------------- |
| System 1    | X | X |   |   |   |
| System 2    |   |   | X | X |   |
| System 3    |   |   | X | X | X |
| System 4    |   | X | X | X |   |
| System 5    |   |   |   |   |   |
| (Add more)  |   |   |   |   |   |


| System Name | Data Ingestion | Data Processing | Produce Elementary Aggregates | Aggregation | Dissemination |
| ----------- | -------------- | --------------- | ----------------------------- | ----------- | ------------- |
| System 1    |   |   |   |   |   |
| System 2    |   |   |   |   |   |
| System 3    |   |   |   |   |   |
| System 4    |   |   |   |   |   |
| System 5    |   |   |   |   |   |
| (Add more)  |   |   |   |   |   |


#### 1.2.2 `(if 1.2 == 'yes')` As measured by total expenditure weight, approximately what share of your outputs are handled by **all** of the systems in 1.2.1?

- [ ] Less than 10%
- [ ] Between 10% and 30%
- [ ] Between 30% and 50%
- [ ] Between 50% and 70%
- [ ] Between 70% and 90%
- [ ] Between 90% and 100%

#### 1.3 Fraction of Cases Handled

In this question, we are trying to understand if there are any systems that handle the **majority** of cases for a particular GSBPM step.

For the purposes of this question, a **single system** handles a majority of cases if it is the **primary (or only) system** for **70% or more** of cases for a particular GSBPM step.

In the table below, for each GSBPM step, please indicate if there is **one** system that handles the **majority** of cases for that step.

| Data ingestion | Data processing | Elementary indexes | Aggregation | Finalization |
| ---            | ---             | ---                | ---         | ---          |
| Select <Yes/No>   | Select <Yes/No> |  Select <Yes/No>   | Select <Yes/No> |  Select <Yes/No>   |


### Question 2: Corporate IT or DIY

In this question, we are trying to understand which systems are maintained by which teams in your organization.

#### 2.1 For each GSBPM step, how many systems are maintained by each of the following teams?

**Instructions**: For the purposes of this question, we define each team as follows.

- **Corporate IT**: Information Technology (IT) professionals who are part of an organization-wide central group.
- **Domain Embedded IT**: IT professionals who are functionally embedded in the price statistics team.
- **Domain Embedded Analysts**: Professionals without a formal IT background (e.g., Economists, Statisticians, Mathematicians, or similar) who are functionally embedded in the price statistics team.
- **Analysts Elsewhere in the Organization**: Professionals without a formal IT background who are **not** functionally embedded in the price statistics team.
- **External Consultants/Contractors**: Professionals outside of the organization to whom system development/maintenance work is contracted.

Furthermore, if a system handles **multiple GSBPM steps**, please include **all GSBPM steps** where the system is used.


|     | Ingestion | Processing | Elementals | Aggregation | Finalization |
| --- | ---       | ---        | ---        | ---         | ---          |
| Corporate IT       | Select # | Select # | Select # | Select # | Select # |
| Embedded IT        | Select # | Select # | Select # | Select # | Select # |
| Embedded analysts  | Select # | Select # | Select # | Select # | Select # |
| Elsewhere analysts | Select # | Select # | Select # | Select # | Select # |
| Contractors        | Select # | Select # | Select # | Select # | Select # |


### Question 3: Analysis of a Group of Systems

In this question, we aim to understand how team boundaries are related to system implementation.

To achieve this goal, we introduce the concept of a **system group**, which we use to understand how a group of related systems are organized within our conceptual model. A concrete example follows to help in this exercise.

#### 3.0 Selection of a "representative system"

In writing this survey, we assume that within a National Statistics Organization there will be "groups" of related systems that share common characteristics. In this question, we would like you (the respondent) to attempt to characterize one of these groups.

#### 3.1 System boundary crossing for your representative system group

With your representative system group in mind, please indicate in the diagram below which systems handle which steps.

|     | Ingestion | Processing | Elementals | Aggregation | Finalization |
| --- | ---       | ---        | ---        | ---         | ---          |
| System Group 1 | Select # | Select # | Select # | Select # | Select # |
| System Group 2 | Select # | Select # | Select # | Select # | Select # |
| System Group 3 | Select # | Select # | Select # | Select # | Select # |
| System Group 4 | Select # | Select # | Select # | Select # | Select # |
| System Group 5 | Select # | Select # | Select # | Select # | Select # |
  

#### 3.2 Team maintenance for your representative system group

Thinking about the system group you defined in 3.1, please indicate which team **maintains each system group** as labelled in question 3.1.

**Note**: when we use the word **maintains**, we are referring to the set of activities required to make changes over time to each system in your system group.

**Example**: if an analyst in the domain team is responsible for using an Excel Workbook to calculate elementary aggregates, and needs to change how a calculation is performed, this analyst is the **maintainer** of that **system**. Therefore, if the system described in this example is "System 3", the answer to this question would be to select "Yes" at the intersection of "Embedded Analysts" and "System 3".

**Example**: if an IT professional in the organization's corporate IT department needs to update Java source code in order to fix a bug, then this IT professional would be the **maintainer** of that **system**. Therefore, if the system described in this example is called "System 2", then the answer to this question would be to select "Yes" at the intersection between "Corporate IT" and "System 2".

In the event that **more than one team** maintains systems in the system group, please select the team that **performs the most significant share of maintenance activities**.

|     | Corporate IT | Embedded IT | Embedded Analysts | Elsewhere Analysts | Contractors |
| --- | ---       | ---        | ---        | ---         | ---          |
| System Group 1 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 2 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 3 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 4 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 5 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |

#### Concrete Example

Suppose that my price statistics system diagram looks as follows.

![gsbpm-systems-3-highlighted](./diagrams/gsbpm-systems-3-highlight.drawio.svg)

My task is to identify a representative **system group**. The systems enclosed by the dashed red line in the diagram cover all of the systems that participate in my representative group.

I would characterize this system group in the following way.

- There is a single system for the purpose of Data Ingestion that is maintained by my organization's corporate IT department.
- There are three systems that handle Data Processing and three systems that handle Elementary Index Calculation. Each **pair** of systems is maintained by one or more small teams of Economists and Statisticians.
- There is one large single system that handles Aggregation and Finalization of Elementary Index Calculations, this system is also maintained by the Corporate IT department.

The more general idea here is that in your own organization's systems diagram, there will likely be a **subset** of systems that **follow a similar pattern to eachother**. In the example above, that pattern is that one large system feeds several smaller systems that in turn feed back into a large system.

Given this example, I show below how I would answer Questions 3.1 and 3.2

**Question 3.1**

|     | Ingestion | Processing | Elementals | Aggregation | Finalization |
| --- | ---       | ---        | ---        | ---         | ---          |
| System Group 1 | **1** | Select # | Select # | Select # | Select # |
| System Group 2 | Select # | **2-5** | Select # | Select # | Select # |
| System Group 3 | Select # | Select # | **2-5** | Select # | Select # |
| System Group 4 | Select # | Select # | Select # | **1** | **1** |
| System Group 5 | Select # | Select # | Select # | Select # | Select # |

**Question 3.2**

|     | Corporate IT | Embedded IT | Embedded Analysts | Elsewhere Analysts | Contractors |
| --- | ---       | ---        | ---        | ---         | ---          |
| System Group 1 | **Yes** | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 2 | Select <Yes/No> | Select <Yes/No> | **Yes** | Select <Yes/No> | Select <Yes/No> |
| System Group 3 | Select <Yes/No> | Select <Yes/No> | **Yes** | Select <Yes/No> | Select <Yes/No> |
| System Group 4 | **Yes** | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| System Group 5 | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |


### Question 4: Open Source Data Science Tools

In this question, we are trying to understand which tools are used in the implementation of systems for each GSBPM step.

#### 4.1 For any of the GSBPM steps in this survey, do you use any open source libraries for making price indexes as part of your system?

Do you download, import, or otherwise use price index method library implementations such as [PriceIndices](https://github.com/JacekBialek/PriceIndices), [piar](https://github.com/marberts/piar), or a similar library.

- [ ] Yes
- [ ] No

#### 4.1.1 `(if 4.1 == 'no')` For what reason(s) do you not use any open source libraries for price index method calculations?

Please select all that apply.

- [ ] I did not know that libraries like this exist.
- [ ] There is insufficient documentation for how to use these libraries.
- [ ] The codebases for these libraries are not well maintained, and we do not want our production system(s) to depend on unmaintained code.
- [ ] These libraries are not suitable for our production systems (e.g., features are missing, not implemented in the way we require, etc.)
- [ ] It is not clear how the code in these libraries would integrate into my existing system(s).
- [ ] Our team doesn't have the skills to make use of these libraries.
- [ ] Some other reason (please specify)


#### 4.2 For each GSBPM step, are your system(s) made in-house or do you use commercial off the shelf (COTS) software?

**Note**: If more than one option is true, please select all that apply.

| GSBPM Step | In House | COTS Software | Bespoke Software built by contractors/consultants |
| --- | --- | --- | --- |
| Ingestion | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| Validation | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| Elementals | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| Aggregation | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |
| Finalization | Select <Yes/No> | Select <Yes/No> | Select <Yes/No> |


#### 4.3 For each GSBPM Step, which programming languages and statistical tools do you use in your system implementation?

Please select all that apply.

#### 4.3.1 How do you perform **Version Control** for your systems?

- [ ] Git
- [ ] GitHub, GitLab, or similar platform
- [ ] Apache Subversion
- [ ] Mercurial
- [ ] Built-in versioning capabilities of a commercial tool
- [ ] File naming conventions (e.g. `my_script_john_doe_version_1.py`, `my_script_john_doe_version_1_final.py`, etc.)
- [ ] Other (Please specify)
- [ ] None of these (we do not perform any version control over our systems)

#### 4.3.2 **Project Management Software**

- [ ] Jira
- [ ] GitHub projects
- [ ] GitLab milestones/issues
- [ ] Shared Excel spreadsheet
- [ ] Other project management software (please specify)
- [ ] None of these (we do not use any software for the purpose of project management)

#### 4.3.3 **Programming Languages**

- [ ] Python
- [ ] R
- [ ] Julia
- [ ] Scala
- [ ] Java
- [ ] C#
- [ ] C
- [ ] Visual Basic (VB)
- [ ] Visual Basic Applications (VBA)
- [ ] SAS Scripting Language
- [ ] Stata Scripting Language
- [ ] SPSS Scripting Language
- [ ] SQL (e.g., stored procedures in a data warehouse)
- [ ] Other (Please specify)
- [ ] None of these (We don't use any programming language for this GSBPM step)

#### 4.3.4 **Open Source Packages**

- [ ] Python Open Source Data Science Tools (e.g., pandas, numpy, etc.)
- [ ] R Open Source Data Science Tools (e.g., dplyr, ggplot2, etc.)
- [ ] Other (Please specify)
- [ ] None of these (We don't use any open source software for this GSBPM step)

#### 4.3.5 **Commercial Software**

- [ ] Commercial Data Warehouse product (e.g., Snowflake, Redshift, IBM Netezza, etc.)
- [ ] SAS
- [ ] Stata
- [ ] MATLAB
- [ ] SPSS
- [ ] Microsoft Excel
- [ ] Microsoft Power BI
- [ ] Microsoft Access
- [ ] Other (Please specify)
- [ ] None of these (We don't use any commercial software for this GSBPM step)

#### 4.3.6 **Cloud Provider/Infrastructure**

Does your organization use a public/private cloud provider of any kind (e.g., Microsoft Azure, AWS, etc.)?

- [ ] Yes
- [ ] No
- [ ] Unsure (I don't know what this question means)
- [ ] Unsure (I don't know whether or not my organization uses a public/private cloud provider)


| GSBPM Step | Version Control | Project Management | Programming Languages | Open Source Packages | Commercial Software | Cloud |
| --- | --- | --- | --- | --- | --- | --- |
| Ingestion | Select all that apply | Select all that apply | Select all that apply  | Select all that apply | Select all that apply | Select all that apply  |
| Validation | Select all that apply | Select all that apply | Select all that apply  | Select all that apply | Select all that apply | Select all that apply  |
| Elementals | Select all that apply | Select all that apply | Select all that apply  | Select all that apply | Select all that apply | Select all that apply  |
| Aggregation | Select all that apply | Select all that apply | Select all that apply  | Select all that apply | Select all that apply | Select all that apply  |
| Finalization | Select all that apply | Select all that apply | Select all that apply  | Select all that apply | Select all that apply | Select all that apply  |

### Question 5: New or Well-Established

In this question, we are trying to understand which systems in each GSBPM step are new and which ones have been in use for a long time.

#### 5.1 Age of System

For each GSBPM step in the table below, please indicate **how many of your systems** fall into each age bracket.

For the purposes of this question, the "age" of a system began when the system was first used to produce official statistics for your organization.

| GSBPM Step | < 1 year | 1-5 years | 5-10 years | 10-20 years | > 20 years |
| --- | --- | --- | --- | --- | --- |
| Ingestion | Select # | Select # | Select # | Select # | Select # |
| Validation | Select # | Select # | Select # | Select # | Select # |
| Elementals | Select # | Select # | Select # | Select # | Select # |
| Aggregation | Select # | Select # | Select # | Select # | Select # |
| Finalization | Select # | Select # | Select # | Select # | Select # |

#### 5.2 Frequency of Updates

In this question, we are trying to understand how frequently **non-trivial updates** are made to the system. To help clarify, we provide some examples of trivial updates and non-trivial updates below.

**Trivial Updates Examples**

- The Operating System on the machine running a system was updated.

- Corporate IT has asked everyone to updgrade from Python 3.10 to Python 3.11, which didn't require any updates to the system's codebase or dependencies.

- A new version of Microsoft Excel was made available through your organization's software catalog, and one of your systems requires an Excel Workbook to run.

**Non-Trivial Updates Examples**

- A new price index calculation was introduced to an Excel Workbook used in the system.

- A bug in the system was fixed.

- The system's data ingestion code was modified to enable the ingestion of a new upstream data source.

---------

For each GSBPM step in the table below, please indicate **how many of your systems** fall into each update frequency.

Definitions of each update frequency can be found below.

- **daily**: at least once per day

- **weekly**: at least once per week but less often than once per day

- **monthly**: at least once per month but less often than once per week

- **every 3 months**: at least once every 3 months but less often than once per month

- **every 6 months**: at least once every 6 months but less often than once every 3 months

- **once per year**: at least once per year but less often than once every 6 months

- **less than once per year**: the system receives updates less often than once per year (e.g., every 2 years)

| GSBPM Step | daily | weekly | monthly | every 3 months | every 6 months | once per year | less than once per year |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Ingestion | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Validation | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Elementals | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Aggregation | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Finalization | Select # | Select # | Select # | Select # | Select # | Select # | Select # |


### Question 6: Team Structures

In this question we are trying to understand the team organization and communication structures at play in the maintenance of these systems.

#### 6.1 Approximately how many individuals need to participate in a **small change** to the system?

In this question, we are trying to get a sense of the communication overhead involved with making **small changes** and **large changes**. We provide examples of each below.

**small changes**

- A small piece of code in a system needs to be modified to update business logic for processing a particular data source.

- A new sheet needs to be added to an Excel Workbook to perform a new calculation in a system.

**large changes**

- An entirely new methodology was recently discovered and needs to be introduced as one of the options in an elementary aggregate system.

- A system was previously ingesting survey data, but it now needs to also ingest retail scanner data.

For the purposes of this question:

- Any individual stakeholder who **needs to be consulted** in order to implement **any part of the change** should be included in your count.

- We want to know how many individuals are required to implement **the entire change** (i.e., from inception to verifying the change is behaving correctly in the production system).

- In the event that you have multiple systems in a given GSBPM step, please include the instance where the **largest number of individuals** is required to get a small change through.

In the table below, please select approximately how many individuals are required to participate in a small change to a system.

- [ ] 1 individual
- [ ] 2-3 individuals
- [ ] 4-6 individuals
- [ ] 7-9 individuals
- [ ] 10-15 individuals
- [ ] 16-24 individuals
- [ ] 25 or more individuals

| Data ingestion | Data processing | Elementary indexes | Aggregation | Finalization |
| ---            | ---             | ---                | ---         | ---          |
| Select band  | Select band | Select band |  Select band |  Select band | 

#### 6.2 Approximately how many individuals need to participate in a **large change** to the system?

**Note**: Same caveats/explanations for 6.2 as 6.1, except we are thinking about **large changes** instead of small changes.

| Data ingestion | Data processing | Elementary indexes | Aggregation | Finalization |
| ---            | ---             | ---                | ---         | ---          |
| Select band  | Select band | Select band |  Select band |  Select band | 

## Section 3: Business Outcomes

In this question, we are interested in business outcomes of interest for NSOs.

### Question 1: Adoption of Alternative Data

#### 1.1 Do you currently make use of alternative data in **any** of your production systems?

For the purposes of this question, when we refer to **alternative data**, we mean any data that was not field collected.

- [ ] Yes
- [ ] No

-------

#### 1.1.1 `(1.1 == "No")` Do you intend/want to make use of alternative data in the future?

- [ ] Yes
- [ ] No

#### 1.1.2 `(1.1 == "Yes") or (1.1.1 == "Yes")` What are the main challenges you face in adopting alternative data?

Please select all that apply

- [ ] Lack of availability of alternative data
- [ ] Lack of funding to procure alternative data
- [ ] Lack of mandate (not a priority)
- [ ] Lack of skills **within the domain team** to make use of alternative data 
- [ ] Lack of organizational capacity (e.g., corporate IT could assist, but there is no capacity)
- [ ] Lack of methodological knowledge to calculate price statistics from alternative data
- [ ] So costly that it is not worth implementing
- [ ] None of these (we do not face any challenges)
- [ ] None of these (please specify other)


#### 1.2 `(1.1 == "Yes")` Approximately what share of your price statistic by expenditure weight **is currently** comprised of alternative data?

- [ ] Less than 10%
- [ ] 10% to 20%
- [ ] 20% to 30%
- [ ] 30% to 40%
- [ ] 40% to 50%
- [ ] 50% to 60%
- [ ] 60% to 70%
- [ ] 70% to 80%
- [ ] 80% to 90%
- [ ] 90% to 100%

#### 1.2.1 `(1.1 == "Yes")` Approximately what share of your price statistic by expenditure weight **would you like to be** comprised of alternative data in your target state?

- [ ] Less than 10%
- [ ] 10% to 20%
- [ ] 20% to 30%
- [ ] 30% to 40%
- [ ] 40% to 50%
- [ ] 50% to 60%
- [ ] 60% to 70%
- [ ] 70% to 80%
- [ ] 80% to 90%
- [ ] 90% to 100%

#### 1.2.2 `(1.1 == "Yes")` Approximately what share of your alternative data price statistics are comprised of each of the following methodologies?

**Note**: we can present the following bands in each cell.

- [ ] Less than 10%
- [ ] 10% to 20%
- [ ] 20% to 30%
- [ ] 30% to 40%
- [ ] 40% to 50%
- [ ] 50% to 60%
- [ ] 60% to 70%
- [ ] 70% to 80%
- [ ] 80% to 90%
- [ ] 90% to 100%

| Simple Implementation | Dynamic Sample Method | Multilateral Method | Something Else |
| --- | --- | --- | --- |
| Select band | Select band | Select band | Select band |

### Question 2: Flexible or Rigid (Lead Time)

In this question, we are concerned with how time consuming it is to introduce changes to systems in each GSBPM step.

#### 2.1 How long do changes take?

In this question, we are concerned with specifically **how long** it takes to introduce changes to the system. More precisely, we want to know how long it takes to implement **small changes** and **large changes**. For examples of each type of change, please refer to the examples shown in Section 2, Question 6.

##### 2.1.1 Small Changes

In the table below, for **small changes** for each GSBPM step, please indicate how many of your systems fall into each time window.

Definitions of each time window can be found below.

- **within 1 day**: changes can be completed in 1 working day.

- **within 1 week**: changes can be completed within 1 week.

- **within 1 month**: changes can be completed within 1 month.

- **within 3 months**: changes can be completed within 3 months.

- **within 1 year**: changes can be completed within 1 year.

- **more than 1 year**: changes takes longer than 1 year to be implemented.

- **too complex**: the system is so complex that it is too risky or costly (or both) to make changes to it.

- **can't be modified**: the system is offered by a third part (e.g., COTS software), and changes are out of scope.

| GSBPM Step | within 1 day | within 1 week | within 1 month | within 3 months | within 1 year | more than 1 year | too complex | can't be modified |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Ingestion | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Validation | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Elementals | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Aggregation | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Finalization | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |

##### 2.1.2 Large Changes

In the table below, for **large changes** for each GSBPM step, please indicate how many of your systems fall into each time window.

Definitions of each time window are the same as in 5.1.1

| GSBPM Step | within 1 day | within 1 week | within 1 month | within 3 months | within 1 year | more than 1 year | too complex | can't be modified |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Ingestion | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Validation | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Elementals | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Aggregation | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |
| Finalization | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # | Select # |

### Question 3: Challenges in Developing Production Systems

Please indicate which of the following challenges your team faces with respect to **system development and maintenance in general**. Please select all that apply.

- [ ] Managing the interaction **between** systems (e.g., integration challenges, passing inputs/outputs between systms).
- [ ] Complexity **within** a system (e.g., managing complex code, managing large quantities of code).
- [ ] Keeping track of which version of a system was used to produce a certain version of an output.
- [ ] Human coordination/communication overhead (e.g., lots of people need to be involved with every decision).
- [ ] Communication challenges **between** teams (e.g., prices domain team struggles to communicate requirements with corporate IT).
- [ ] Lots of manual tasks that are not automated/cannot be automated (e.g., a person has to manually review system outputs to validate them).
- [ ] Lack of skills (e.g., people do not have the skills to maintain complex systems)
- [ ] Organizational politics (e.g., mandate conflicts between corporate IT and the prices domain team)
- [ ] Verifying that a system behaves correctly (e.g., the price index calculation logic is correct)
- [ ] Verifying the correctness of data (e.g., input data often contains mistakes, significant time is spent negotiating error fixes with the data provider, etc.)
- [ ] Lack of software tools (e.g., certain necessary software is not approved by corporate IT, a COTS product cannot be procured).
- [ ] Lack of hardware (e.g., the only device provided is a single work computer, and this device does not have enough CPU/memory/storage to work with large volumes of data).
- [ ] Bureaucratic and process challenges (e.g., many "approval" steps are required to move work forward)
- [ ] Staff/resourcing challenges (e.g., not enough people to do the work, all of our time is spent maintaining existing systems, so there is little/no capacity to develop new systems).
- [ ] We don't have a "testing" environment, so we have to be really confident that are changes are correct before testing them live in our production system.
- [ ] None of these (we do not face any challenges).
- [ ] Other challenges that are not listed here (please specify).
