##################
# ADMIN REST API #
##################

==========================
Setting up a new election:
==========================

/steve/admin/setup/$electionid
  POST
    input: 
          title: Title of the election
          owner: uid of election owner
          monitors: email addresses of monitors
          starts: UNIX timestamp of start (optional)
          ends: UNIX timestamp when it closes (optional)
    output:
      HTTP 201 Created on success
      HTTP 400 Bad request if params missing or invalid or already exists
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "created"
        }
        
==============================
Editing an existing election:
==============================

/steve/admin/edit/$electionid
  POST
    input: 
          title: Title of the election (optional)
          owner: uid of election owner (optional)
          monitors: email addresses of monitors (optional)
          starts: UNIX timestamp of start (optional)
          ends: UNIX timestamp when it closes (optional)
    output:
      HTTP 200 Saved on success
      HTTP 404 Not Found if no such election
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "Changes saved"
        }
        
        
        
=================================
Creating an issue in an election:
=================================

/steve/admin/create/$electionid/$issueid
  POST
    input: 
          title: Title of the issue
          description: (optional) Description of the issue
          type: type of issue (yna, stv[1-9])
          candidates: (if stv[1-9]) \n separated list of candidate names
          nominatedby: (if YNA) Person to nominate this issue
          seconds: (if YNA) \n separated list of seconds for the issue
          
    output:
      HTTP 201 Created on success
      HTTP 400 Bad request if params missing or invalid or already exists
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "created"
        }
       
        
================================
Editing an issue in an election:
================================

/steve/admin/edit/$electionid/$issueid
  POST
    input: 
          title: Title of the issue (optional)
          description: (optional) Description of the issue (optional)
          type: type of issue (yna, stv[1-9]) (optional)
          candidates: (if stv[1-9]) \n separated list of candidate names (optional)
          nominatedby: (if YNA) Person to nominate this issue (optional)
          seconds: (if YNA) \n separated list of seconds for the issue (optional)
          
    output:
      HTTP 200 Saved on success
      HTTP 404 Not Found if no such issue or election
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "Changes saved"
        }
        
        
==================================
Adding a statement to a candidate:
==================================

/steve/admin/statement/$electionid/$issueid
  POST
    input: 
          candidate: Candidate to set statement for
          statement: Statement to set/add
          
    output:
      HTTP 200 Saved on success
      HTTP 400 Bad request if params missing
      HTTP 404 Not Found if candidate is not on the ballot
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "Changes saved"
        }

        
================================
Adding a candidate to the issue:
================================

/steve/admin/addcandidate/$electionid/$issueid
  POST
    input: 
          candidate: Candidate to set add
          statement: Statement to set/add (optional)
          
    output:
      HTTP 200 Saved on success
      HTTP 400 Bad request if params missing
      HTTP 404 Not Found if no such issue/election
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "Changes saved"
        }


===================================
Deleting a candidate to the issue:
===================================

/steve/admin/delcandidate/$electionid/$issueid
  POST
    input: 
          candidate: Candidate to delete
          
    output:
      HTTP 200 Saved on success
      HTTP 400 Bad request if params missing
      HTTP 404 Not Found if candidate is not on the ballot
      HTTP 500 on error
    response payload:
       JSON formatted
        {
        "message": "Changes saved"
        }






##################
# VOTER REST API #
##################

============================
Viewing election and issues:
============================

/steve/voter/view/$electionid
  GET
    input:
      uid: Voter hash
    output:
      HTTP 200 Okay if election exists
      HTTP 403 Forbidden if voter hash missing or invalid
      HTTP 404 Not Found if no such election
    response payload:
      JSON formatted
        {
        "base_data": {
            "owner": "humbedooh", 
            "starts": null, 
            "ends": null, 
            "monitors": [
                "humbedooh@humbedooh.com"
            ], 
            "title": "Foo Elections, 2015"
        }, 
        "baseurl": "https://stv.website/steve/election?foo", 
        "issues": [
            {
                "description": null, 
                "title": "Test Board Election", 
                "APIURL": "https://stv.website/steve/voter/view/foo/baz", 
                "prettyURL": "https://stv.website/steve/ballot?foo/baz", 
                "candidates": [
                    {
                        "name": "Just One Guy"
                    }, 
                    {
                        "name": "John Doe", 
                        "statement": "Vote for me!"
                    }, 
                    {
                        "name": "Kate Smurf", 
                        "statement": "Vote for me too!"
                    }
                ], 
                "type": "stv6", 
                "id": "baz"
            }, 
            {
                "description": "This is to nominate WALL-E for ASF membership", 
                "title": "Membership for WALL-E", 
                "seconds": "humbedooh", 
                "APIURL": "https://stv.website/steve/voter/view/foo/member-wall-e", 
                "prettyURL": "https://stv.website/steve/ballot?foo/member-wall-e", 
                "candidates": [], 
                "nominatedby": "mattman", 
                "type": "yna", 
                "id": "member-wall-e"
            }
        ]
    }
    
    
=======================
Viewing a single issue:
=======================
/steve/voter/view/$electionid/$issueid
  GET
    input:
      uid: Voter hash
    output:
      HTTP 200 Okay if issue exists
      HTTP 404 Not Found if no such election/issue
      HTTP 403 Forbidden if voter hash missing or invalid
    response payload:
      JSON formatted
        {
          "issue": {
              "description": null, 
              "title": "Test Board Election", 
              "APIURL": "https://stv.website/steve/voter/view/foo/baz", 
              "prettyURL": "https://stv.website/steve/ballot?foo/baz", 
              "candidates": [
                  {
                      "name": "Just One Guy"
                  }, 
                  {
                      "name": "John Doe", 
                      "statement": "Vote for me!"
                  }, 
                  {
                      "name": "Kate Smurf", 
                      "statement": "Vote for me too!"
                  }
              ], 
              "type": "stv6", 
              "id": "baz"
          }
      }
      
      
      
      
MORE TO COME!