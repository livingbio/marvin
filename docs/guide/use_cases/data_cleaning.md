# Data cleaning

> "Data cleaning is 80% of data science."
>
> -- Everyone

Data cleaning is an essential step in the data analysis pipeline. Ensuring that your data is in the right format and free of inconsistencies, errors, or missing values can greatly improve the quality of your results and help you draw accurate conclusions. While some aspects of data cleaning are straightforward and can be addressed through methods that are straightforward to express in code, such as replacing null values with 0, removing missing data, trimming outliers, or changing capitalization, there are other aspects that are more complex and harder to handle programmatically.

That's where AI functions come in. These powerful AI-enabled data tools can tackle complex data cleaning tasks that are difficult to automate using traditional techniques. Examples include converting values into less-granular categories, filling missing data based on context, extracting entities, and standardizing responses. In this document, we will briefly overview the LLM-powered data cleaning functions and demonstrate how they can be utilized to enhance your data cleaning process.

## AI functions for data

AI functions provide a sophisticated and efficient solution for handling complex data-related tasks. These functions offer a distinct advantage over traditional programmatic methods, as they are equipped to manage tasks that are difficult or impossible to achieve using conventional techniques.

One of the primary benefits of AI functions for data is their ability to understand context. In many cases, the context surrounding a piece of data is crucial for correctly transforming or filling in missing values. For example, when filling in missing state information for cities, an AI function can leverage its vast knowledge of geographical information to accurately determine the correct state. This context-awareness enables AI functions for data to handle a wide variety of tasks that would otherwise require significant manual effort or intricate, custom-built algorithms.

Another key advantage of AI functions for data is their capacity to handle less structured data or data that involves some level of human interpretation. For instance, when data needs to be categorized or mapped to a specific set of values, AI functions can utilize their understanding of language and concepts to accurately determine the appropriate category for each data point. This is particularly useful when dealing with survey responses or other forms of textual data that may not adhere to a standardized format. Importantly, AIs can map granular data to either a known set of categories or determine the appropriate categories on their own. 

AI functions for data also excel at entity extraction, a process that involves pulling structured data from unstructured text. Examples of entity extraction include converting items on a resume to JSON or extracting zip codes or countries from addresses. By leveraging their language understanding capabilities, AI functions can effectively parse unstructured text and identify key pieces of information, making it easier to analyze, manipulate, and integrate this data into other systems.

In addition, AI functions for data can standardize data in various formats, such as phone numbers, dates, addresses, or names. This allows for more consistent data representation and improves compatibility between different datasets. These functions can automatically detect various formats and convert them into a unified standard, saving time and reducing the risk of errors that can arise from manual data standardization.

## Marvin's data functions

### Categorization

Marvin has two AI functions for categorization. `map_categories` maps a list of data to a set of known categories, while `categorize` maps a list of data to a set of categories defined by a natural language description.

For example, the following are essentially equivalent ways of converting a list of color names to its nearest "primary" color:

```python
from marvin.ai_functions.data import map_categories, categorize

colors_data = [
    "teal", 
    "cyan",
    "peach",
    "salmon",
    "red-orange",
    "lime",
    ...
]

# assign categories from a known list
map_categories(colors_data, categories=['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'])

# describe possible categories in natural language
categorize(colors_data, description='the colors of the rainbow')
```

### Standardization

Marvin has a `standardize` function that takes a list of data and converts it to a standard described in natural language. For example:

```python
from marvin.ai_functions.data import standardize

survey_data=[
    "(555) 555 5555", 
    "555-555-5555", 
    "5555555555", 
    "555.555.5555"
    ]

# converts all values to (555) 555-5555
standardize(survey_data, format="US phone number with area code")
```

### Context-aware fill for missing values

Dealing with missing values is always difficult. There are many techniques for dealing with missing values, including removing them, filling them with a known value, or using analytical methods that properly account for them. AI functions introduce a new option: filling them based on context. Note that this approach can only be used when there are strong correlations in the dataset; otherwise, the context is not useful for predicting missing values. This is also a form of data augmentation and will not be appropriate for all user cases.

Marvin has two functions for context-aware fill that differ only in input and return types. `context_aware_fillna` takes a matrix of data (organized as list[list] such that each inner list is a row of data) and column names; `context_aware_fillna_df` takes and returns Pandas dataframes.

```python
from marvin.ai_functions.data import context_aware_fillna_df

movies_data = pd.DataFrame(
    [
        ["The Terminator", 1984, None],
        ["Minority Report", None, "Steven Spielberg"],
        ["WALL-E", None, "Andrew Stanton"],
        ["Blade Runner", 1982, None],
    ],
    columns=["title", "release_year", "director"],
)
context_aware_fillna_df(movies_data)

# Result:
#
#                 title  release_year          director
#    0   The Terminator          1984     James Cameron
#    1  Minority Report          2002  Steven Spielberg
#    2           WALL-E          2008    Andrew Stanton
#    3     Blade Runner          1982      Ridley Scott
```