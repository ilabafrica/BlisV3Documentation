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

Data :: 

	{
    'headers' : {
      'Accept' : 'application/json',
      'Content-type' : 'application/json',
      'Authorization' : 'Bearer '.$accessToken
    }
    'json' :  {
        "resourceType" : "DiagnosticOrder",
        "subject" : {
          "identifier" : "CJjajd6237",
          "name": "Makanga Dickson",
          "birthDate": "1994-05-30",
          "gender": "male"
        },
        "orderer" : {
          "name":"Alfred Ojok",
          "contact":"+777 7777 777 777",
          "organization":"iLabAfrica Medical Centre"
        },
        "encounter" : {
          "resourceType" : "Encounter",
          "class":"outpatient",
        },
        "supportingInformation" : "-",
        "item" : {
          {
            "test_type_id":"EGerG54",
            "specimen" : "Stool"
          }
        },
        "note" : "lorem ipsum"
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
	    'json' :  {
		  "resourceType" : "DiagnosticReport",
		  "identifier" : "MBaX77TLgUgZOOIlSh",
		  "subject" : {
		    "identifier" : "MBaX77TLgUgZOOIlSh",
		  },
		  "result" : {
		    "resourceType" : "Observation",
		    "identifier" : "BaX77TLgUgZ",
		    "effectiveDateTime" : NULL,
		    "issued" : NULL,
		    "performer" : "Alex Lab",
		    "component" : [
		      {
		        "code" : "Specific Gravity",
		        "valueString" : "10",
		      }
		    ],
		  },
		}  
	}