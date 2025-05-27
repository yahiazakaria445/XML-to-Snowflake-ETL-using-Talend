#  XML to Snowflake ETL using Talend Open Studio

##  Overview

This project demonstrates how to build an ETL (Extract, Transform, Load) pipeline using Talend Open Studio to parse XML files, transform the extracted data, and load it into a Snowflake cloud data warehouse.

![ETL](https://github.com/user-attachments/assets/0d69ad05-2238-4f9f-81f9-602ff939c367)

---

## 📂 Source Data: `film_actor.xml`
The source XML file (`film_actor.xml`) contains data about films and their associated actors. The goal is to extract meaningful information (film and actor details), transform it into a flat tabular structure, and load it into Snowflake.


Each `<film>` node contains:

- **Film metadata**: `film_id`, `title`, `description`, `release_year`, etc.
- A nested `<actors>` list with multiple `<actor>` entries.

**Example XML structure**:

```xml
<film film_id="1">
  <title>ACADEMY DINOSAUR</title>
  ...
  <actors>
    <actor actor_id="1">
      <actor_first_name>PENELOPE</actor_first_name>
      <actor_last_name>GUINESS</actor_last_name>
    </actor>
    ...
  </actors>
</film>

```

## 🛠 ETL Job Breakdown

### 1. Pre-job Setup
  ![ETL1](https://github.com/user-attachments/assets/18dc3a4b-feeb-4042-9c9c-f7b75e9393d2)
- **`tPrejob_1`**: Initializes and prepares the environment.

- **`tFileExist_1`**: Verifies the existence of the input XML file before continuing.

### 2. Extract Phase
- **`XML_parsing (tFileInputXML)`**:
  - Defined via Talend Metadata (File XML).
  - Uses XPath loop: `/films/film/actors/actor`
  - **Extracted fields**:
    - From `<film>`: `film_id`, `title`, `description`
    - From `<actor>`: `actor_id`, `actor_first_name`, `actor_last_name`
- **`tLogRow_1`**: Displays parsed output in a readable tabular format.
  
![ETL2](https://github.com/user-attachments/assets/97d4be30-e7e1-4fb0-a86b-5199131635ab)

### 3. Transform Phase
![etl4](https://github.com/user-attachments/assets/a67c2a82-3fef-46b6-9080-d211005e38f5)

- **`tFilterColumns_1`**: Filters the output to retain only necessary columns.
- **`tLogRow_2`**: Logs filtered data for inspection.
- **`tSortRow_1`**: Sorts records .
- **`tConvertType_1`**: Converts string fields to appropriate data types.
- **`tReplicate_1`**: Splits data into two streams:
  - One for aggregation/logging
  - One for database output
- **`tAggregateRow_1`**: Aggregates data .
- **`tLogRow_3`**: Logs aggregated results.
- **`tMsgBox_1`**: Displays a summary message after execution.

### 4. Load Phase
![ETL3](https://github.com/user-attachments/assets/49e2898e-f801-484f-b79d-0bb6e47e1c21)

- **`tDBOutput_1`**:
  - Loads the final transformed data into Snowflake.
  - Uses a Snowflake connection.
  - Ensures proper data type handling and schema alignment.


