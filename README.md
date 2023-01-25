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
country_as_colums, years_as_columns = ingest_manipulate(filename)

print(country_as_colums, years_as_columns)

# Select the columns
data_selection = dataframe[['Country Name', 'Country Code', 'Indicator Name',
                            'Indicator Code', '2013', '2014', '2015', '2016', '2017', '2018', "2019"]]

# Select the countries
data_selection = data_selection[data_selection['Country Name'].isin(
    ["United Kingdom", 'Canada', "Zimbabwe", "Germany", "Australia", "China", "India"])].set_index("Country Name")

# Select the indicators
data_selection = data_selection[data_selection['Indicator Name'].isin(
    ["Foreign direct investment, net inflows (% of GDP)", 'Urban population growth (annual %)', "Population growth (annual %)"])]
def corr_lack(country):
    """This function plots a heatmap of the correlation between the selected indicators for a given country."""

    cor_indi_names = ["Population growth (annual %)", "Urban population growth (annual %)", "Foreign direct investment, net inflows (% of GDP)",
                      "CO2 emissions (kt)", "Mortality rate, under-5 (per 1,000 live births)", "Access to electricity (% of population)", "Agricultural land (sq. km)"]
    corr_years = ['2013', '2014', '2015', '2016', '2017', '2018', '2019']

    selected_df = dataframe[dataframe["Country Name"] == country].set_index("Indicator Name")

    filtered_data = selected_df.loc[cor_indi_names, corr_years].transpose()

    # Heatmap plot
    plt.title(country, fontsize=14)
    sns.heatmap(filtered_data.corr(), linecolor='grey',
                linewidths=0.1, annot=True)
    plt.show()


corr_lack("Brazil")

corr_lack("United Arab Emirates")


fdi_data = data_selection[data_selection['Indicator Name']
                          == "Foreign direct investment, net inflows (% of GDP)"]

urban_data = data_selection[data_selection['Indicator Name']
                            == "Urban population growth (annual %)"]

# plot the fdi data
fdi_data.plot(kind="bar", rot=0,
              title="Foreign direct investment, net inflows (% of GDP)", ylabel="Percentage (%)")
plt.show()

# plot the urban population growth
urban_data.plot(kind="bar", rot=0,
                title="Urban population growth (annual %)", ylabel="Percentage (%)")

plt.show()

# mean of the urban population growth
urban_data.mean().plot(kind="bar", title="Mean of the Urban Population Growth", rot=0)
plt.show()
