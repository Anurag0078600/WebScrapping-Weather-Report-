# WebScrapping-Weather-Report- I have used Python to do webscrapping of (https://mausam.imd.gov.in/) site to get weather report of major cities, and placed the data in excel file and created a graph represnting the weather report, using the table that i extracted from website.

Please find below code used to scrape the website.

# this imports requests(to download the website from web browser), bs4(to parse the html code in the python for analysis and extraction of important information).
 
import requests,bs4
# Pandas(to create dataframe from the collected information).
import pandas as pd 

# created on list element to collect all the data that I extract from the website.
Temp = [] 

# created a variable Url to store the URL that we need to scrape.
url = 'https://mausam.imd.gov.in/' 

# created a variable to get the HTML code from the website
res = requests.get(url) 

# to know the status of the download , some sites use protection to stop scraping so we can get that details here in the form of exception.
res.raise_for_status()

# here we parsed the html for analysis and extraction purpose.
soup = bs4.BeautifulSoup(res.text,'html.parser') 

# this command pretify the parsed html code so increase the readability
soup.prettify 

# here I created a variable to store all the information in the particular class.
anchors = soup.select('.current-wx') 


an = anchors[0].getText() # to get all the text from the obtained class.
print(an) # to check any specific pattern to fetch data acurately.
anu = an.split('\n')
print(anu)
# I obsereved a pattern and then created a code to obtain the data in correct form
for a in anu:

    if a !='':
    
        Temp.append(a)
        
    else:
    
        continue
        
actualTemp= []

for a in Temp:

    if a == 'Current Weather Across Major Cities' or a == 'Forecast':
    
        continue
        
    else:
    
        actualTemp.append(a)
        
print(actualTemp)   
# to check the lenght of the data extracted using the code.
print(len(actualTemp)) 

# i created 4 different list elements to store values of similar type.
States = []

temp = []

wind_state = []

Humidity = []

# created a variable to use it as index to get the data stored in specific list element.
i = 0 

# found that total obtained items in actualTemp are 32, so used below set of formula to get data to specific list element
while i < 32: 

    States.append(actualTemp[i])
    
    i = i + 1
    
    temp.append(actualTemp[i])
    
    i = i + 1
    
    wind_state.append(actualTemp[i])
    
    i = i + 1
    
    Humidity.append(actualTemp[i])
    
    i = i + 1
# Print all list elements.    
print(States)

print(temp)

print(wind_state)

print(Humidity)

# created a data set
WeatherReport = {
  'States': States,
  'Temp': temp,
  'Wind Speed' : wind_state,
'Humidity' : Humidity
}  

# created a dataframe
df = pd.DataFrame(WeatherReport) 

# checked the data frame.
print(df) 

# saved the data in excel form.
df.to_excel("output.xlsx") 
