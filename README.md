# Amazon Redshift Statistics Descriptor

![alt text](/img/viz.png "Amazon Redshift Statistics Descriptor")

A lightweight tool based on sweetviz that generates high-density visualizations to kickstart Exploratory Data Analysis within Amazon Redshift using pyodbc with just one line of codee.

## Installation

Copy the `main.py` script and install the requirements located in the dist folder.

```
pip install -r requirements.txt
```

We will also need to download and install the ODBC Driver for Amazon Redshift.

[Download Amazon Redshift ODBC Driver](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-connection.html)

## Getting Started

| Positional argument | Example/Description |
| --- | --- |
| server | komodo-cluster-3000.abcdefghijkl.us-east-2.redshift.amazonaws.com |
| user | awsuser |
| password | specifies the user password |
| database | dev, sample_data_dev |
| schema | public |

| Option | Example/Description |
| --- | --- |
| -h, --help | show this help message and exit |
| -r, --rows | specifies the number of rows to sample from the table (default: 500000) |
| -l, --level | specifies the database object level in which the analysis should be executed, "s" for schema and "t" for table (default: "s") |
| -t, --table | specifies the database table name |
| --associations | indicates that a correlation graph should be generated |
| --open-browser | indicates that a web browser tab should be opened while datasets are analyzed |

The default behaviour of the script will load and analyze the specified number of rows of each table in the selected database schema.

```
python main.py komodo-cluster-3000.abcdefghijkl.us-east-2.redshift.amazonaws.com awsuser S3cUr3P@S$w0rD dev public -r=10000
```

The program will build and save locally high-density HTML visualizations and generate an Excel summary with table name, table rows, data size, table index size and parsed record count in a new folder called **obj**.

![alt text](/img/cmd.png "Amazon Redshift Statistics Descriptor")

If we need a correlation graph to be generated for the columns of each table, we must include the `--associations` flag.

```
python main.py komodo-cluster-3000.abcdefghijkl.us-east-2.redshift.amazonaws.com awsuser S3cUr3P@S$w0rD dev public -r=10000 --associations
```

We must consider that correlations and other associations may take a **quadratic time (n^2)** to complete.

![alt text](/img/associations.png "Amazon Redshift Statistics Descriptor")

If we only need the analysis for a single table we must specify "**t**" as `-l` or `--level` argument value with the corresponding **table name** in `-t` or `--table` argument.

```
python main.py komodo-cluster-3000.abcdefghijkl.us-east-2.redshift.amazonaws.com awsuser S3cUr3P@S$w0rD dev public -r=500000 -l=t -t=sales
```

Finally let's take into consideration that unlike Snowflake or Azure SQL Server, Amazon Redshift does not support `TABLESAMPLE`. For this reason, the present project makes use of `RANDOM()` which represents a more expensive computation.

## Prerequisites

Amazon Redshift Statistics Descriptor was tested with:

* Python: 3.7.16
* Packages:
    * pyodbc: 4.0.39
    * pandas: 1.3.5
    * sweetviz: 2.1.4
    * XlsxWriter: 3.1.0 
* Anaconda: 2.4.0

## License

This project is licenced under the [MIT License][1].

[1]: https://opensource.org/licenses/mit-license.html "The MIT License | Open Source Initiative"