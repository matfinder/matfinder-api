# matfinder-doc

<img src="https://github.com/matfinder/matfinder-doc/blob/master/img/logo_03.jpeg" alt="Drawing" height="30"/> matfinder-api v1.0.0

matfinder is a web database of materials properties with the basic, but fundamental, role of making materials data more accessible to everyone. For that reason our strategy is to create and mainatain a web API where people can insert and fetch materials information in a safe and reliable way. Hence, any project wishing to use our API is more than welcome.

Our API is hosted at <a href='https://www.matfinder.io/api/'>https://www.matfinder.io/api/</a>.

Look our <a href='https://www.youtube.com/watch?v=cN3O46Owjk8'>video</a>!

## Table of contents

1. [Authentication routes](#authentication)
    1. [POST /api/signup](#signup)
    2. [POST /api/activate/<activation_key>](#activate)
    3. [POST /api/login](#login)
    4. [POST /api/forgot](#forgot)
    5. [POST /api/reset/<modification_key>](#reset)
    6. [POST /api/logout](#logout)
    7. [POST /api/refresh](#refresh)
    8. [GET /api/user](#getuser)
    9. [PUT /api/user](#putuser)
    10. [GET /api/pendinginsertions](#pendinginsertions)

2. [Group routes](#group)
    1. [GET /api/groups](#groupsall)
    2. [GET /api/group/<group_name>](#groupone)

3. [Subgroup routes](#subgroup)
    1. [GET /api/subgroups](#subgroupsall)
    2. [GET /api/subgroup/<subgroup_id>](#subgroupone)
    3. [GET /api/subgroups/group/<group_id>](#subgroupgroup)
    4. [POST /api/subgroup/create](#subgroupcreate)

4. [Materials routes](#material)
    1. [GET /api/materials](#materialsall)
    2. [GET /api/material/<material_id>](#materialone)
    3. [GET /api/materials/group/<group_id>](#materialsbygroup)
    4. [GET /api/materials/subgroup/<subgroup_id>](#materialsbysubgroup)
    5. [POST /api/material/create](#materialcreate)

5. [Documents routes](#document)
    1. [GET /api/documents](#documentsall)
    2. [GET /api/document/<document_name>](#documentone)

6. [References routes](#reference)
    1. [GET /api/refs](#referencesall)
    2. [GET /api/ref/<ref_id>](#referenceone)
    3. [POST /api/ref/create](#referencecreate)

7. [Point properties routes](#pointproperty)
    1. [GET /api/pointproperties](#pointpropertiesall)
    2. [GET /api/pointproperty/<property_id>](#pointpropertyone)
    3. [POST /api/pointproperty/create](#pointpropertycreate)
    4. [POST /api/pointproperties/material/<material_id>](#poinpropertiesmat)
    5. [POST /api/pointproperties/dependences](#pointpropertiesdep)
    6. [POST /api/pointproperty/<mat_id>/<prop_id>/dependence_values](#pointpropertydepvals)

8. [Single properties routes](#singleproperty)
    1. [GET /api/singleproperties](#singlepropertiesall)
    2. [GET /api/singleproperty/<singleproperty_id>](#singlepropertyone)
    3. [GET /api/singleproperties/material/<material_id>](#singlepropertiesmat)
    4. [POST /api/singleproperty/create](#singlepropertycreate)

9. [Points routes](#point)
    1. [POST /api/points/<material_id>/<pointproperty_name>](#pointsmatprop)
    2. [POST /api/point/create](#pointcreate)
    3. [POST /api/points/create](#pointscreate)

10. [Singles routes](#single)
    1. [GET /api/single/<single_id>](#singleone)
    2. [POST /api/single/create](#singlecreate)

11. [Companies routes](#company)
    1. [GET /api/companies](#companiesall)
    2. [GET /api/company/<company_id>'](#companyone)

12. [Products routes](#product)
    1. [GET /api/products](#productsall)
    2. [GET /api/product/<product_id>](#productone)
    3. [POST /api/single/create](#productcreate)

13. [Search, messages and analytics routes](#messages)
    1. [POST /api/search/advanced](#search)
    2. [POST /api/message](#messagesend)
    3. [POST /api/redirect/<product_id>](#redirect)



---



Authentication routes: <a name="authentication"></a>
-

The matFinder platform allows to create accounts that are required for inserting data. This data is then validated by administrators before appearing in the website, i.e. the administrator is responsible to toggle the status of the inserted data to True.



`POST`  **/api/signup** - *user registration* <a name="signup"></a>
-

If successful, an email is sent to the user to activate the account.
```javascript
// Request Body Example
{
    "username": "user_1",
    "name": "User One",
    "email": "user_1@example.example",
    "password": "********",
    "country": "Portugal",
    "organization": "example"
}

// Response Example
{
    "user": {
        "user_type": "regular",
        "email": "user_1@example.example",
        "organization": "example",
        "country": "Portugal",
        "time_stamp": "2019-03-13T16:03:17.355568",
        "name": "User One",
        "username": "user_1",
        "address": null
    },
    "success": true,
    "message": "user created but not activated."
}
```



`POST`  **/api/activate/<activation_key>** - *account activation* <a name="activate"></a>
-

```javascript
// Response Example
{
    "success": true,
    "message": "account activated."
}
```



`POST` **/api/login**  - *user authentication* <a name="login"></a>
-

```javascript
// Request Body Example
{
    username: "user_1",
    password: "0000"
}

// Response Example
{
    "username": "user_1",
    "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZGVudGl0eSI6ImZjdXAzIiwiZXhwIjoxNTU1MDg1NjQxLCJpYXQiOjE1NTI0OTM2NDEsImp0aSI6IjU4MDE4MWFmLTA1MmMtNDljYS1iZmVkLTEzODlhZWJmMTBjMyIsInR5cGUiOiJyZWZyZXNoIiwibmJmIjoxNTUyNDkzNjQxfQ.uZeQrgpjvTJF3rOENhxRE6Co59cTW67pXSmZwvE1GlQ",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZGVudGl0eSI6ImZjdXAzIiwiZXhwIjoxNTUyNTgwMDQxLCJmcmVzaCI6dHJ1ZSwiaWF0IjoxNTUyNDkzNjQxLCJqdGkiOiIyZGNjNjc2Ny00MzllLTRiOGItYmNhYi1jOTczODZmYjc3YjciLCJ0eXBlIjoiYWNjZXNzIiwibmJmIjoxNTUyNDkzNjQxfQ.efD0pn1Uzek1nMaOwnOSPCvidgAl2W7u64B4WcXco2U"
}
```

___

##### NOTE: After logging in, routes requiring authentication must include in the request header the following:

```javascript
// Request Header Example
{
    "Authentication": Bearer <access_token>
}
```
___



`POST`  **/api/forgot** - *forgot password request* <a name="forgot"></a>
-

```javascript
// Request Body Example
{
	"email": "user_1@example.example"
}

// Response Example
{
    "success": true,
    "message": "email sent for reseting password."
}
```



`POST`  **/api/reset/<modification_key>** - *reset password* <a name="reset"></a>
-

```javascript
// Request Body Example
{
	"password": "********"
}

// Response Example
{
    "success": true,
    "message": "password changed."
}
```



`POST`  **/api/logout** - *logout* <a name="logout"></a>
-

```javascript
// Request Header Example
{
    "Authorization": Bearer <access_token>
}

// Response Example
{
    "success": true,
    "message": "Successfully logged out"
}
```

`POST`  **/api/refresh** - *requests an access_token from a refresh_token* <a name="refresh"></a>
-

```javascript
// Request Header Example
{
    "Authorization": Bearer <refresh_token>
}

// Response Example
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZGVudGl0eSI6ImZjdXAzIiwiZXhwIjoxNTUyNDk1Njk0LCJmcmVzaCI6ZmFsc2UsImlhdCI6MTU1MjQ5NDc5NCwianRpIjoiODllOWRmYTgtZmJkOC00MjE2LWFhMzgtZjNjYjFmZDRlZGNhIiwidHlwZSI6ImFjY2VzcyIsIm5iZiI6MTU1MjQ5NDc5NH0.BMrlx3RZIBmWv7at63E4sWLSbtLoYzvvQGc2w5Sn438"
}
```



`GET`  **/api/user** - *requests the users information* <a name="getuser"></a>
-

```javascript
// Request Header Example
{
    "Authorization": Bearer <refresh_token>
}

// Response Example
{
    "success": true,
    "user": {
        "user_type": "regular",
        "email": "user_1@example.example",
        "organization": "example",
        "country": "Portugal",
        "time_stamp": "2019-03-13T16:03:17.355568",
        "name": "User One",
        "username": "user_1",
        "address": null
    }
}
```



`PUT`  **/api/user** - *changes the users information* <a name="putuser"></a>
-

```javascript
// Request Header Example
{
    "Authorization": Bearer <refresh_token>
}

// Request Body Example
{
    "organization": "example 2"
}

// Response Example
{
    "user": {
        "user_type": "regular",
        "email": "user_1@example.example",
        "organization": "example 2",
        "country": "Portugal",
        "time_stamp": "2019-03-13T16:03:17.355568",
        "name": "User One",
        "username": "user_1",
        "address": null
    }
}
```



`GET`  **/api/pendingsinsertions** - *requests pending insertions* <a name="pensinginsertions"></a>
-

```javascript
// Request Header Example
{
    "Authorization": Bearer <refresh_token>
}

// Response Example
{
    "success": true,
    "pending_insertions": [
        [
            "material",
            "material_example with id=161",
            "2019-03-28T12:14:27.014695"
        ],
        [
            "property",
            "teste_property",
            "2019-03-11T00:10:16.923749"
        ],
        [
            "single value",
            "with id=63 for property boiling_point and material Gadolinium",
            "2019-03-29T16:13:34.321606"
        ],
        [
            "multi value",
            "for property delta_T_ad_Happlied and material Gadolinium",
            "2019-03-11T00:07:23.912742"
        ]
    ]
}
```



Groups routes: <a name="group"></a>
-

The groups routes give the information about the groups of materias, e.g. solids, liquids.



`GET`  **/api/groups** - *all groups* <a name="groupsall"></a>
-

This route give all the groups.

```javascript
// Response Example
[
    {
        "time_stamp": "2018-01-17T17:17:05.192418",
        "username": "djsilva",
        "image_url": null,
        "name": "solids",
        "description": "Solid is one of the four fundamental..."
    },
    {
        "time_stamp": "2018-01-17T17:17:05.192418",
        "username": "djsilva",
        "image_url": null,
        "name": "liquids",
        "description": "A liquid is a nearly incompressible..."
    },
    {
        "time_stamp": "2018-01-17T17:17:05.192418",
        "username": "djsilva",
        "image_url": null,
        "name": "gases",
        "description": "Gas is one of the four fundamental..."
    },
    {
        "time_stamp": "2018-01-17T17:17:05.192418",
        "username": "djsilva",
        "image_url": null,
        "name": "nanomaterials",
        "description": "Nanomaterials describe, in principle..."
    }
]
```



`GET`  **/api/group/<group_name>** - *group information* <a name="groupone"></a>
-

```javascript
// Route Example
/api/group/solids

// Response Example
{
    "time_stamp": "2018-01-17T17:17:05.192418",
    "username": "djsilva",
    "image_url": null,
    "name": "solids",
    "description": "Solid is one of the four fundamental states of matter (the others being liquid, gas, and plasma). It is characterized by structural rigidity and resistance to changes of shape or volume..."
}
```



Subgroups routes: <a name="subgroup"></a>
-

The subgroup routes retrieve the information of materials subgroups.



`GET`  **/api/subgroups** - *Fetch all subgroups* <a name="subgroupall"></a>
-

This route give all the subgroups.

```javascript
// Response Example
[
    {
        "description": "A metal is a material...",
        "name": "metal",
        "image_url": "metals.jpg",
        "time_stamp": "2018-01-17T17:18:33.451328",
        "group_name": "solids",
        "id": 2,
        "username": "djsilva"
    },
    {
        "description": "A rare-earth element (REE)...",
        "name": "Rare Earths / Lanthanides",
        "image_url": "solids.jpg",
        "time_stamp": "2018-01-27T18:49:45.242423",
        "group_name": "solids",
        "id": 7,
        "username": "jhb"
    },
    {
        "description": "A liquid is a nearly...",
        "name": "liquids",
        "image_url": null,
        "time_stamp": "2018-11-25T13:00:07.462527",
        "group_name": "liquids",
        "id": 10,
        "username": "jhb"
    },
    {
        "description": "Halocarbon compounds are...",
        "name": "Halocarbons",
        "image_url": null,
        "time_stamp": "2018-11-25T13:47:25.250625",
        "group_name": "liquids",
        "id": 11,
        "username": "jhb"
    }
]

```



`GET`  **/api/subgroup/<subgroup_id>** - *subgroup information* <a name="subgroupone"></a>
-

```javascript
// Route Example
/api/subgroup/1

// Response Example
{
    "description": "A semiconductor material has an electrical conductivity value falling between that of a conductor...",
    "name": "semiconductors",
    "image_url": "semiconductor.jpg",
    "time_stamp": "2018-01-17T17:18:33.451328",
    "group_name": "solids",
    "id": 1,
    "username": "djsilva"
}
```



`GET`  **/api/subgroups/group/<group_id>** - *subgroups by group* <a name="subgroupgroup"></a>
-

```javascript
// Route Example
/api/subgroups/group/solids

// Response Example
[
    {
        "description": "A metal is a material...",
        "name": "metal",
        "image_url": "metals.jpg",
        "time_stamp": "2018-01-17T17:18:33.451328",
        "group_name": "solids",
        "id": 2,
        "username": "djsilva"
    },
    {
        "description": "A rare-earth element...",
        "name": "Rare Earths / Lanthanides",
        "image_url": "solids.jpg",
        "time_stamp": "2018-01-27T18:49:45.242423",
        "group_name": "solids",
        "id": 7,
        "username": "jhb"
    },
    {
        "description": "A semiconductor material...",
        "name": "semiconductors",
        "image_url": "semiconductor.jpg",
        "time_stamp": "2018-01-17T17:18:33.451328",
        "group_name": "solids",
        "id": 1,
        "username": "djsilva"
    }
]
```



`POST`  **/api/subgroups/create** - *subgroup creation* <a name="subgroupcreate"></a>
-

Creates a new subgroup. Requires authentication. group_name and name variables are required in the request body.

```javascript
// Request body
{
	"name": "subgroup_example",
	"group_name": "solids",
	"description": "Example of description.",
	"image_url": "www.example.example/images/99"
}

// Response Example
{
    "description": "Example of description.",
    "name": "subgroup_example",
    "image_url": "www.example.example/images/99",
    "time_stamp": "2019-03-20T15:42:09.309174",
    "group_name": "solids",
    "id": 19,
    "username": "user_1"
}
```



Materials routes: <a name="material"></a>
-

The materials routes give the information about the materials.



`GET`  **/api/materials** - *Fetch all materials* <a name="materialall"></a>
-

This route give all the materials.

```javascript
// Response Example
[
    [
    {
        "time_stamp": "2018-01-28T15:47:35.951094",
        "image_url": "solids.jpg",
        "name": "Silver Antimonium Selenium",
        "id": 11,
        "chemical_formula": "AgSbSe2",
        "description": "An alternative to traditional metal...",
        "subgroup_id": 1,
        "username": "spts"
    },
    ...
]
```



`GET`  **/api/material/<material_id>** - *subgroup information* <a name="materialone"></a>
-

```javascript
// Route Example
/api/material/1

// Response Example
{
    "time_stamp": "2018-01-17T17:22:09.282022",
    "singles": [
        {
            "time_stamp": "2018-01-17T17:24:21.057541",
            "material_id": 1,
            "reference_id": 2,
            "username": "djsilva",
            "id": 1,
            "single_property": "boiling_point",
            "value": 1585
        },
        {
            "time_stamp": "2018-01-17T17:24:21.057541",
            "material_id": 1,
            "reference_id": 2,
            "username": "djsilva",
            "id": 2,
            "single_property": "melting_point",
            "value": 3273
        }
    ],
    "image_url": "metals.jpg",
    "name": "Gadolinium",
    "subgroup": {
        "time_stamp": "2018-01-27T18:49:45.242423",
        "image_url": "solids.jpg",
        "name": "Rare Earths / Lanthanides",
        "group_name": "solids",
        "username": "jhb",
        "description": "A rare-earth element (REE) or rare-earth metal (REM)...",
        "id": 7
    },
    "id": 1,
    "chemical_formula": "Gd1",
    "description": "Gadolinium is a silvery-white,...",
    "subgroup_id": 7,
    "username": "djsilva",
}
```



`GET`  **/api/materials/group/<group_name>** - *group information* <a name="materialsbygroup"></a>
-

```javascript
// Route Example
/api/materials/group/solids

// Response Example
[
    {
        "time_stamp": "2018-01-28T15:47:35.951094",
        "image_url": "solids.jpg",
        "name": "Silver Antimonium Selenium",
        "id": 11,
        "chemical_formula": "AgSbSe2",
        "description": "An alternative to traditional metal tellurides...",
        "subgroup_id": 1,
        "username": "spts"
    },
    ...
]
```



`GET`  **/api/materials/subgroup/<subgroup_id>** - *subgroup information* <a name="materialsbysubgroup"></a>
-

```javascript
// Route Example
/api/materials/subgroup/1

// Response Example
[
    {
        "time_stamp": "2018-01-28T15:47:35.951094",
        "image_url": "solids.jpg",
        "name": "Silver Antimonium Selenium",
        "id": 11,
        "chemical_formula": "AgSbSe2",
        "description": "An alternative to traditional metal tellurides...",
        "subgroup_id": 1,
        "username": "spts"
    },
    ...
]
```



`POST`  **/api/material/create** - *subgroup creation* <a name="materialcreate"></a>
-

Creates a new material. Requires authentication. subgroup_id and name variables are required in the request body.

```javascript
// Request body
{
    "name": "material_example",
    "subgroup_id": 1,
    "description": "Example of description.",
    "image_url": "www.example.example/images/100"
}

// Response Example
{
    "time_stamp": "2019-03-28T12:14:27.014695",
    "image_url": "www.example.example/images/100",
    "name": "material_example",
    "id": 161,
    "chemical_formula": null,
    "description": "Example description.",
    "subgroup_id": 1,
    "username": "user_1"
}
```



Documents routes: <a name="document"></a>
-

The documents routes give the information about the documents.



`GET`  **/api/documents** - *Fetch all documents* <a name="documentall"></a>
-

This route give all the documents.

```javascript
// Response Example
[
    {
        "time_stamp": "2018-01-17T17:20:09.891689",
        "name": "paper",
        "username": "djsilva"
    },
    {
        "time_stamp": "2018-01-17T17:20:09.891689",
        "name": "webpage",
        "username": "djsilva"
    },
    {
        "time_stamp": "2018-11-24T16:36:23.992811",
        "name": "book",
        "username": "spts"
    }
]
```



`GET`  **/api/document/<document_name>** - *Fetch document_id* <a name="documentone"></a>
-

This route give the information about document document_name.

```javascript
// Route Example
/api/documents/paper

// Response Example
{
    "time_stamp": "2018-01-17T17:20:09.891689",
    "name": "paper",
    "username": "djsilva"
}
```



References routes: <a name="reference"></a>
-

The references routes give the information about the references.



`GET`  **/api/refs** - *Fetch all documents* <a name="referenceall"></a>
-

This route give all the references.

```javascript
// Response Example
[
    {
        "time_stamp": "2018-01-17T17:23:52.707393",
        "authors": "T. F. Petersen, N. Pryds, A. Smith, J. Hattel, H. Schmidt, H.-J. H. Knudsen",
        "source_id": 1,
        "id": 1,
        "title": "Two-dimensional mathematical model of a reciprocating room-temperature Active Magnetic Regenerator",
        "username": "djsilva",
        "url": null,
        "doi": "10.1016/j.ijrefrig.2007.07.009"
    },
    ...
]
```



`GET`  **/api/ref/<ref_id>** - *Fetch ref_id* <a name="referenceone"></a>
-

This route give the information about reference reference_id.

```javascript
// Route Example
/api/ref/1

// Response Example
{
    "time_stamp": "2018-01-17T17:23:52.707393",
    "authors": "T. F. Petersen, N. Pryds, A. Smith, J. Hattel, H. Schmidt, H.-J. H. Knudsen",
    "source_id": 1,
    "id": 1,
    "title": "Two-dimensional mathematical model of a reciprocating room-temperature Active Magnetic Regenerator",
    "username": "djsilva",
    "url": null,
    "doi": "10.1016/j.ijrefrig.2007.07.009"
}
```



`POST`  **/api/ref/create** - *reference creation* <a name="referencecreate"></a>
-

Creates a new reference. Requires authentication. source_id and title variables are required in the request body.

```javascript
// Request body
{
	"title": "title_example",
	"source_id": 1,
	"authors": "example_author.",
	"doi": "example_doi",
    "url": "www.example.example"
}

// Response Example
{
    "time_stamp": "2019-03-28T12:14:27.043957",
    "authors": "example_author",
    "source_id": 1,
    "id": 23,
    "title": "blabla",
    "username": "user_1",
    "url": "www.exampleref.io",
    "doi": "example_doi"
}
```

Point properties routes: <a name="pointproperty"></a>
-

The point properties routes give the information about the pointproperties.



`GET`  **/api/pointproperties** - *Fetch all point properties* <a name="pointpropertyall"></a>
-

This route give all the point properties.

```javascript
// Response Example
[
    {
        "name": "Corrosion rate in 5% Sulfuric Acid\n",
        "username": "jhb",
        "fullname": "",
        "description": "The rate of corrosion is the ...",
        "time_stamp": "2018-03-11T13:45:15.370487",
        "si_units": "g m[-2] h[-1]"
    },
    ...
]
```



`GET`  **/api/pointproperty/<property_id>** - *Fetch all point properties* <a name="pointpropertyone"></a>
-

This route give the information about property_id.

```javascript
// Route Example
/api/pointproperty/thermal_conductivity

// Response Example
{
    "name": "thermal_conductivity",
    "username": "djsilva",
    "fullname": "Thermal Conductivity",
    "description": "Thermal conductivity is the property of ...",
    "time_stamp": "2018-01-17T17:19:16.671457",
    "si_units": "[W][m^-1][K^-1]"
}
```



`POST`  **/api/pointproperty/create** - *Create a pointproperty* <a name="pointpropertycreate"></a>
-

This route creates a new pointproperty. Requires authentication. name, fullname and si_units variables are required in the request body.

```javascript
// Request body
{
    "name": "example_property",
    "fullname": "Point Property Example",
    "si_units": "[units]",
    "description": "example_description"
}

// Response Example
{
    "name": "example_property",
    "username": "user_1",
    "fullname": "Point Property Example",
    "description": "example_description",
    "time_stamp": "2019-03-28T16:40:45.825464",
    "si_units": "[units]"
}
```



`GET`  **/api/pointproperty/material/<material_id>** - *Fetch all point properties of a material* <a name="pointpropertiesmat"></a>
-

Fetch all point properties of material_id

```javascript
// Route Example
/api/pointproperty/material/1

// Response Example
[
    {
        "name": "delta_T_ad_Happlied",
        "username": "djsilva",
        "fullname": "&Delta;T<sub>ad,H</sub>",
        "description": "Change of temperature during an adiabatic application of magnetic field, normally associated to the magnetocaloric effect.",
        "time_stamp": "2018-01-17T17:19:16.671457",
        "si_units": "[K]"
    },
    ...
]
```



`GET`  **/api/pointproperty/dependences** - *Fetch all dependences of pointproperties* <a name="pointpropertiesdep"></a>
-

Fetch all dependences of pointproperties

```javascript
// Route Example
/api/properties/dependences

// Response Example
[
    "temperature",
    "pressure",
    "ex",
    "ey",
    "ez",
    "hx",
    "hy",
    "hz"
]
```



`POST`  **/api/pointproperty/<mat_id>/<prop_id>/dependence_values** - *Fetch the allowed values of dependences* <a name="pointpropertydepvals"></a>
-

This route fetch all the values present in the database for the dependences of material id mat_id and pointproperty prop_id, and divides them into filtered and unfiltered. These values are used when filtering the plots. This route requires a listboxvariable, e.g. temperature, in the body request for the variable in question. The variable dependences is optional and filters the remaining variables. The variable dependences has the json structure.

```javascript
// Route Example
/api/pointproperty/1/delta_T_ad_Happlied/dependence_values

// Request body
{
    "listboxvariable": "temperature",
    "dependences": "{\"hx\":1}"
}

// Response Example
{
    "unfiltered": [
        257.347,
        259.021,
        261.429,
        263.469,
        265.464,
        267.143,
        270.408
    ],
    "filtered": [
        260.702,
        264.915,
        268.589,
        272.138
    ]
}
```



Single properties routes: <a name="singleproperty"></a>
-

The single properties routes give the information about the singleproperties.


`GET`  **/api/singleproperties** - *Fetch all single properties* <a name="singlepropertyall"></a>
-

This route give all the point properties.

```javascript
// Response Example
[
    {
        "description": "The boiling point of a ...",
        "username": "djsilva",
        "name": "boiling_point",
        "fullname": "Boiling Point",
        "si_units": "[K]",
        "time_stamp": "2018-01-17T17:19:32.450993"
    },
    ...
]
```



`GET`  **/api/singleproperty/<property_id>** - *Fetch all single properties* <a name="singlepropertyone"></a>
-

This route give the information about property_id.

```javascript
// Route Example
/api/singleproperty/boiling_point

// Response Example
{
    "description": "The boiling point of a ...",
    "username": "djsilva",
    "name": "boiling_point",
    "fullname": "Boiling Point",
    "si_units": "[K]",
    "time_stamp": "2018-01-17T17:19:32.450993"
}
```



`GET`  **/api/singleproperty/material/<material_id>** - *Fetch all single properties of a material* <a name="singlepropertiesmat"></a>
-

Fetch all single properties of material_id

```javascript
// Route Example
/api/singleproperty/material/1

// Response Example
[
    {
        "description": "The melting point (or, rarely, liquefaction point) ...",
        "username": "djsilva",
        "name": "melting_point",
        "fullname": "Melting Point",
        "si_units": "[K]",
        "time_stamp": "2018-01-17T17:19:32.450993"
    },
    ...
]
```



`POST`  **/api/singleproperty/create** - *Create a singleproperty* <a name="singlepropertycreate"></a>
-

This route creates a new singleproperty. Requires authentication. name, fullname and si_units variables are required in the request body.

```javascript
// Request body
{
    "name": "example_property",
    "fullname": "Single Property Example",
    "si_units": "[units]",
    "description": "example_description"
}

// Response Example
{
    "description": "example_description",
    "username": "user_1",
    "name": "example_property",
    "fullname": "Single Property Example",
    "si_units": "[units]",
    "time_stamp": "2019-03-29T15:35:48.692538"
}
```



Points: <a name="point"></a>
-

The points routes give the information about the points.



`POST`  **/api/points/<material_id>/<pointproperty_name>** - *Fetch points for material_id and pointproperty_name* <a name="pointsmatprop"></a>
-

This route give all the points for material_id and pointproperty_name. The variable dependences is optional and filters the remaining variables. The variable dependences has the json structure.

```javascript
// Route Example
/api/points/1/delta_T_ad_Happlied

// Request body
{
    "dependences": "{\"hx\":1}"
}

// Response Example
[
    {
        "hy": null,
        "material_id": 1,
        "username": "user_1",
        "ey": null,
        "hz": null,
        "ex": null,
        "ez": null,
        "temperature": 260.702,
        "hx": 1,
        "value": 1.52429,
        "pressure": null,
        "point_property": "delta_T_ad_Happlied",
        "time_stamp": "2018-01-17T17:24:21.057541",
        "reference_id": 1,
        "id": 1
    },
    ...
]
```



`POST`  **/api/point/create** - *Create a point* <a name="pointcreate"></a>
-

This route creates a new point. Requires authentication. point_property, material_id and reference_id and value variables are required in the request body. Moreover, hx, hy, hz, ex, ey, ez, temperature and pressure are optional variable.

```javascript
// Request body
{
    "point_property": "delta_T_ad_Happlied",
    "material_id": 1,
    "reference_id": 1,
    "value": 1
}

// Response Example
{
    "name": "example_property",
    "username": "user_1",
    "fullname": "Point Property Example",
    "description": "example_description",
    "time_stamp": "2019-03-28T16:40:45.825464",
    "si_units": "[units]"
}
```



`POST`  **/api/points/create** - *Create a set of points* <a name="pointscreate"></a>
-

This route creates a new set of points. Requires authentication. point_property, material_id, reference_id and points variables are required in the request body. The points variable is a list of json objects that indicate the values of each point.

```javascript
// Request body
{
    "point_property": "delta_T_ad_Happlied",
    "material_id": 1,
    "reference_id": 1,
    "points": [
        {"value":9999,"temperature":900},
        {"value":9990,"temperature":800, "hx":0.1}
    ]
}

// Response Example
{
    "success": true,
    "inserted_points": [
        {
            "reference_id": 1,
            "pressure": null,
            "id": 30863,
            "username": "user_1",
            "ez": null,
            "time_stamp": "2019-03-29T16:13:34.339673",
            "ey": null,
            "temperature": 900,
            "hz": null,
            "hy": null,
            "point_property": "delta_T_ad_Happlied",
            "material_id": 1,
            "hx": null,
            "value": 9999,
            "ex": null
        },
        {
            "reference_id": 1,
            "pressure": null,
            "id": 30864,
            "username": "user_1",
            "ez": null,
            "time_stamp": "2019-03-29T16:13:34.339673",
            "ey": null,
            "temperature": 800,
            "hz": null,
            "hy": null,
            "point_property": "delta_T_ad_Happlied",
            "material_id": 1,
            "hx": 0.1,
            "value": 9990,
            "ex": null
        }
    ]
}
```



Singles: <a name="single"></a>
-

The singles routes give the information about the single value properties.



`GET`  **/api/single/<single_id>** - *Fetch single_id* <a name="singleone"></a>
-

This route give the information about single_id.

```javascript
// Route Example
/api/single/1

// Response Example
{
    "reference_id": 2,
    "id": 1,
    "username": "djsilva",
    "material_id": 1,
    "value": 1585,
    "time_stamp": "2018-01-17T17:24:21.057541",
    "single_property": "boiling_point"
}
```



`POST`  **/api/single/create** - *Create a single* <a name="singlecreate"></a>
-

This route creates a new point. Requires authentication. single_property, material_id and reference_id and value variables are required in the request body.

```javascript
// Request body
{
    "single_property": "boiling_point",
    "material_id": 1,
    "reference_id": 1,
    "value": 1
}

// Response Example
{
    "reference_id": 1,
    "id": 63,
    "username": "user_1",
    "material_id": 1,
    "value": 1,
    "time_stamp": "2019-03-29T16:13:34.321606",
    "single_property": "boiling_point"
}
```



Search and messages routes: <a name="message"></a>
-

These are the remaining routes of the API.



`POST`  **/api/search/advanced** - *Fetch all materials for the filtered search* <a name="search"></a>
-

This route give all the materials for the search request. search_field in the request body is required.

```javascript
// Request body
{
    "search_field": [
        {
            "val":"delta_T_ad_Happlied","type":"pointproperty",
            "dependences": {
                "temperature":{},
                "pressure":{},
                "hx": {
                    "min":0,
                    "max":10.5
                },
                "hy":{},
                "hz":{},
                "ex":{},
                "ey":{},
                "ez":{}
            },
            "min":"0.5",
            "max":"1.5"
        }
    ]
}

// Response Example
[
    {
        "results": [
            {
                "id": 1,
                "chemical_formula": "Gd1",
                "username": "djsilva",
                "subgroup_id": 7,
                "description": "Gadolinium is a silvery-white...",
                "image_url": "metals.jpg",
                "name": "Gadolinium",
                "time_stamp": "2018-01-17T17:22:09.282022"
            }
        ]
    }
]
```



`POST`  **/api/message** - *Sends a message to server* <a name="messagesend"></a>
-

This route sends a message to the matfinder server. Required for creating data that are not included in the other routes, e.g. to create a material group.

```javascript
// Request body
{
    "email": "example@example",
    "title": "Example of message",
    "content": "example content"
}

// Response Example
{
    "success": true,
    "message": "Message received."
}
```
