# Feedstock to Function API (v1)

The Feedstock to Function API (FTF-API) is an open API for accessing 
our chemical properties data
based on REpresentational State Transfer (REST) principles. In a RESTful system,
information is organized into **resources**, each of which is uniquely
identified via a uniform resource identifier (URI). This page documents the 
first release (v1) of the FTF-API and provides examples of common use-cases.


<!--- 
(While the FTF-API is designed to be code base agnostic and can conceivably be
used
with any programming language supporting basic http requests, a convenient
wrapper to MAPI has been implemented in the [Python Materials Genomics
(pymatgen)](http://pypi.python.org/pypi/pymatgen) library to facilitate
researchers in using the MAPI. Please see the [#pymatgen
wrapper](#pymatgen-wrapper) section.
--->

## Resources 
In the Feedstock to Function Tool, resources are generally packages of 
information about a chemical substance or blend of substances. Currently
supported information types (v1 of the REST API) include the following:

**Chemical** - Identification attributes (i.e. SMILES, InChIKey, IUPAC, etc.) 
and properties (experimental and/or predicted) of a chemical
substance.

**Blend** - Identification attributes
and properties (experimental and/or predicted) of a chemical blend.

## Authentication

SSL Encryption All requests to the FTF-API must be done over HTTPS.
Non-secure http requests are not allowed and will result in a HTTP 403
(Forbidden) response.


### API keys

To access the FTF-API, you will need your API key, except for certain free
queries that do not require a key. You can obtain your API key by logging into
the Feedstock to Function website, and going to 
[your profile page](https://feedstock-to-function.lbl.gov/accounts/change). 
Your API key and a button to
regenerate the key is provided at the bottom of the page.

Note that the API key effectively allows access to FTF data via
your account. You should therefore make all efforts to keep it secret and under
no circumstances should you share your API key with anyone. You will be held
responsible for any violations conducted using your API key. Should anyone else
require access to the FTF-API, they should register for an 
account on our [website](https://feedstock-to-function.lbl.gov/accounts/create) 
and generate their own API keys.

All FTF-API https requests must supply API key by one of the following methods:

* As an x-api-key header (recommended method), e.g., 
```
{‘X-API-KEY’: ‘YOUR_API_KEY’}
``` 
* As a GET request variable e.g. 
```
{REQUEST_URI}?API_KEY={YOUR_API_KEY}
```

The following is an example of a full url requesting for information of the
material with SMILES ID CCO with the API key supplied as a GET variable.

```
https://feedstock-to-function.lbl.gov/api/v2/chemicals/?smiles=CCO&API_KEY={YOUR_API_KEY}
```

## Security

You agree not to use automated scripts to collect large fractions of the
database and disseminate them. You may collect large fractions of the database
for analysis and to present processed results with proper attribution. If you
plan to download large datasets, please email feedstocktofunction@gmail.com
with the email address associated with your account and with your use case so
that we can avoid flagging your account as abusing the service. We may also
suggest an efficient way for you to obtain the data you need. We reserve the
right to disable API keys as a security precaution against bots.


## API Reference

### General URI Format

All URIs in the FTF-API are of the general form

`https://feedstock-to-function.lbl.gov/api/v1/{resource_type}[/{parameters}]`

1. The initial part of the URI (https://feedstock-to-function.lbl.gov/api/v2/) is a
preamble, specifying a https REST request. The v2 denotes version 2 of the MAPI,
to provide flexibility to support multiple versions of the API in future. 2.
{request_type} specifies the kind of information or operation being requested.
Currently supported request types include "materials", "battery", "reaction",
"mpquery" and "api_check". 3. {identifier} is an identifier for the specific
information requested. Depending on the {request_type}, an identifier may or may
not be necessary. 4. {parameters} - Some requests require additional parameters
to be provided.

### General Response Format

All responses from the FTF-API are in the JavaScript Object Notation
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
GET https://feedstock-to-function.lbl.gov/api/v1/chemicals/?{IDENTIFIER}={VALUE}[&fields={FIELD_1,FIELD_2,FIELD_3,...}]
```

Obtain chemical substance data based on an identifier. The response is always a
list of associative arrays, i.e., `[ {key:value, ... }, {... }, ...]`. Valid
identifiers include `name`, `smiles`, `iupac`, `inchi`, `cas` and `inchi`.
For example, the following query will return the chemical substance 
with the SMILES ID CCO (ethanol):
```
GET https://feedstock-to-function.lbl.gov/api/v1/chemicals/?smiles=CCO
```
Note that since no fields were specified in the optional `fields` portion of the
query string, this request will yield a response that include all attributes
of the chemicals in the result set. If, for example we were only interested
in retrieving the chemical formula, IUPAC name and InChI key for ethanol, we 
could specify these fields of interest in the query as such:

```
GET https://feedstock-to-function.lbl.gov/api/v1/chemicals/?smiles=CCO&fields=formula,iupac,inchi
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
GET https://feedstock-to-function.lbl.gov/api/v1/blends/?{IDENTIFIER}={VALUE}[&fields={FIELD_1,FIELD_2,FIELD_3,...}]
```
    
