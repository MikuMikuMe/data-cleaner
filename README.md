# data-cleaner

To create a complete Python program for a data-cleaning project named "data-cleaner," we will build a simple ETL (Extract, Transform, Load) tool. This tool automates the process of loading a dataset, performing data cleaning and transformation operations, and then exporting the refined dataset in an analysis-ready format. We'll also include useful comments and basic error handling.

For this example, we'll assume the datasets are in CSV format. The tool will handle missing values, cast data types, remove duplicates, and more. We'll use the Pandas library for data manipulation.

```python
import pandas as pd
import numpy as np

class DataCleaner:
    def __init__(self, input_filepath, output_filepath):
        """
        Initialize the DataCleaner with input and output file paths.
        
        :param input_filepath: Path to the input CSV file.
        :param output_filepath: Path where the cleaned data will be saved.
        """
        self.input_filepath = input_filepath
        self.output_filepath = output_filepath

    def extract_data(self):
        """
        Extract data from the input CSV file.
        
        :return: DataFrame containing the extracted data.
        """
        try:
            df = pd.read_csv(self.input_filepath)
            print(f"Data successfully extracted from {self.input_filepath}.")
            return df
        except FileNotFoundError:
            print(f"Error: The file {self.input_filepath} was not found.")
            raise
        except Exception as e:
            print(f"An error occurred while extracting data: {e}")
            raise

    def transform_data(self, df):
        """
        Transform the DataFrame by cleaning and preparing it.
        
        :param df: Input DataFrame to be transformed.
        :return: Transformed DataFrame.
        """
        try:
            # Drop duplicates
            df.drop_duplicates(inplace=True)
            print("Duplicates removed.")

            # Fill missing values - fill numeric NaNs with mean, categorical NaNs with mode
            for column in df.columns:
                if df[column].dtype == np.number:
                    df[column].fillna(df[column].mean(), inplace=True)
                else:
                    df[column].fillna(df[column].mode()[0], inplace=True)
            print("Missing values handled.")

            # Example transformation: strip whitespace from string columns
            for column in df.select_dtypes(include='object').columns:
                df[column] = df[column].str.strip()
            print("String columns cleaned.")

            # Ensure proper data types (as an example, ensure all numbers are float)
            for column in df.select_dtypes(include=['int64', 'float64']).columns:
                df[column] = df[column].astype(float)
            print("Data types casted appropriately.")

            return df
        except Exception as e:
            print(f"An error occurred during data transformation: {e}")
            raise

    def load_data(self, df):
        """
        Load the transformed DataFrame to a CSV file.
        
        :param df: Transformed DataFrame to be saved.
        """
        try:
            df.to_csv(self.output_filepath, index=False)
            print(f"Data successfully loaded to {self.output_filepath}.")
        except Exception as e:
            print(f"An error occurred while loading data: {e}")
            raise

    def run_etl(self):
        """
        Run the complete ETL process: extract, transform, load.
        """
        try:
            df = self.extract_data()
            df_transformed = self.transform_data(df)
            self.load_data(df_transformed)
            print("ETL process completed successfully.")
        except Exception as e:
            print(f"ETL process failed: {e}")

if __name__ == "__main__":
    # Define input and output file paths
    input_filepath = 'dirty_dataset.csv'
    output_filepath = 'cleaned_dataset.csv'
    
    # Create a DataCleaner instance
    data_cleaner = DataCleaner(input_filepath, output_filepath)
    
    # Run the ETL process
    data_cleaner.run_etl()
```

### Key Points:

1. **File Handling:** Basic error handling for file-related errors is implemented.
2. **Data Transformation:** 
   - Duplicate rows are removed.
   - Missing values are filled with column mean for numerical data or mode for categorical data.
   - String trimming is applied to strip whitespace.
   - Numeric columns are cast to float for consistency.
3. **ETL Flow:** The program follows a clear ETL pattern: Extract -> Transform -> Load.
4. **Comments:** The code contains comments to describe each stage.

Before running the program, ensure that the necessary libraries (Pandas and NumPy) are installed and replace the `'dirty_dataset.csv'` and `'cleaned_dataset.csv'` with the actual paths of your input and output files.