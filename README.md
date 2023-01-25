# ADS2_Resubmission
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Read the data
filename = "D:\Datasets\world_bank_data.csv"
dataframe = pd.read_csv("D:\Datasets\world_bank_data.csv")


def ingest_manipulate(filename):
    """This function takes a filename as an argument, reads the file using the pandas library and manipulates the data accordingly.
    It returns two manipulated dataframes, one with the countries as columns and the other with the years as columns."""

    dataframe = pd.read_csv(filename, index_col=["Country Name"])
    country_as_colums = dataframe.transpose()
    years_as_columns = dataframe.drop(
        ["Indicator Name", "Country Code", "Indicator Code"], axis=1)
    return country_as_colums, years_as_columns
