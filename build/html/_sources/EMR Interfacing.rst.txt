EMR Interfacing
===============

BLIS allows tests to be requested via an external system. BLIS has been configured in a way where  a test  can be requested by multiple external systems. This will only happen if the systems can integrate and map fully with BLIS. External system configuration and integration with BLIS will depend on the available systems in any given hospital. 

Currently BLIS allows intergration of SANITAS and mHealth4Afrika  to interface with BLIS.

A user can send a test request to BLIS and receive the results.

Steps for BLIS Integration to an EMR API
****************************************

*The EMR should try to log in to the BLIS server to authenticate if its allowed to send a test request.eg. 
Url: ::
                  api/tpa/login

Data: ::

  {
    "headers" : {
        "Accept" : "application/json",
        "Content-type" : "application/json"
    },
    "json" : {
        "email" : "email@example.com",
        "password" : "password"
     },
  }


*BLIS should have the credentials that EMR shoud use inorder to give them access to send a request to the system.

*When the EMR is authenticated it receives an access token from BLIS and uses it to post test requests to BLIS.eg.
Url:: 
				/api/testrequest

Data: :: 

	{
    'headers' : 
      'Accept' : 'application/json',
      'Content-type' : 'application/json',
      'Authorization' : 'Bearer '.$accessToken
        }
    'json' :  { 
    "resourceType": "ProcedureRequest",
    "contained": [
    {
      "resourceType": "Patient",
      "id": "patient",
      "identifier": [
        {
          "value": "CJjajd6237"
        }
      ],
      "name": [
        {
          "family": "Dickson",
          "given": [
            "Makanga"
          ]
        }
      ],
      "gender": "male",
      "birthDate": "1994-05-30"
    },
    {
      "resourceType": "Practitioner",
      "id": "practitioner",
      "identifier": [
        {
          "value": "blis-username"
        }
      ],
      "name": [
        {
          "family": "John",
          "given": [
            "Doe"
          ]
        }
      ],
      "telecom": [
        {
          "system": "phone",
          "value": "+2541234567"
        },
        {
          "system": "email",
          "value": "jdoe@blis.com.ke"
        }
      ]
    }
  ],
  "code": {
    "coding": [
      {
        "system": "http://www.mhealth4afrika.eu/fhir/StructureDefinition/dataElementCode",
        "code": "art-hb"
      }
    ]
  },
  "subject": {
    "reference": "#patient"
  },
  "context": {
    "reference": "Encounter/abC32ah4X"
  },
  "requester": {
    "agent": {
      "reference": "#practitioner"
    }
  }


After BLIS receives a test request from an EMR and completes the result entry,it does the following:

*First BLIS tries to log into the EMR, When BLIS is authenticated it receives an access token from EMR and uses it to post test results to EMR eg.::


	{
	      'headers' : {
	      'Accept' : 'application/json',
	      'Content-type' : 'application/json',
	      'Authorization' : 'Bearer '.$accessToken
	   }
        'json' : { 
    "resourceType": "DiagnosticReport",
    "contained": [
        {
            "resourceType": "Observation",
            "id": "Observation1",
            "extension": [
                {
                    "url": "http://www.mhealth4afrika.eu/fhir/StructureDefinition/dataElementCode",
                    "valueCode": "art-hb"
                }
            ],
            "code": {
                "coding": [
                    {
                        "system": "http://loinc.org",
                        "code": "718-7",
                        "display": "Hemoglobin [Mass/volume] in Blood"
                    }
                ]
            },
            "effectiveDateTime": "2019-01-14T17:12:54+03:00",
            "performer": [
                {
                    "reference": "Practitioner/blis-username"
                }
            ],
            "valueQuantity": {
                "value": 7.2,
                "unit": "g/dl",
                "system": "http://unitsofmeasure.org",
                "code": "g/dL"
            }
        }
    ],
    "identifier": [
        {
            "value": "diagnosticReportExampleIdentifier"
        }
    ],
    "subject": {
        "reference": "Patient/CJjajd6237"
    },
    "context": {
        "reference": "Encounter/abC32ah4X"
    },
    "performer": [
        {
            "actor": {
                "reference": "Practitioner/blis-username"
            }
        }
    ],
    "result": [
        {
            "reference": "#Observation1"
        }
    ]
}