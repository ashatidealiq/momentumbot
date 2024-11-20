# MomentumBot

## Description

This demo project uses a Keras model to predict prices for 3 assets (Gold, NASDAQ Composite Index and Bitcoin) in different asset classes. 

The project also uses the LSTM algorithm to make predictions on interest rates, gold, S&P 500, and BTC. There's a basic GUI where the user can input their investment today and get a 3-6m PNL prediction adjusted for inflation.

Note: This is a toy project to demo scalable machine learning and deep learning. I wouldn't use it to risk real money and nor should you. Fair warning? 

## Uses

**yfinance**: Crawls Yahoo Finance data to generate data sets

**LSTM**: Train and predict on time series data

**Hopsworks**: Manage and store features and model registry. Hopsworks is slower to work with than a GCP/AWS/Azure setup but it's free. You can work with it via Jupyter notebooks and it has some nice features for setting up feature groups and sharing across pipelines etc. It's not Azure Data Factory or Azure ML Studio but I like it. It will save you clicks and $ over Azure.

**Modal**: Periodically execute the program to update remote features and models

**Gradio**: Display prediction results

**Huggingface**: Deploy Gradio apps online

## Structure
![structure](README.assets/structure-1705018458168.png)

First, we use yfinance to implement a tool for crawling Yahoo Finance historical data. We create DataFrames from historical data and create feature groups on Hopsworks through ***backfill-feature-group***.

Second, we retrieve historical features from Hopsworks and train the first-generation models using LSTM and upload the models to Hopsworks file system through ***training-pipeline***. Below is the example for gold. As you can see predictions from deep learning can be surprisingly accurate.

![output](README.assets/output.png)

Then we deploy ***feature-pipeline-daily*** on Modal to update the latest data to Hopsworks every working day. If there is a sufficient amount of new daily data it is necessary to retrain the model to avoid concept drift. That's why we have ***cyclical-training-pipeline*** to generate and update new models every month.

Finally, infer and display prediction results with a Gradio app deployed on Hugging Face.

## How to Run

1. Clone this repo
2. Configure the Canda virtual environment and install requirements
3. Create accounts on hopsworks.ai and modal.com, create/cofigure necessary API keys and tokens
4. Run all *backfill-feature-group.ipynb*, *training-pipeline.ipynb*, *feature-pipeline-daily.py* and *cyclical-training-pipeline*.py in order
5. Run *investment_ui.py* in folder Gradio to see inference output and some cool scenario analysis
