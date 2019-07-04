
## SQL Interface within JupyterLab

#### Learn how to use and modify SQL tables within Jupyter labs.

[**Jupyter Notebooks**](https://jupyter.org/) are an essential part of any Data Science workflow, so much so that many of the organizations like Netflix find them indispensable. The browser-based computing environment, coupled with reproducible document format, has made them the de-facto choice of millions of data scientists and researchers around the globe. Jupyter boasts of a great international community coming from almost every country on earth. A lot of kernels have been developed by the community themselves.

[**Jupyter Lab**](https://jupyterlab.readthedocs.io/en/stable/) is the next-generation user interface for Project Jupyter offering all the familiar building blocks of the classic Jupyter Notebook like the notebook, terminal, text editor, file browser, rich outputs, etc. However, unlike the classic notebooks, all these features are provided in a flexible and powerful user interface. The basic idea of the Jupyter Lab is to bring all the building blocks that are in the classic notebook, plus some new stuff, under one roof

![](https://cdn-images-1.medium.com/max/800/1*jXtKEfOPSXM3utAF4dFF-A.png)

JupyterLab enables showing its work area with notebooks, text files, terminals, and notebook outputs all capable of interacting with each other.


Let’s first get Jupyter Lab installed and running on our systems

#### Installation

JupyterLab can be installed using `conda, pip` or `pipenv.`
```
conda install -c conda-forge jupyterlab
or
pip install jupyterlab
or
pipenv install jupyterlab  
pipenv shell
```
Have a look at the official installation [documentation](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html) for more details.

#### Starting JupyterLab

You can start the Jupyter by simply typing the following at the console:

jupyter lab

JupyterLab will open automatically in the browser with an interface resembling the one below. This means everything is in place, and you are good to go.

![](https://cdn-images-1.medium.com/max/800/1*xo8LGAaxdBCKFQVFb8ZQ3g.png)

The **main work area** is the place where the actual activity takes place. It comprises of the notebooks, documents, consoles, terminals, etc. Just double click or drag a file on to this area to start working.


##  SQL Interface to JupyterLab

![](https://cdn-images-1.medium.com/max/800/1*CsAk3iZVasWdIlpwAw-UOQ.png)

  
Most of the times, the data, that we work with are stored in files called databases, and an essential task of a Data Scientist is to be able to access data from databases and then analyze them. Therefore it is a great idea to have a seamless interface between SQL databases and Jupyter Notebook/Lab so that accessing and manipulating data becomes easier and efficient. This interface can be achieved in two possible ways:

### 1. Using IPython SQL Magic extension

**Magic commands** are a set of convenient functions in Jupyter Notebooks that are designed to solve some of the common problems in standard data analysis. You can see all available magics with the help of `%lsmagic`.

[IPython SQL magic](https://github.com/catherinedevlin/ipython-sql)  extension makes it possible to write SQL queries directly into code cells as well as read the results straight into pandas DataFrames.  This works for both the traditional notebooks as well as the modern Jupyter Labs.

**Installing SQL module in the notebook**
```
 !pip install ipython-sql
```
**Loading the SQL module**
```
 %load_ext sql
```
The above magic command loads the `ipython-sql` extension. We can connect to any database which is supported by [SQLAlchemy](https://www.sqlalchemy.org/). Here we will connect to an SQLite database. Enter the following command in the code cell:

    %sql sqlite://

If you get the output as  `‘Connected: @None',` this means the connection has been established.

-   **Creating a database**

Finally, we create a demo table called EMPLOYEES to showcase the function.
```
%%sql   
CREATE TABLE EMPLOYEE(firstname varchar(50),lastname varchar(50));  
INSERT INTO EMPLOYEE VALUES('Tom','Mitchell');  
INSERT INTO EMPLOYEE VALUES('Jack','Ryan');
```
We can now execute queries on our database
```
 %sql SELECT * from EMPLOYEE;  
 ----------------------------------  
 * sqlite://  
 Done.
```
The above query outputs the following table

![](https://cdn-images-1.medium.com/max/800/1*OpFSVHERmrDtM0puWs4MkQ.png)

-   **Working with an existing database**

We can also connect to an existing database using the magic function. For this article, we will be making use of the SQL_SAFI database(**Studying African Farmer-led Irrigation (SAFI) database)**. The [SAFI Project](http://www.safi-research.org/) is a research project looking at farming and irrigation methods used by farmers in Tanzania and Mozambique. This dataset is composed of survey data relating to households and agriculture.  You can download it from [here](https://datacarpentry.org/sql-socialsci/data/SQL_SAFI.sqlite).
```
 #Specifying the path of the database  
 %sql sqlite:////Users/Parul/Desktop/SQL_SAFI.sqlite  
 -----------------------------------------------------------
    
 'Connected: @/Users/Parul/Desktop/SQL_SAFI.sqlite'
```
The above statement opened the database named `SQL_SAFI.sqlite` that resides in the path: `Users/Parul/Desktop.` Let's select the `Crops` table and display its contents.

    %sql select * from Crops

![](https://cdn-images-1.medium.com/max/800/1*RSCgeAG9TlurUUbl2W4COQ.png)

Displaying first few rows of the Crops Table
```
 %sql select * from Crops where D_curr_crop = "maize" AND ID <= 3;
```

![](https://cdn-images-1.medium.com/max/800/1*-qFwjmQ3I0nL4NTF95tigw.png)

The result can also be converted to a pandas dataframe, as follows:
```
result = %sql select * from Crops where D_curr_crop = "maize" AND ID <= 3;  
dataframe = result.DataFrame()
```

![](https://cdn-images-1.medium.com/max/800/1*60fLCyz_9L2Orp-SA4Ngow.png)

    dataframe.info()

![](https://cdn-images-1.medium.com/max/800/1*uQyX0z2zdTOhfXLP8dGgzw.png)



###  2. The jupyterlab-sql extension

Before going further, let’s talk a little about the Jupyter extensions in general

**JupyterLab Extensions**

Extensions are one of the most useful features of Jupyter Lab and can enhance the Jupyter lab experience. Jupyter Lab is essentially itself an extensible environment with the notebooks, terminal, and editor, all present as an extension.

JupyterLab extensions are **npm packages** (the standard package format in Javascript development). Extensions provide a way to customize and enhance the JupyterLab experience by providing several useful options like new themes, viewers, keyboard shortcuts, advanced settings options, to name a few. There are many community-developed extensions on GitHub. You can search for the GitHub topic [jupyterlab-extension](https://github.com/topics/jupyterlab-extension) to find extensions.

To install JupyterLab extensions, you need to have [Node.js](https://nodejs.org/) installed which can either be installed from their [website](https://nodejs.org/en/) or as follows.
```
conda install -c conda-forge nodejs  
or  
brew install node #for MacOS users
```

The `jupyterlab-sql extension` is one such useful extension that lets you add a SQL user interface to Jupyter Lab. This has two primary advantages:

-   The SQL tables can be explored with a simple point and click.
-   Tables can also be modified and read with custom queries.

You can read more about it at the official [Github repository](https://github.com/pbugnion/jupyterlab-sql).

#### Installation

To install `jupyterlab-sql`, run the following commands in the given order:

```
pip install jupyterlab_sqljupyter serverextension enable jupyterlab_sql --py --sys-prefixjupyter lab build
```

You will then need to restart any running Jupyter servers.

Also, it is important to note that `jupyterlab-sql` only works with Python 3.5 and above.

#### Getting Started

After you restart the Jupyter Lab, a new SQL icon appears in the launcher

![](https://cdn-images-1.medium.com/max/800/1*5pz5b0BLWWMQBZb9TurfOQ.png)

This means, everything is installed correctly, and you are ready to connect your instance to a database.

#### Connection

For connecting to the database, you need to have a valid database URL. `jupyterlab-sql` has been extensively tested against SQLite, PostgreSQL, and MySQL databases. Follow the [SQLAlchemy guide](https://docs.sqlalchemy.org/en/13/core/engines.html#database-urls) on URLs. The URL must be a database URL, for instance:
```
postgres://localhost:5432/postgres  
postgres://username:password@localhost:5432/postgres  
mysql://localhost/employees  
sqlite://  
sqlite:///myfile.db
```

Let us connect to the existing **SQL_SAFI** database, that we used earlier. Click on the SQL icon in the launcher and type in the database url. Press `enter` to connect.

![](https://cdn-images-1.medium.com/max/800/1*Ib0TkQngX_zg3aItxjdwLA.png)

As soon as the database gets connected, you can view all your tables in the database.

![](https://cdn-images-1.medium.com/max/800/1*coiNkwjjJ7rPdtFw9f5b8Q.png)

![](https://cdn-images-1.medium.com/max/800/1*xWPH1gj1IQAbRa-NHyesaQ.gif)

Next, we can also write custom SQL queries to get the desired data from the tables.


### Conclusion

Using SQL and JupterLab together takes the data analysis to the next level. The jupyter-sql interface makes it very easy to connect the SQL Server to Jupyter ecosystem and extract the data directly into it, without having to leave the Jupyter interface.




