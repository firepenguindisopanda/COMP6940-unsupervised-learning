# COMP6940-unsupervised-learning

# WIDS Datathon 2025 - Dataset Loading Instructions

[WiDS Kaggle Competition Page](https://www.kaggle.com/competitions/widsdatathon2025/data)

[How to get kaggle api key](https://christianjmills.com/posts/kaggle-obtain-api-key-tutorial/)

[Link to the dataset](https://drive.google.com/drive/folders/17Bl12sKJpwn3MXlXP5C4I7yqDwQKAqI1?usp=sharing)

This README provides instructions for loading the WIDS Datathon 2025 dataset using from a local ZIP file.

## Loading Data from a ZIP File

If you don't have a Kaggle account or prefer to work with a local copy of the data, follow these steps:

### Step 1: Download and Extract the ZIP File

1. Download the `widsdatathon2025.zip` file provided to you
2. Extract the ZIP file to a location on your computer
3. Note the path where you extracted the data

### Step 2: Set Up Your Environment

```python
import numpy as np  # linear algebra
import pandas as pd  # data processing, CSV file I/O
import os
import zipfile
```

### Step 3: Load Data from the Extracted ZIP Files

Replace `'/path/to/extracted/folder'` with the actual path where you extracted the ZIP file:

```python
# Set the base path to your extracted data
base_path = '/path/to/extracted/folder'

# Define paths to training data files
train_func_path = os.path.join(base_path, "TRAIN_NEW/TRAIN_FUNCTIONAL_CONNECTOME_MATRICES_new_36P_Pearson.csv")
train_cat_path = os.path.join(base_path, "TRAIN_NEW/TRAIN_CATEGORICAL_METADATA_new.xlsx")
train_quant_path = os.path.join(base_path, "TRAIN_NEW/TRAIN_QUANTITATIVE_METADATA_new.xlsx")
train_target_path = os.path.join(base_path, "TRAIN_NEW/TRAINING_SOLUTIONS.xlsx")

# Define paths to test data files
test_func_path = os.path.join(base_path, "TEST/TEST_FUNCTIONAL_CONNECTOME_MATRICES.csv")
test_cat_path = os.path.join(base_path, "TEST/TEST_CATEGORICAL.xlsx")
test_quant_path = os.path.join(base_path, "TEST/TEST_QUANTITATIVE_METADATA.xlsx")
sample_sub_path = os.path.join(base_path, "SAMPLE_SUBMISSION.xlsx")

# Load training data
try:
    train_data_functional_connectome = pd.read_csv(train_func_path)
    train_data_categorical_metadata = pd.read_excel(train_cat_path)
    train_data_quantitative_metadata = pd.read_excel(train_quant_path)
    train_data_target_variable = pd.read_excel(train_target_path)
    
    print("Successfully loaded training data files")
    print(f"Functional connectome shape: {train_data_functional_connectome.shape}")
    print(f"Categorical metadata shape: {train_data_categorical_metadata.shape}")
    print(f"Quantitative metadata shape: {train_data_quantitative_metadata.shape}")
    print(f"Target variable shape: {train_data_target_variable.shape}")
except Exception as e:
    print(f"Error loading training data: {e}")

# Load test data
try:
    test_data_functional_connectome = pd.read_csv(test_func_path)
    test_data_categorical_metadata = pd.read_excel(test_cat_path)
    test_data_quantitative_metadata = pd.read_excel(test_quant_path)
    sample_submission = pd.read_excel(sample_sub_path)
    
    print("\nSuccessfully loaded test data files")
    print(f"Functional connectome shape: {test_data_functional_connectome.shape}")
    print(f"Categorical metadata shape: {test_data_categorical_metadata.shape}")
    print(f"Quantitative metadata shape: {test_data_quantitative_metadata.shape}")
    print(f"Sample submission shape: {sample_submission.shape}")
except Exception as e:
    print(f"Error loading test data: {e}")
```

### Step 4: Merge the Data

```python
# Merge training data
train_full_df = pd.merge(train_data_functional_connectome, train_data_categorical_metadata, on='participant_id')
train_full_df = pd.merge(train_full_df, train_data_quantitative_metadata, on='participant_id')
train_full_df = pd.merge(train_full_df, train_data_target_variable, on='participant_id')

# Merge test data
test_full_df = pd.merge(test_data_functional_connectome, test_data_categorical_metadata, on='participant_id')
test_full_df = pd.merge(test_full_df, test_data_quantitative_metadata, on='participant_id')

print(f"\nMerged training data shape: {train_full_df.shape}")
print(f"Merged test data shape: {test_full_df.shape}")
```

## Option 2: Loading Data Directly from a ZIP File Without Extraction

If you prefer not to extract the ZIP file, you can read the data directly from the ZIP:

```python
import numpy as np
import pandas as pd
import os
import zipfile
import io

# Path to your downloaded ZIP file
zip_path = '/path/to/widsdatathon2025.zip'

# Function to read files directly from ZIP
def read_file_from_zip(zip_file, file_path):
    with zipfile.ZipFile(zip_file, 'r') as z:
        if file_path.endswith('.csv'):
            return pd.read_csv(io.BytesIO(z.read(file_path)))
        elif file_path.endswith('.xlsx'):
            return pd.read_excel(io.BytesIO(z.read(file_path)))
        else:
            raise ValueError(f"Unsupported file format: {file_path}")

# Define file paths within the ZIP
train_func_path = "TRAIN_NEW/TRAIN_FUNCTIONAL_CONNECTOME_MATRICES_new_36P_Pearson.csv"
train_cat_path = "TRAIN_NEW/TRAIN_CATEGORICAL_METADATA_new.xlsx"
train_quant_path = "TRAIN_NEW/TRAIN_QUANTITATIVE_METADATA_new.xlsx"
train_target_path = "TRAIN_NEW/TRAINING_SOLUTIONS.xlsx"

test_func_path = "TEST/TEST_FUNCTIONAL_CONNECTOME_MATRICES.csv"
test_cat_path = "TEST/TEST_CATEGORICAL.xlsx"
test_quant_path = "TEST/TEST_QUANTITATIVE_METADATA.xlsx"
sample_sub_path = "SAMPLE_SUBMISSION.xlsx"

# Load training data
try:
    train_data_functional_connectome = read_file_from_zip(zip_path, train_func_path)
    train_data_categorical_metadata = read_file_from_zip(zip_path, train_cat_path)
    train_data_quantitative_metadata = read_file_from_zip(zip_path, train_quant_path)
    train_data_target_variable = read_file_from_zip(zip_path, train_target_path)
    
    print("Successfully loaded training data files from ZIP")
except Exception as e:
    print(f"Error loading training data from ZIP: {e}")

# Load test data
try:
    test_data_functional_connectome = read_file_from_zip(zip_path, test_func_path)
    test_data_categorical_metadata = read_file_from_zip(zip_path, test_cat_path)
    test_data_quantitative_metadata = read_file_from_zip(zip_path, test_quant_path)
    sample_submission = read_file_from_zip(zip_path, sample_sub_path)
    
    print("Successfully loaded test data files from ZIP")
except Exception as e:
    print(f"Error loading test data from ZIP: {e}")

# Merge the data
train_full_df = pd.merge(train_data_functional_connectome, train_data_categorical_metadata, on='participant_id')
train_full_df = pd.merge(train_full_df, train_data_quantitative_metadata, on='participant_id')
train_full_df = pd.merge(train_full_df, train_data_target_variable, on='participant_id')

test_full_df = pd.merge(test_data_functional_connectome, test_data_categorical_metadata, on='participant_id')
test_full_df = pd.merge(test_full_df, test_data_quantitative_metadata, on='participant_id')

print(f"Merged training data shape: {train_full_df.shape}")
print(f"Merged test data shape: {test_full_df.shape}")
```