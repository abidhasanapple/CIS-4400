# CIS-4400
# Project 2: Finding the best place to rent an apartment or buy a house based on the crime rates and the 311 calls directory. 

	1.	Link of all data sources 
 https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9
  
  2.	Link that shows the data dictionary (excel, google sheets)
 [my_311_data_V3.csv](https://github.com/abidhasanapple/CIS-4400/files/11940418/my_311_data_V3.csv)

3.	Scripts that gather these data
import pandas as pd
from sodapy import Socrata

data_url='data.cityofnewyork.us'    # The Host Name for the API endpoint (the https:// part will be added automatically)
data_set='erm2-nwe9'    # The data set at the API endpoint (311 data in this case)
app_token='XvMR7RBARLotSL2V6l3jiEDvC'   # The app token created in the prior steps
client = Socrata(data_url,app_token)      # Create the client to point to the API endpoint
# Set the timeout to 60 seconds    
client.timeout = 120
# Retrieve the first 2000 results returned as JSON object from the API
metadata = client.get_metadata(data_set)
[x['name'] for x in metadata['columns']]
['Unique Key', 'Created Date', 'Agency', 'Complaint Type', 'Descriptor', 
'Location Type', 'Incident Zip','Borough']
# The SoDaPy library converts this JSON object to a Python list of dictionaries
results = client.get(data_set, where="agency = 'NYPD'", limit=2000)
results = client.get(data_set, where="agency = 'NYPD'", 
                     select="Unique_Key, Created_Date, Agency, Complaint_Type, Descriptor, Location_Type, Incident_Zip, Borough", limit=2000)
# Convert the list of dictionaries to a Pandas data frame
df = pd.DataFrame.from_records(results)
# Save the data frame to a CSV file
df.to_csv("my_311_data_V3.csv")
  

