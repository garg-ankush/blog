---
title: "ML-clerk, a simple way to store modeling results for data scientists"
date: 2/9/23
author: Ankush Garg
---
Python package here: [ml-clerk](https://pypi.org/project/ml-clerk/)

**What is ml-clerk?**

ML-clerk, at its core, is a lightweight logging library that helps you keep track of performance, model parameters, timing (or whatever) throughout your data science experiment.

**I'm a data scientist. Why should I care?**

As a data scientist you're constantly running new experiments. As you run new experiments, you are recording your results (if not, you should be recording your results). ML clerk does exactly that. It reduces the inertia by automatically recording the results of your experiment, so you don't have to worry about keeping track.

**Ok now that I'm convinced, where does ml-clerk log stuff?**

ML clerk has two places where it tracks things - excel or google sheets. Excel setup is easier, so start with that. For google sheets usage, see the link below.

#### Training a model - demo purposes only

```python 
# Imports

from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression

dataset = load_iris()

# Define data
X = dataset['data']
y = dataset['target']

# Train the model
logistic_regression_classifier = LogisticRegression()
logistic_regression_classifier.fit(X, y)

# Make predictions
predictions = logistic_regression_classifier.predict(X)
probabilities = logistic_regression_classifier.predict_proba(X)
```

### Excel usage
```python 
#!/usr/bin/python
from ml_clerk import Clerk

# Set up the clerk with excel mode
clerk = Clerk(excel_mode=True)

# file_path refers to the excel workbook you want to record in, and the sheet name referes to the sheet
FILE_PATH = 'hello_world.xlsx'
SHEET_NAME = 'Sheet1'

clerk.set_up(filepath=FILE_PATH, sheet_name=SHEET_NAME)

### Record all the artifacts in one go
clerk.record(predictions=predictions, probabilities=probabilities, model_parameters=logistic_regression_classifier.get_params())
```

### Google sheets usage
```python
from ml_clerk import Clerk

# Set up the clerk with google sheets mode

clerk = Clerk(google_sheets_mode=True)

# file_path refers to the excel workbook you want to record in, and the sheet name refers to the sheet
GOOGLE_SHEETS_FILE_PATH = '#your google sheets url'
SHEET_NAME = '#your sheet name'

clerk.set_up(file_path=GOOGLE_SHEETS_FILE_PATH, sheet_name=SHEET_NAME)

# Record all the artifacts in one go
clerk.record(
			predictions=predictions,
		  probabilities=probabilities, 
			model_parameters=logistic_regression_classifier.get_params()
)
```

**Google sheets permissions**

1. Follow this link for how to permission your Google account - [py-gsheets](https://erikrood.com/Posts/py_gsheets.html).

2. Make sure you enable the Google sheets API under the project in Google console.

3. Update the `.env` with the path to token.json credentials file.


