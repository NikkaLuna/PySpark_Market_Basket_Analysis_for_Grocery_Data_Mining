PySpark Market Basket Analysis for Grocery Data Mining
=============================================

This project implements the Apriori algorithm from scratch for mining frequent itemsets in a distributed environment using PySpark. The goal is to perform a market basket analysis to identify pairs of items that are most frequently bought together in a grocery store. This information can be used to optimize product placement strategies, enhancing cross-sell opportunities.

Project Overview
----------------

The project is divided into two main components:

1.  **Distributed Combination Generation:**
    -   The dataset is partitioned and sent to the worker nodes of the compute cluster.
    -   A Python function generates potential pairs from each partition.
    -   The potential pairs are filtered based on specified conditions.
      
2.  **Filtering at the Master Node:**
    -   The filtered potential pairs from the worker nodes are aggregated at the master node.
    -   Additional filtering is performed to retain only those item pairs that exceed a specified count using PySpark.

Project Structure
-----------------

-   **Jupyter Notebook (`pySpark_apriori.ipynb`)**: This notebook contains the complete implementation of the Apriori algorithm, with cells corresponding to each step in the process.
-   **Python Functions**:
  
    -   `pre_check()`: Combines frequent itemsets of size k-1 to generate k-sized combinations.
    -   `post_check()`: Filters out combinations where all subsets are not present in the frequent itemsets from the previous step.
    -   `count_check()`: Filters out combinations with occurrence counts less than the support count.
    -   `generator()`: Generates and filters frequent itemsets with length k.
    -   `get_singles()`: Prepares the data for input into the generator by extracting frequent single items.
    -   `apriori()`: Implements the Apriori algorithm, generating frequent itemsets from dataset partitions.

Installation and Setup
----------------------

To run this project, you will need the following Python libraries:

-   `itertools`: For creating item combinations.
-   `findspark`: To import PySpark as a regular module.
-   `pyspark`: The core module used in this project, which provides a Python API for Apache Spark.

### Setup Instructions

1.  Install the required Python libraries using pip:

    bash

    `pip install pyspark findspark`


## 2. Configure and initialize Spark:

```python
import findspark
findspark.init()

from pyspark import SparkConf, SparkContext
from pyspark.sql import SparkSession

conf = SparkConf().setAppName("Market Basket Analysis").setMaster('local')
sc = SparkContext(conf=conf)
spark = SparkSession(sc)
```

3.  Open the `pySpark_apriori.ipynb` notebook to start the project.

How It Works
------------

### Step 1: Data Preparation

-   Load the dataset into a PySpark Resilient Distributed Dataset (RDD).
-   Preprocess the data by removing headers and tokenizing rows.

### Step 2: Distributed Apriori Algorithm

-   **Single Itemsets Generation**: Generate frequent single items using the `get_singles()` function.
-   **Frequent Itemset Generation**: Use the `generator()` function to generate frequent itemsets of increasing size.
-   **Combination Filtering**: Use the `pre_check()` and `post_check()` functions to filter and verify frequent combinations.
-   **Count Filtering**: Use the `count_check()` function to filter combinations based on the support count.

### Step 3: Aggregation at Master Node

-   Collect the frequent combinations from all worker nodes.
-   Use an auxiliary function to compute the global support counts for the combinations.
-   Filter the combinations based on the global support count.

Output
------

![Market Basket Analysis Output](https://github.com/NikkaLuna/PySpark_Market_Basket_Analysis_for_Grocery_Data_Mining/blob/main/Output.png)

The final output is a list of frequent item pairs that exceed the specified support count, indicating that they are frequently bought together. These pairs can then be used to optimize the placement of items in the store to increase cross-selling opportunities.
