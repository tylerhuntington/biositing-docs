# Biositing Tool API (v1)

The Biositing Tool API is an open API for programatically accessing 
data in our geospatial platform
based on REpresentational State Transfer (REST) principles. In a RESTful system,
information is organized into **resources**, each of which is uniquely
identified via a uniform resource identifier (URI). This page documents the 
first release (v1) of the Biositing API and provides examples of common use-cases.


<!--- START BLOCK COMMENT 
(While the Biositing API is designed to be code base agnostic and can conceivably be
used
with any programming language supporting basic http requests, a convenient
wrapper to the API has been implemented in the [Python Materials Genomics
(pymatgen)](http://pypi.python.org/pypi/pymatgen) library to facilitate
researchers in using the API. Please see the [#pymatgen
wrapper](#pymatgen-wrapper) section.
END BLOCK COMMENT --->

[//]: # (## Resources )

[//]: # (In the Biositing API, resources refer to specific types of data in the tool that )

[//]: # (can be accessed programatically with a URI endpoint. )

[//]: # (Currently)
[//]: # (supported information types &#40;v1 of the REST API&#41; include the following:)

[//]: # (**Ethanol Biorefineries** - Identifiers &#40;i.e. name, address, latitude, longitude, etc.&#41; )

[//]: # (and properties &#40;capacity, operational status&#41; of ethanol biorefineries in the continental U.S.)

[//]: # ()
[//]: # (**Renewable Diesel & SAF Plants** - Identifiers &#40;i.e. name, address, latitude, longitude, etc.&#41; )

[//]: # (and properties &#40;capacity, operational status&#41; of renewable diesel and SAF plants in the continental U.S.)

[//]: # ()
[//]: # (**...** )

[//]: # (**More coming soon!** )

## Authentication

SSL Encryption All requests to the Biositing API must be done over HTTPS.
Non-secure http requests are not allowed and will result in a HTTP 403
(Forbidden) response.


<!--- 
### API keys

To access the Biositing API, you will need your API key, except for certain free
queries that do not require a key. You can obtain your API key by logging into
the Biositing website, and going to 
[your profile page](https://biositing.jbei.org/accounts/change). 
Your API key and a button to
regenerate the key is provided at the bottom of the page.

Note that the API key effectively allows access to FTF data via
your account. You should therefore make all efforts to keep it secret and under
no circumstances should you share your API key with anyone. You will be held
responsible for any violations conducted using your API key. Should anyone else
require access to the Biositing API, they should register for an 
account on our [website](https://biositing.jbei.org/accounts/create) 
and generate their own API keys.

All Biositing API https requests must supply API key by one of the following methods:

* As an x-api-key header (recommended method), e.g., 
```
{‘X-API-KEY’: ‘YOUR_API_KEY’}
``` 
* As a GET request variable e.g. 
```
{REQUEST_URI}?API_KEY={YOUR_API_KEY}
```

The following is an example of a full url requesting for information of the
chemical substance with SMILES ID CCO with the API key 
supplied as a GET variable.

```
https://biositing.jbei.org/api/v1/chemicals/?smiles=CCO&API_KEY={YOUR_API_KEY}
```

--->

## Security

You agree not to use automated scripts to collect large fractions of the
database and disseminate them. You may collect large fractions of the database
for analysis and to present processed results with proper attribution. If you
plan to download large datasets, please email lead.jbei@gmail.com
with the email address associated with your account and with your use case so
that we can avoid flagging your account as abusing the service. We may also
suggest an efficient way for you to obtain the data you need. We reserve the
right to disable API access as a security precaution against bots.


## API Reference

### General URI Format

All URIs in the Biositing API are of the general form

`https://biositing.jbei.org/api/v1/{resource_type}[?{parameters}]`

1. The initial part of the URI (https://biositing.jbei.org/api/v1/) 
is a preamble, specifying a https REST request. 
The v1 denotes version 1 of the API,
to provide flexibility to support multiple versions of the API in the future. 

2. `{resource_type}` specifies the kind of information or 
operation being requested.

3. `{parameters}` specifies optional query parameters in the format `key=value`.
When specifying mulitple query parameters in a URIthey must be separated
by `&` characters e.g. `key1=value1&key2=value2&key3=value3`

<!--- START BLOCK COMMENT 
### General Response Format

All responses from the Biositing API are in the JavaScript Object Notation
(JSON). XML is not supported currently. The responses generally are of the
following form:

``` 
{
  "count": {NUMBER OF QUERY RESULTS},
  "next": {LINK TO NEXT 10 QUERY RESULTS}
  "previous": {LINK TO PREVIOUS 10 QUERY RESULTS},
  "results": [
    {FIRST 10 QUERY RESULTS}
   ]
}
```
Note that the results for a given request are paginated in groups 
of 10. If the size of the results set is less than 10, the `next` and `previous`
fields of the response object will be `null`. Otherwise, they will provide
valid links to the next and previous 'page' of results. 

END BLOCK COMMENT--->

## Resources 
In the Biositing API, resources refer to specific types of data in the tool that 
can be accessed programatically with a URI endpoint.
Currently
supported endpoints (v1 of the REST API) include the following:


### `ethanol_biorefineries`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/ethanol_biorefineries
```

Obtain dataset of ethanol biorefineries in the continental U.S. Attributes 
include name, address, latitude, longitude, capacity, operational status, etc.

### `renewable_diesel_saf_plants`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/renewable_diesel_saf_plants
```

Obtain dataset of renewable diesel and SAF plants in the continental U.S. 
Attributes include name, address, latitude, longitude, capacity, 
operational status, etc.

### `biosolids_facilities`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/biosolids_facilities
```

Obtain dataset of biosolids facilities
in the continental
U.S.

### `biodiesel_plants`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/biodiesel_plants
```

Obtain dataset of biodiesel plants
in the continental
U.S.

### `livestock_anaerobic_digesters`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/livestock_anaerobic_digesters
```

Obtain dataset of livestock anaerobic digesters
in the continental
U.S.

### `nass_manure`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/nass_manure
```

Obtain dataset of sub-county point source estimates of manure in the continental
U.S. downscaled from county-level data from the 2022 USDA NASS Census of 
Agriculture.

### `cafos`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/cafos
```

Obtain dataset of CAFO locations and production amounts for the continental U.S.

### `nass_ag_residues`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/nass_ag_residues
```

Obtain dataset of sub-county point source estimates of agricultural residues
in the continental
U.S. downscaled from county-level data from the 2022 USDA NASS Census of
Agriculture.



### `fats_oils_greases`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/fats_oils_greases
```

Obtain dataset of fats, oils, and greases estimates at the county level
for the continental
U.S.


### `rtr_geo_storage`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/rtr_geo_storage
```

Obtain dataset of geologic storage potential 
in the continental
U.S.
based on the Roads to Removal Report.

### `rcsp_saline_geo_storage`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/rcsp_saline_geo_storage
```

Obtain dataset of geologic storage potential
in saline formations across the continental
U.S.
based on data from the Regional Carbon Sequestration Partnership.

### `usgs_coal_geo_storage`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/usgs_coal_geo_storage
```

Obtain dataset of geologic storage potential
in coal formations across the continental
U.S.
based on data from the U.S. Geological Survey.


### `cejst_ej_indicators`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/cejst_ej_indicators
```

Obtain dataset of U.S. census tract level environmental 
justice indicators based on the Climate and Economic Justice Screening Tool.


### `grid_transmission_regions`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/grid_transmission_regions
```

Obtain dataset of simplified grid transmission regions based on 
the 2023 National Transmission Needs Study by U.S. Department of
                          Energy.


### `site_buffer`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/site_buffer?latitude={LATITUDE}&longitude={LONGITUDE}&radius={RADIUS}
```

Obtain an inventory of bioeconomy resources (e.g. biomass, municipal solid waste, plastic waste etc.) within a defined buffer radius of a point location specified by latitude and longitude coordinates.

#### Required Parameters
The following query parameters must be specified in the GET request URI.

`latitude` 
  
  -  Latitude coordinate of buffer zone centroid in in decimal degrees format 
  e.g. `39.44869283589919`

`longitude` 
  
  -  Longitude coordinate of buffer zone centroid in in decimal degrees format. 
  e.g. `-92.48419980931273`

`radius` 
  
  -  Radial distance (in kilometers) to define the buffer zone around the point
  specified by `latitude` and `longitude`

#### Optional Parameters
The following optional parameters may also be specified:

`summarize=true` 
  
  -  Whether to return sum totals resource types
  within the site buffer zone instead of returning individual point-level
  resource estimates. Note: this is a fixed parameter for which the only
  allowable value is `true`.
  

#### Example Usage
```
GET https://biositing.jbei.org/api/v1/site_buffer?latitude=39.44869283589919&longitude=-92.48419980931273&radius=20
```

### `state_data`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/state_data?state={STATE}&bt_scenario={BILLION_TON_SCENARIO}&resource_type={RESOURCE_TYPE}
```

Obtain an inventory of bioeconomy resources
within a state. Data is sourced from the <a href="https://www.energy.gov/eere/bioenergy/2023-billion-ton-report-assessment-us-renewable-carbon-resources" target="_blank">2023 Billion Ton Report</a> and downscaled 
to sub-county point-level estimates.

#### Required Parameters
The following query parameters must be specified in the GET request URI.

`state`

-  Two-letter abbreviation (capitalized) of the state for which data should be queried
   (e.g. `CA` for California, `OR` for Oregon, etc.). A full list of U.S. states and their two-letter abbreviations can be found <a href="https://www.faa.gov/air_traffic/publications/atpubs/cnt_html/appendix_a.html" target="_blank">here</a>.

`bt_scenario`

-  2023 Billion Ton Study Scenario for which data should be queried. Scenario definitions can be found <a href="https://www.energy.gov/sites/default/files/2024-03/beto-2023-billion-ton-report_0-exec-sum_0.pdf#page=4" target="_blank">here</a>. Must be one of the following:
    - `near-term`
    - `mature-market-low`
    - `mature-market-medium`
    - `mature-market-high`
    - `emerging`


`bt_subclass`

2023 Billion Ton Study resource subclass for which data should be queried. Must be one of the following:

- `AgProcessingWaste`
- `AgriculturalResidues`
- `EnergyCropsHerbaceous`
- `IntermediateOilseeds`
- `EnergyCropsWoody`
- `FireReductionThinnings`
- `ForestProcessingWaste`
- `LoggingResidues`
- `OtherForestWaste`
- `SmallDiameterTrees`
- `FOG`
- `Paper`
- `Plastic`
- `OtherSolidWaste`
- `OtherWetWaste`
- `GaseousResourcesLandfillGas`
- `GaseousResourcesCO2`
- `GaseousResourcesCO2HighConcentration`

[//]: # (Intermediate oilseeds)

[//]: # (    - `AgResidues`)

[//]: # (    - `EnergyCrops`)

[//]: # (    - `FoodWaste`)

[//]: # (    - `Manure`)

[//]: # (    - `MSW`)


[//]: # (#### Optional Parameters)

[//]: # (The following optional parameters may also be specified:)

[//]: # ()
[//]: # (`summarize=true`)

[//]: # ()
[//]: # (-  Whether to return sum totals resource types)

[//]: # (   within the site buffer zone instead of returning individual point-level)

[//]: # (   resource estimates. Note: this is a fixed parameter for which the only)

[//]: # (   allowable value is `true`.)


#### Example Usage
```
GET https://biositing.jbei.org/api/v1/state_data?state=CA&bt_scenario=near-term&bt_subclass=AgProcessingWaste
```


<!--- START BLOCK COMMENT 
#### Fields
The following fields may be specified as comma-separated values for the `fields`
query parameter to request a specific subset of information 
be included in the results set of a `chemicals` query:

`name` 
  
  - molecular name of a chemical substance

`smiles`
  
  * SMILES ID of a chemical substance

`formula`
  
  * Chemical formula of a chemical substance

`inchi`
  
  * InChI key of a chemical substance

`iupac`
  
  * IUPAC name of a chemical substance

`synonyms`
  
  * Common synonyms for the molecular name of a chemical substance

`bp_exp`
  
  * Experimentally measured boiling point of a chemical substance

`mp_exp`
  
  * Experimentally measured melting point of a chemical substance

`fp_exp`
  
  * Experimentally measured flash point of a chemical substance

`cn_exp`
  
  * Experimentally measured cetane number of a chemical substance

`dcn_exp`
  
  * Experimentally measured derived cetane number of a chemical substance

`ysi_exp`
  
  * Experimentally measured yield sooting index of a chemical substance

`bp_exp_srcs`
  
  * Source(s) of experimentally measured boiling point of a chemical substance

`mp_exp_srcs`
  
  * Source(s) of experimentally measured melting boiling point of a 
    chemical substance

`fp_exp_srcs`
  
  * Source(s) of experimentally measured flash point of a chemical substance

`cn_exp_srcs`
  
  * Source(s) of experimentally measured cetane number of a chemical substance

`dcn_exp_srcs`
  
  * Source(s) of experimentally measured derived cetane number of a chemical 
    substance

`ysi_exp_srcs`
  
  * Source(s) of experimentally measured yield sooting index of a chemical 
    substance
  
`bp_pred`
  
  * Predicted boiling point of a chemical substance

`mp_pred`
  
  * Predicted melting point of a chemical substance

`fp_pred`
  
  * Predicted flash point of a chemical substance

`cn_pred`
  
  * Predicted cetane number of a chemical substance

`dcn_pred`
  
  * Predicted derived cetane number of a chemical substance

`ysi_pred`
  
  * Predicted yield sooting index of a chemical substance

`bp_pred_perc_err`
  
  * Percent error of predicted boiling point of a chemical substance

`mp_pred_perc_err`
  
  * Percent error of predicted melting point of a chemical substance

`fp_pred_perc_err`
  
  * Percent error of predicted flash point of a chemical substance

`cn_pred_perc_err`
  
  * Percent error of predicted cetane number of a chemical substance

`dcn_pred_perc_err`
  
  * Percent error of predicted derived cetane number of a chemical substance

`ysi_pred_perc_err`
  
  * Percent error of predicted yield sooting index of a chemical substance

`bp_pred_abs_err`
  
  * Absolute error of predicted boiling point of a chemical substance

`mp_pred_abs_err`
  
  * Absolute error of predicted melting point of a chemical substance

`fp_pred_abs_err`
  
  * Absolute error of predicted flash point of a chemical substance

`cn_pred_abs_err`
  
  * Absolute error of predicted cetane number of a chemical substance

`dcn_pred_abs_err`
  
  * Absolute error of predicted derived cetane number of a chemical substance

`ysi_pred_abs_err`
  
  * Absolute error of predicted yield sooting index of a chemical substance
  
`bp_unit`
  
  * Scientific unit of experimentally measured and predicted boiling point 
    values of a chemical substance

`mp_unit`
  
  * Scientific unit of experimentally measured and predicted melting point 
    values of a chemical substance

`fp_unit`
  
  * Scientific unit of experimentally measured and predicted flash point 
    values of a chemical substance

`cn_unit`
  
  * Scientific unit of experimentally measured and predicted cetane number 
    values of a chemical substance

`dcn_unit`
  
  * Scientific unit of experimentally measured and predicted derived cetane
    number values of a chemical substance

`ysi_unit`
  
  * Scientific unit of experimentally measured and predicted derived yield
    sooting index values of a chemical substance
END BLOCK COMMENT --->
    
    
<!--- START BLOCK COMMENT 
END BLOCK COMMENT --->
    
