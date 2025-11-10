# dune-api-guide
## Fetching Dune API Data with Python in Google Colab

A simple guide to get query results from the Dune API as a CSV file using the `dune-client` library in a Google Colab notebook.

This guide provides a complete Python script, but you will need:
* A Dune account with an API Key.
* The Query ID of the query you want to fetch.

---

### Step 1: Install Libraries in Colab

Run this command in a Colab cell to install the necessary libraries.

```python
!pip install dune-client pandas
````

-----

### Step 2: Add Your Dune API Key (Securely)

Use Colab's "Secrets" feature to keep your API key safe.

1.  Click the **"Key" icon** (Secrets) in the left-hand sidebar of your Colab notebook.
2.  Click **"Add a new secret"**.
3.  For the **Name**, enter `DUNE_API_KEY`.
4.  In the **Value** field, paste your API key.
5.  Turn on the **"Notebook access"** toggle.

-----

### Step 3: Python Script to Fetch Data

Copy and paste this code into a new Colab cell.

**Remember to change two variables:**

1.  `QUERY_ID`: Set this to your query's ID.
2.  `file_name`: Set this to your desired output CSV filename.

<!-- end list -->

```python
import pandas as pd
from dune_client.client import DuneClient
from google.colab import userdata

try:
    my_api_key = userdata.get('DUNE_API_KEY')
except Exception as e:
    print("Error: Could not find 'DUNE_API_KEY' in Colab secrets.")
    print("Please follow Step 2 to add your API key.")

if 'my_api_key' in locals():
    dune = DuneClient(my_api_key)
    QUERY_ID = 1234567
    print(f"Fetching latest results for Query ID: {QUERY_ID}...")
    try:
        results_df = dune.get_latest_result_dataframe(QUERY_ID)
        file_name = "results.csv"
        results_df.to_csv(file_name, index=False)
        print(f"\nSuccess! Data saved to '{file_name}'.")
        print("You can find the file in the Colab file browser (folder icon on the left).")
        print("\nData preview:")
        print(results_df.head())
    except Exception as e:
        print(f"\nAn error occurred:")
        print(e)
        print("\nPlease check that your Query ID is correct and is a public query or your own.")
```

Feel free to send a DM on X for feedback/ help implementing: @brown_ux
