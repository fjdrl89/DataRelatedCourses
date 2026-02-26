# Welcome to the Data Scientist: Analytics Specialist Career Path

This Career Path is divided into 2 parts:
- The first section covers all the basics every Data Scientist needs. We call this the Foundations of Data Science.
- The second section is the specialized KSAs you need to go from being a data generalist to an Analytics-based Data Scientist.

In the first half of this Career Path, you will learn about Python, SQL, statistics, and data best practices. You will start with a code-free introduction to working with data in the Data Literacy Track and then dive into SQL and Python in the SQL Fundamentals and Python Fundamentals Tracks. SQL and Python are essential tools that will give you the technical foundation you need to work with data programmatically.

Then you will dive into Data Manipulation with Python Pandas. Pandas is a Python library at the heart of Data Science. It is an essential tool for working with data and has dozens of useful built-in functions. Once you have mastered Pandas, you will apply your new programming skills to Exploratory Data Analysis where you will learn to summarize datasets before mastering the Fundamentals of Statistics where you will learn to spot trends and patterns.

Next, you will learn how to transform those numbers and analyses into meaningful Data Visualizations. Then you will apply your new skills to Wrangle and Tidy Datasets and prepare real data for analysis. Finally, you will bring it all together to Communicate Data Science Findings to various groups and stakeholders.

In the Analytics Specialty, you will learn the most practical tools and techniques to get meaning from data. You’ll use data responsibly to drive decision-making, and give evidence for and against various hypotheses. You’ll supplement your programming skills by developing proficiency in BI Tools such as Excel and Tableau.

By the end of the Data Scientist: Analytics Specialist Career Path, you will be able to:

- Create programs with Python 3
-Query and manipulate data with SQL and Python pandas
- Create data visualizations with Python matplotlib and seaborn
- Summarize and analyze datasets
- Conduct hypothesis testing
- Design experiments
- Clean and tidy datasets
- Use statistical methods to evaluate large and small datasets
- Answer business questions with data
- Build dashboards to tell a data story with Tableau
- Use advanced SQL techniques to analyze data efficiently and communicate your findings
- Decide when to use BI Tools vs. programming tools to solve problems and answer questions
- Leverage Jupyter Notebooks for experimentation and communication

Along the way, you will build a portfolio of projects to showcase your data science KSA’s, and prove your job-ready skills!

# Principles of Data Literacy

## Introduction to Data

### Welcome to Data Literacy

Garbage in, garbage out is a data-world phrase that means “our data-driven conclusions are only as strong, robust, and well-supported as the data behind them.”

Part of understanding and communicating with data means asking the right questions so that we end up with useful, relevant data.

Part of practicing good data literacy means asking…

- Do we have sufficient data to answer the question at hand?
- Can my data answer my exact question?

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/764083e2-dde9-46e9-952f-ca78c22d4a20" />

### **Case Studies in Data Literacy** _(Lesson)_

#### Addressing Bias

Part of practicing good data literacy means asking…

- Who participated in the data?
- Who is left out?
- Who made the data?

#### What is Statistics?

- Statistics helps us test the likelihood of an event happening by random chance versus systematically.

#### High Stakes Visualization

- Data visualization is one of the most visible and obvious places we interact with data.
- It helps us to explore and understand data-driven arguments and is a powerful tool for communication.

#### Causal Analysis and John Snow's cholera theory

- In the world of data, we’ll hear time and time again that “correlation does not equal causation.”
- In other words, while two events might be connected or related, that doesn’t mean they’re in a cause-and-effect relationship.
- A “causal link” means proving that one event causes another.

### **Data Collection Methods, Ethics and Free Sources** _(Article)_

#### Understanding data ethics and privacy

Whenever we talk about data collection, we need to discuss data ethics. Much of the data available to us comes from individuals and would be considered personally identifiable, meaning we could use it to identify someone. Many people use the acronym PII (pronounced “pie” — yum!) for the term “personally identifiable information”. Examples of PII are address, email, phone number, social security numbers, credit card numbers, and medical records. We all have an obligation to protect personally identifiable information.

Ethical issues regarding data collection may be divided into the following categories:

- **Consent:** Individuals must be informed and give their consent for information to be collected.
- **Ownership:** Anyone collecting data must be aware that individuals have ownership over their information.
- **Intention:** Individuals must be informed about what information will be taken, how it will be stored, and how it will be used.
- **Privacy:** Information about individuals must be kept secure. This is especially important for any and all personally identifiable information.

#### Methods of Collecting Data

We collect, process, and analyze data to better understand our world and make more informed decisions. The first step in any data work is to collect the data itself. Data can come from a lot of places, including research, governments, technology, observation, or directly from individuals — the list is endless!

We collect this data in many different ways. One way is to seek out information that doesn’t yet exist and measure it directly. This can include activities like surveys, observational studies, or recording the results of an experiment. This kind of data might be considered static, meaning the information is collected once and does not change. Think about conducting a survey by mail: the survey results are collected and recorded only once.

