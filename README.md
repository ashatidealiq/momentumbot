# MomentumBot

## Description

This demo project uses a simple Keras model to predict prices for 3 separate asset classes (Gold, NASDAQ Composite Index, and Bitcoin). 

Note: The project primarily demonstrates scalable machine learning and deep learning. I really wouldn't use it to trade. 

The project also uses the LSTM algorithm to make predictions interest rates, gold, S&P 500, and BTC. The user can input their investment today and get the prediction about their profit or loss in 3 or 6 months (after the inflation index).

## Uses

**yfinance**: Crawl Yahoo Finance data to generate data sets

**LSTM**: Train and predict on time series data

**Hopsworks**: Manage and store features and model registry

**Modal**: Periodically execute the program to update remote features and models

**Gradio**: Display prediction results

**Huggingface**: Deploy Gradio apps online

## Structure
![structure](README.assets/structure-1705018458168.png)

First, we use yfinance to implement a tool for crawling Yahoo Finance historical data. We create DataFrames from historical data and create feature groups on Hopsworks through ***backfill-feature-group***.

Second, we retrieve historical features from Hopsworks and train the first-generation models using LSTM and upload the models to Hopsworks file system through ***training-pipeline***. Here is the gold example of our model, this picture shows that the predictions is relatively accurate.

![output](README.assets/output.png)

Then we deploy ***feature-pipeline-daily*** on Modal to update the latest data to Hopsworks every working day. We also realize that, if there is a sufficient amount of new daily data, to avoid Concept Drift, it is necessary to retrain the model. That's why we have ***cyclical-training-pipeline*** to generate and update new models every month.

Finally, infer and display prediction results by using Gradio app deployed on Hugging Face.

## How to Run

1. Clone this repo
2. Configure the Canda virtual environment and install requirements
3. Create accounts on hopsworks.ai and modal.com, create/cofigure necessary API keys and tokens
4. Run all *backfill-feature-group.ipynb*, *training-pipeline.ipynb*, *feature-pipeline-daily.py* and *cyclical-training-pipeline*.py in order
5. Run *investment_ui.py* in folder Gradio to see what will happen =）
