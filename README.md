# NBA
#Import Libraries
import time
import csv
import requests
from bs4 import BeautifulSoup

#Define Web Page and Save As Table Object
url = 'https://rotogrinders.com/projected-stats/nba?site=fanduel'
response = requests.get(url)
html = response.content
soup = BeautifulSoup(html, "lxml")

#Define Unique Designator to Find Desired Values
table = soup.find('table', attrs={'class': 'tbl data-table no-wrap'})

#Begin Loop
list_of_rows = []
for row in table.findAll('tr')[1:]:
    list_of_cells = []
    for cell in row.findAll('td'):
        text = cell.text.replace('\n','')
        text = text.strip()
        list_of_cells.append(text)
    list_of_rows.append(list_of_cells) 
           
#Time Stamp
timestr = time.strftime("%Y%m%d-%H%M%S")

#Open and Write To CSV File
outfile = open("C:\Anaconda2\david\output\FanDuel" + timestr + ".csv", "wb")
writer = csv.writer(outfile)
writer.writerow(["Name", "Pos", "Date", "Salary", "Projected", "Pts/1000" ])
writer.writerows(list_of_rows)

#Print to Console When Routine Finishes
print "FanDuel Data Exported"
