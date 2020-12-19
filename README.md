# World_Health_Report_Analysis
#Exploratory Data Analysis of 2020 World Health Report


# Module Importing
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats


#Reading data files
use_columns = [0, 1, 2, 6, 7, 8, 9, 10, 11, 12] #column selection
whr = pd.read_csv("World_Happiness_Report_2020.csv", usecols = use_columns) #read file with selected columns
whr = pd.DataFrame(whr[:len(whr)]) #reformat as data file
whr.info() #information about each column / variable including types
whr.head() #first few rows
whr.columns #columns


#Renaming Columns
whr.columns = whr.columns.str.replace("Country name", "Country")
whr.columns = whr.columns.str.replace("Regional indicator", "Region")
whr.columns = whr.columns.str.replace("Ladder score", "Happiness")
whr.columns = whr.columns.str.replace("Logged GDP per capita", "GDP per capita")
whr.columns = whr.columns.str.replace("Freedom to make life choices", "Freedom of life choices")
whr.columns = whr.columns.str.replace("Perceptions of corruption", "Corruption perceptions")

print(whr.columns)


##Correlation/Scatter Plots of Factors vs Happiness Score
#Create figure
fig1 = plt.figure(figsize=(16,7))
plt.suptitle('2020 Correlation of Happiness Score and Contributing Factors', fontsize=14, fontweight='bold', fontstyle='italic')

#Figure for loop
c = 1 #initial plot counter
for col in whr.loc[:, 'GDP per capita':'Corruption perceptions']:
    plt.subplot(2, 3, c)
    plt.title('{} vs Happiness Score'.format(col))
    plt.xlabel(col)
    def r2(a, b):
        return stats.pearsonr(a=whr['Happiness'], b=whr[col])
    sns.regplot(x=whr[col], y=whr['Happiness'], data=whr, ci=95, label=True)
    c = c + 1

#Show figure
fig1.tight_layout()
fig1.show()
fig1.savefig('2020 Correlation of Happiness Score and Contributing Factors.png')


##Correlation Heatmap
fig2 = plt.figure(figsize=(16,7))
heat_map = sns.heatmap(whr.corr(), cmap='BuPu', annot=True, fmt='0.2f')
sns.set(font_scale = 0.75)
heat_map.set_xticklabels(heat_map.get_xticklabels(), rotation = 10)
heat_map.set_title('2020 Happiness Factors Correlation Heatmap', fontsize=14, fontweight='bold', fontstyle='italic')
fig2.tight_layout()
fig2.show()
fig2.savefig('2020 Happiness Factors Correlation Heatmap.png')


##Show Happiest Countries
whr_top10 = whr[:10].sort_values('Happiness', ascending=False)
whr_top10.info()
fig3 = plt.figure(figsize=(16,7))
sns.set(font_scale = 1.25)
sns.barplot(y=whr_top10['Happiness'], x=whr_top10['Country'], data=whr_top10, palette='hls')
plt.ylim(7, 8)
fig3.suptitle('2020 Top 10 Happiest Countries', fontsize=18, fontweight='bold', fontstyle='italic')
fig3.tight_layout()
fig3.show()
fig3.savefig('2020 Top 10 Happiest Countries.png')


#Grouped by Region
fig4 = plt.figure(figsize=(16,7))
boxplot = sns.boxplot(y=whr['Happiness'], x=whr['Region'], hue=whr['Region'],palette='hls', width=3)
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0, fontsize= 'x-small', title='Regions')
boxplot.axes.xaxis.set_visible(False)
fig4.suptitle('2020 Happiness Score Distribution per Region', fontsize=14, fontweight='bold', fontstyle='italic')
fig4.tight_layout()
fig4.show()
fig4.savefig('2020 Happiness Score Distribution per Region.png')


##Read other year's data
usecols18_19 = [1,2,3,5]
whr19 = pd.read_csv("World_Happiness_Report_2019.csv", usecols = usecols18_19)
whr18 = pd.read_csv("World_Happiness_Report_2018.csv", usecols = usecols18_19)
usecols17 = [0,2,5,7]
whr17 = pd.read_csv("World_Happiness_Report_2017.csv", usecols = usecols17)
usecols16 = [0,3,6,8]
whr16 = pd.read_csv("World_Happiness_Report_2016.csv", usecols = usecols16)
usecols15 = [0,3,5,7]
whr15 = pd.read_csv("World_Happiness_Report_2015.csv", usecols = usecols15)