Data can also be live and ever-changing based on the most up-to-date information. For example, apps and websites can track clicks and time spent on pages across multiple users at the same time without a human actively recording all the data points. Unlike the static data of more traditional methods, sensors and trackers can also continuously update data to include new information in a live feed. Think about weather predictions: the data that goes into weather predictions are updated continuously to get the most accurate predictions.

Finally, rather than collect measurements directly, we can also use existing data that was collected by others or for some other purpose. There are lots of databases that are freely available for public use. We can even compile data from a variety of sources and join them together before an analysis.

#### Where to find free datasets for analysis?

Many organizations house all kinds of data. Datasets are often kept private or can only be accessed for a fee. This may be done for reasons like protecting the identity of individuals, keeping valuable information from competitors, or making a profit from data collection.

The following list has links and descriptions for websites that provide free access to some interesting datasets. The companies and organizations on this list provide public access to data, allowing anyone with internet access to view this information. The websites vary in how they provide data access: some may have a CSV or Excel file of data that can easily be downloaded to a computer, while others allow access to a database via an API.

- [World Health Organization (WHO)](https://www.who.int/data/gho/): Data available on the WHO’s site cover a variety of health-related topics, such as COVID-19, air pollution, and even brain health. There are fact sheets and direct access to various datasets, including:
  - [Mental health](https://www.who.int/health-topics/mental-health#tab=tab_1)
  - [Road traffic mortality](https://www.who.int/data/gho/data/themes/topics/topic-details/GHO/road-traffic-mortality)
- [FiveThirtyEight](https://data.fivethirtyeight.com/): This is a very popular analysis website that provides direct access to some of their datasets. Topics include sports, politics, science & health, culture, and economics. Check out some of these interesting finds:
  - [Political polls](https://projects.fivethirtyeight.com/polls/)
  - [The best NBA players](https://projects.fivethirtyeight.com/nba-player-ratings/)
- [Data.gov](https://www.data.gov/): The U.S. government has its own open data collection. The site includes information on agriculture, climate, energy, and many other topics. Here are a few unique datasets:
  - [Storm Events Database](https://catalog.data.gov/dataset/ncdc-storm-events-database2)
  - [Census data for the United States of America](https://www.census.gov/data.html)
  - [Census data for EU countries](https://ec.europa.eu/eurostat/web/population-demography/population-housing-censuses)
- [Data Unicef](https://data.unicef.org/): UNICEF’s Data and Analytics team provides global access to data on children. This organization believes that the right data in the right hands can help us make informed and equitable decisions. You can view a variety of topics and data from various countries.
Image of computer screen with Google search homepage open in a web browser.

Looking for something specific? [Google Dataset Search](https://datasetsearch.research.google.com/) works like a google search bar for datasets. We think the following datasets look really interesting!

- [Orchids](https://datasetsearch.research.google.com/search?src=0&query=Value%20of%20the%20import%20and%20export%20of%20orchids%20in%20the%20Netherlands%202020&docid=L2cvMTFweDF5bnRzOQ%3D%3D) — Did you know the total value of trees, plants, and flowers exported from the Netherlands in 2021 was nearly 12 billion euros?
- [Biodiversity at U.S. national parks](https://datasetsearch.research.google.com/search?query=national%20parks&docid=L2cvMTFqbl82ZmdmeQ%3D%3D) — Did you know that Haliaeetus leucocephalus (also know as a Bald Eagle) can be found in just about every U.S. National Park? Check out this data file to explore animal and plant species that have been identified and verified by evidence in national parks.
- [Revenue of the cosmetic & beauty industry in the U.S.](https://datasetsearch.research.google.com/search?query=cosmetics&docid=L2cvMTFuZmJqOWtsXw%3D%3D) — Talk about big money: the revenue of the U.S. cosmetic industry was estimated to amount to about 49.2 billion U.S. dollars in 2019.

#### Conclusion

Understanding data collection methods, ethics, and sources is essential for responsible data analysis. Ethical considerations ensure privacy and fairness, while diverse data collection techniques help gather accurate insights. Free datasets from trusted sources like WHO, Data.gov, and Google Dataset Search provide valuable opportunities for exploration. By applying these principles, you can make informed, ethical decisions in data-driven projects.

### **Data Types and Quality** _(Lesson)_




























## Thinking about Data


## Visualizing Data


## Analyzing Data



# Learn SQL

# Python Fundamentals for Data Science (Part I)

# Python Fundamentals for Data Science (Part II)

# Portfolio Project: U.S. Medical Insurance

# Python Pandas for Data Science

# Exploratory Data Analysis in Python

# Statistics Fundamentals for Data Science

# Data Visualization Fundamentals with Python

# Portfolio Project: Data Visualization

# Data Wrangling, Cleaning, and Tidying

# Communicating Data Science Findings

# Data Science Foundations Portfolio Project

# Next Steps: The Analytics Specialty

# Advanced Exploratory Data Analysis

# Visualization for Data Science Applications

# Advanced SQL for Data Science

# Learn Tableau for Data Visualization

# Learn Microsoft Excel for Data Analysis

# Data Scientist: Analytics Specialist Final Portfolio Project

# Data Scientist: Analytics Specialist Final Review



