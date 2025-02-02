---
title: PortaBULL Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby

toc_footers:

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for PortaBULL
---

# Post Mating Data

> To Post mating data, the JSON must be formatted correctly, Please note that with "Linkit" at the beginning, this is not correctly formatted JSON:

```json
    "Linkit": [{
      "source_id",
      "object",
      "Herd_Size",
      "Matings_HIONo",
      "Matings_ABCode",
      "Matings_Date",
      "Herds_Name",
      "Cows_ID",
      "Added_DateTime",
      "ECode",
      "Matings_Status",
      "Matings_TechID",
      "Matings_ID",
      "Matings_Sync",
      "LoaderPresent"
    }]
```

This data structure can be joined together multiple times to build up the full list of mating records to POST to CRV.

### HTTP Request

`POST http://crvinsight.crvnz.co.nz:9889`

### Data Structure

Parameter | Description
--------- | -----------
source_id | Generated by PortaBULL to denote the SQLite database table.
object | Generated by PortaBULL and is used for the Unique Record ID.
Herd Size | Number of cows in the selected herd.
Matings_HIONo | Cow's herd recording number.
Matings_ABCode | Bull code of the bull used in insemination.
Matings_Date | Date of insemination.
Herds_Name | Herd's participant code.
Cows_ID | Cow's National ID.
Added_DateTime | DateTime stamp insemination record was created.
ECode | Enumerated Type for event code, 1 = conventional, 6 = sexed.
Matings_Status | Status of record, A = Approved, C/D = Confirmed, N/O = New, I = Ignore.
Matings_TechID | Identification number of Technician for insemination.
Matings_ID | Record Identifier with decimals stripped.
Matings_Sync | Insemination is part of a Synchro program.
LoaderPresent | Gun loader present.

<aside class="success">
Data has been recorded into CRV databases.
</aside>

# Get Technician List

```ruby
    Rho::AsyncHttp.get(
      :url => "#{$serverip}:3000/techs.json",
      :callback => (url_for :action => :tech_callback),
      :authentication => {
            :type => :basic, 
            :username =>"vn5BdmRurZP8uHfk", 
            :password => "JkBmVSTJGemPu9P9"
          },
      :callback_param => "" )
  
      render :action => :wait
  end
```

> The above command returns JSON structured like this:

```json
  {
      "tech": {
          "Techs_ID": "1446",
          "Techs_TechNo": "1446",
          "Techs_Init": "GOREPE"
      }
  }
```

This endpoint retrieves all CRV technicians.


### HTTP Request

`GET http://insight.crvnz.co.nz:3000/techs.json`

### URL Parameters

Parameter | Description
--------- | -----------
Basic Auth Username | vn5BdmRurZP8uHfk
Basic Auth Password | JkBmVSTJGemPu9P9

### Data Structure

Parameter | Description
--------- | -----------
Techs_ID | Unique Identifier
Techs_TechNo | Technician ID
Techs_Init |  Technicial Username

# Download Herds

```ruby
      Rho::AsyncHttp.get(
        :url => "#{$serverip}:3000/linkit_herds.json?TechID=#{$Tech_ID}",
        :callback => (url_for :action => :herd_callback),
        :authentication => {
             :type => :basic, 
             :username =>"vn5BdmRurZP8uHfk", 
             :password => "JkBmVSTJGemPu9P9"
           },
        :callback_param => "" 
      )
```

> The above command returns JSON structured like this:

```json
    {
        "linkit_herd": {
            "LinkHerds_ID": "10173099",
            "LinkHerds_Herd": "RFLV",
            "PBOrders_ParticCode": "RFLV",
            "LinkHerds_Owner": "OAKWOOD HILLS LTD (RFLV)",
            "LinkHerds_SPOwner": null,
            "LinkHerds_CIDR": "N",
            "LinkHerds_User": null,
            "LinkHerds_Imported": null,
            "LinkHerds_Tech": "5939",
            "PBOrders_TechAllocated": "5939",
            "PBOrders_TechAllocated2": "8347",
            "PBOrders_TechAllocated3": null,
            "LinkHerds_Finished": "Y",
            "LinkHerds_HerdSize": "510",
            "LinkHerds_Modified": null,
            "LinkHerds_ModifiedBy": null,
            "LinkHerds_HideFlag": "0",
            "LinkHerds_Locked": "0",
            "Herds_IsRecording": "N"
        }
    }
```

This endpoint retrieves all Herds for the Logged in technicians.


### HTTP Request

`GET http://insight.crvnz.co.nz:3000/linkit_herds.json?TechID=XXXX`

### URL Parameters

Parameter | Description
--------- | -----------
TechID | Technicians ID

### Data Structure