#2019 Cleanup
whr19.columns = whr19.columns.str.replace("Country or region", "Country")
whr19.columns = whr19.columns.str.replace("Score", "Happiness")
whr19.info()
print("World Health Report 2019 Columns")
print(whr19.columns)


#2018 Cleanup
whr18.columns = whr18.columns.str.replace("Country or region", "Country")
whr18.columns = whr18.columns.str.replace("Score", "Happiness")
whr18.info()
print("World Health Report 2018 Columns")
print(whr18.columns)


#2017 Cleanup
whr17.columns = whr17.columns.str.replace("Happiness.Score", "Happiness")
whr17.columns = whr17.columns.str.replace("Economy..GDP.per.Capita.", "GDP per capita")
whr17.columns = whr17.columns.str.replace("Health..Life.Expectancy.", "Healthy life expectancy")
whr17.info()
print("World Health Report 2017 Columns")
print(whr17.columns)

#2016 Cleanup
whr16.columns = whr16.columns.str.replace("Happiness Score", "Happiness")
whr16.columns = whr16.columns.str.replace("(", "")
whr16.columns = whr16.columns.str.replace(")", "")
whr16.columns = whr16.columns.str.replace("Economy GDP per Capita", "GDP per capita")
whr16.columns = whr16.columns.str.replace("Health Life Expectancy", "Healthy life expectancy")
whr16.info()
print("World Health Report 2016 Columns")
print(whr16.columns)

#2015 Cleanup
whr15.columns = whr15.columns.str.replace("Happiness Score", "Happiness")
whr15.columns = whr15.columns.str.replace("(", "")
whr15.columns = whr15.columns.str.replace(")", "")
whr15.columns = whr15.columns.str.replace("Economy GDP per Capita", "GDP per capita")
whr15.columns = whr15.columns.str.replace("Health Life Expectancy", "Healthy life expectancy")
whr15.info()
print("World Health Report 2015 Columns")
print(whr15.columns)

#2020 Cleanup
whr20 = whr.drop(columns=['Region', 'Social support', 'Freedom of life choices', 'Generosity', 'Corruption perceptions', 'Happiness in Dystopia'])
print("World Health Report 2020 Columns")
print(whr20.columns)


#Add year to datasets
whr20['Year'] = 2020
whr19['Year'] = 2019
whr18['Year'] = 2018
whr17['Year'] = 2017
whr16['Year'] = 2016
whr15['Year'] = 2015

##Create new dataframe and add each year to it
whr_overall = pd.DataFrame(columns = ['Country', 'Happiness', 'GDP per capita', 'Healthy life expectancy', 'Year'])

years = [whr15, whr16, whr17, whr18, whr19, whr20]
for y in years:
    whr_overall = whr_overall.append(y[['Country', 'Happiness', 'GDP per capita', 'Healthy life expectancy', 'Year']], ignore_index=True)


##Filter for 2020 Top 10 Countries
top10_2020 = whr_top10['Country']
whr_overall_top10 = whr_overall[whr_overall['Country'].isin(top10_2020)].sort_values('Happiness', ascending=False)
whr_overall_top10.info()


##Show Happiest Countries Grouped by Year
fig5 = plt.figure(figsize=(16,7))
sns.barplot(x=whr_overall_top10['Country'], y=whr_overall_top10['Happiness'], hue=whr_overall_top10['Year'], data=whr_overall_top10, palette='hls')
plt.ylim(7, 8)
fig5.suptitle('Top 10 Happiest Countries from 2020 over the Last 5 Years', fontsize=18, fontweight='bold', fontstyle='italic')
fig5.tight_layout()
fig5.show()
fig5.savefig('Top 10 Happiest Countries from 2020 over the Last 5 Years.png')


##Show Happiest Countries as Averages
fig6 = plt.figure(figsize=(16,7))
boxplot = sns.boxplot(x=whr_overall_top10['Country'], y=whr_overall_top10['Happiness'], palette='hls')
fig6.suptitle('Happiness Score Distribution from 2015-2020', fontsize=14, fontweight='bold', fontstyle='italic')
fig6.tight_layout()
fig6.show()
fig6.savefig('Overall Happiness Score Distribution per Country.png')
