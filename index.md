## Scraping a table from Wikipedia using Python

We are going to take a look at how to extract data from a table on a wikipedia page. First, lets take a look at the website we would like to scrape the data from. [We will use this page](https://en.wikipedia.org/wiki/List_of_largest_law_firms_by_profits_per_partner).

### Step 1  -- Import Modules

We will need to use the following modules to complete our web-scraping:

```markdown

import requests
from bs4 import BeautifulSoup
import pandas as pd

```
### Step 2 -- Save URL into a variable:

Simply copy and paste the URL into a variable, here we name the variable "URL". This will make it easier to refer to our website in our code.

```markdown

URL='https://en.wikipedia.org/wiki/List_of_largest_law_firms_by_profits_per_partner'

```
### Step 3 -- Create a function to extract the data from wikipediua into a Pandas DataFrame:

```markdown

def get_table(URL, n=0):        #use n to get which table you want (0 is the default and will be the first table)
    return pd.DataFrame([[cell.text for cell in row.find_all(["th","td"])]
        for row in BeautifulSoup(requests.get(URL).content, 'html.parser').find_all("table")[(n)].find_all("tr")])

```

## Step 4 -- Create a function to format the resulting DataFrame

When the data is first extracted, the output will not be formatted correctly, so we need to write a code to automatically format the table:

```markdown

def Format(df):
    df.set_axis([x.rstrip() for x in df.iloc[0]], axis=1, inplace=True)
    df.drop(0, inplace=True)
    df.reset_index(inplace=True)
    df.drop(['index'], axis=1, inplace=True)
    return df

```

The webpage can be extracted by using both of these fucntions together:

```markdown

Format(get_table(URL))

```

This code will work to extract any page on wikipedia, for example lets look at [this page](https://en.wikipedia.org/wiki/Economy_of_the_United_States?msclkid=3d62c767ae4111ecb1c6599eb44d5376#Data). If we look at the data section on this page, we see a chart showing economic data for the US. Let's extract the data from this table:

```markdown

URL='https://en.wikipedia.org/wiki/Economy_of_the_United_States?msclkid=3d62c767ae4111ecb1c6599eb44d5376#Data'

Format(get_table(URL,3))    #we use three because this is the third table on the wikipedia page.

```

Go try it yourself!