Parameter | Description
--------- | -----------
LinkHerds_ID | Unique Herd Identifier
LinkHerds_Herd | Herd Participant Code
PBOrders_ParticCode | Herd Participant code from PBOrders
LinkHerds_Owner | Herd Owner
LinkHerds_SPOwner | 
LinkHerds_CIDR | Is herd using CIDRs
LinkHerds_User |
LinkHerds_Imported | Imported DateTime to server
LinkHerds_Tech | Default Allocated Tech
LinkHerds_TechAllocated | First Allocated Tech
LinkHerds_TechAllocated2 | Second Allocated Tech
LinkHerds_TechAllicated3 | Third Allocated Tech
LinkHerds_Finished | Is the Herd finished for the season
LinkHerds_Size | Herd Size
LinkHerds_Modified | Modified date
LinkHerds_Hideflag | Hideflag
LinkHerds_Locked | Record Lock
LinkHerds_IsRecording | Is CRV Recording Herd

# Download Cows

```ruby
    result = Rho::AsyncHttp.get(
      :url => "#{$serverip}:3000/linkit_females.json?herd=#{herd.Herds_Name}",
      :authentication => {
            :type => :basic, 
            :username =>"vn5BdmRurZP8uHfk", 
            :password => "JkBmVSTJGemPu9P9"
          }
    )
```

> The above command returns JSON structured like this:
 
```json
    {
        "linkit_female": {
            "Herds_Name": "RFLV",
            "Cows_ID": "M21024807",
            "Cows_HIONo": "RFLV-20-1",
            "Cows_NLISID": "RFLV-20-1",
            "Cows_ElecID": "982 123747515257",
            "Cows_BreedS": "FFJX",
            "Cows_Birth": "2020-07-18",
            "Cows_LastServ": null,
            "Cows_LastSire": null,
            "Cows_CowStatus": "0",
            "Cows_SM1": null,
            "Cows_SM2": null,
            "Cows_SM3": null
        }
    }
```

This endpoint retrieves all Cows for the Selected Herd.


### HTTP Request

`GET http://insight.crvnz.co.nz:3000/linkit_females.json?herd=XXXX`

### URL Parameters

Parameter | Description
--------- | -----------
Herd | Herd Participant Code

### Data Structure

Parameter | Description
--------- | -----------
Herds_Name | Herd Participant Code
Cows_ID | National Cow ID
Cows_HIONo | Cows Herd Recording Number
Cows_NLISID | Cows Birth ID
Cows_ElecID | Electronic Tag ID
Cows_BreedS | Cows 4 character Breed String
Cows_Birth | Cows Birth Date
Cows_LastServ | Cows Last AI Service Date
Cows_LastSire | Cows Last AI Service Sire
Cows_CowStatus | Enum type for Animal Status (0 - Yet to Calve / Young Stock, 1 = Milking, 2 = Dry)
Cows_SM1 | Cows first SireMatch preference
Cows_SM2 | Cows Second SireMatch Preference
Cows_SM3 | Cows third SireMatch preference

# Download Bull Teams

```ruby
    result = Rho::AsyncHttp.get(
      :url => "#{$serverip}:3000/linkit_bteams.json?herd=#{herd.Herds_Name}",
      :authentication => {
            :type => :basic, 
            :username =>"vn5BdmRurZP8uHfk", 
            :password => "JkBmVSTJGemPu9P9"
          }
    )
```

> The above command returns JSON strucuted like this:

```json
    {
        "linkit_bteam": {
            "Herds_Name": "RFLV",
            "BTeams_ABCode": "719010",
            "BTeams_ShortName": "FB SG HfordPP",
            "BTeams_RegName": "FERTABULL SG HEREFORD PP",
            "BTeams_Year": "2021"
        }
    }
```

This enpoint retrieves all Bull Team Bulls for the Selected Herd.

### HTTP Request

`GET http://insight.crvnz.co.nz:3000/linkit_bteams.json?herd=XXXX`

### URL Parameters

Parameter | Description
--------- | -----------
Herd | Herd Participant Code

### Data Structure

Parameter | Description
--------- | -----------
Herds_Name | Herds Participant Code
BTeams_ABCode | Bull Code
BTeams_ShortName | Bulls Short / Common Name
BTeams_RegName | Bulls Registered Name
BTeams_Year | Year Bull was added to Bull Team

# Download Matings

```ruby
      result = Rho::AsyncHttp.get(
        :url => "#{$serverip}:3000/linkit_matings.json?herd=#{herd.Herds_Name}&tech=#{$Tech_ID}",
        :authentication => {
             :type => :basic, 
             :username =>"vn5BdmRurZP8uHfk", 
             :password => "JkBmVSTJGemPu9P9"
           }
      )
```

> The above command returns JSON strucuted like this:

```json
```

This endpoint retrieves all Mating data for Selected Herd and Tech

### HTTP Request

`GET http://insight.crvnz.co.nz:3000/linkit_matings.json?herd=XXXX&tech=XXXX`

### URL Parameters

Parameter | Description
--------- | -----------
Herd | Herd Participant Code
Tech | Technician ID

### Data Structure

Parameter | Description
--------- | -----------
Herds_Name | Herds Participant Code
BTeams_ABCode | Bull Code
BTeams_ShortName | Bulls Short / Common Name
BTeams_RegName | Bulls Registered Name
BTeams_Year | Year Bull was added to Bull Team
