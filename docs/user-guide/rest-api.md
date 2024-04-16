# Biositing Tool API (v1)

tyler The Biositing Tool API is an open API for accessing 
core data and in our tool
based on REpresentational State Transfer (REST) principles. In a RESTful system,
information is organized into **resources**, each of which is uniquely
identified via a uniform resource identifier (URI). This page documents the 
first release (v1) of the Biositing API and provides examples of common use-cases.


<!--- 
(While the Biositing API is designed to be code base agnostic and can conceivably be
used
with any programming language supporting basic http requests, a convenient
wrapper to the API has been implemented in the [Python Materials Genomics
(pymatgen)](http://pypi.python.org/pypi/pymatgen) library to facilitate
researchers in using the API. Please see the [#pymatgen
wrapper](#pymatgen-wrapper) section.
--->

## Resources 
In the Biositing Tool, resources are generally packages of 
information about a chemical substance or blend of substances. Currently
supported information types (v1 of the REST API) include the following:

**Ethanol Biorefineries** - Identifiers (i.e. name, address, latitude, longitude, etc.) 
and properties (capacity, operational status) of ethanol biorefineries in the continental U.S.

**Renewable Diesel Plants** - Identifiers (i.e. name, address, latitude, longitude, etc.) 
and properties (capacity, operational status) of renewable diesel plants in the continental U.S.

**Sustainable Aviation Fuel (SAF) Plants** - Identifiers (i.e. name, address, latitude, longitude, etc.) 
and properties (capacity, operational status) of SAF plants in the continental U.S.

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
plan to download large datasets, please email feedstocktofunction@gmail.com
with the email address associated with your account and with your use case so
that we can avoid flagging your account as abusing the service. We may also
suggest an efficient way for you to obtain the data you need. We reserve the
right to disable API access as a security precaution against bots.


## API Reference

### General URI Format

All URIs in the Biositing API are of the general form

`https://biositing.jbei.org/api/v1/{resource_type}[/{parameters}]`

1. The initial part of the URI (https://biositing.jbei.org/api/v1/) 
is a preamble, specifying a https REST request. 
The v1 denotes version 1 of the API,
to provide flexibility to support multiple versions of the API in future. 


2. `{resource_type}` specifies the kind of information or 
operation being requested.
Currently supported request types include `chemicals` and `blends`.


3. `{parameters}` specifies optional query parameters (details below).

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

## Resources

### `chemicals`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/chemicals/?{IDENTIFIER}={VALUE}[&fields={FIELD_1,FIELD_2,FIELD_3,...}]
```

Obtain chemical substance data based on an identifier. The response is always a
list of associative arrays, i.e., `[ {key:value, ... }, {... }, ...]`. Valid
identifiers include `name`, `smiles`, `iupac`, `inchi`, `cas` and `inchi`.
For example, the following query will return the chemical substance 
with the SMILES ID CCO (ethanol):
```
GET https://biositing.jbei.org/api/v1/chemicals/?smiles=CCO
```
Note that since no fields were specified in the optional `fields` portion of the
query string, this request will yield a response that include all attributes
of the chemicals in the result set. If, for example we were only interested
in retrieving the chemical formula, IUPAC name and InChI key for ethanol, we 
could specify these fields of interest in the query as such:

```
GET https://biositing.jbei.org/api/v1/chemicals/?smiles=CCO&fields=formula,iupac,inchi
```

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
    
    
### `blends`

#### Request Template
```
GET https://biositing.jbei.org/api/v1/blends/?{IDENTIFIER}={VALUE}[&fields={FIELD_1,FIELD_2,FIELD_3,...}]
```
    
