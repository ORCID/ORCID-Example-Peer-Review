#Testing Peer Review

This repository was created to test the Peer Review functionality on both QA and on Sandbox. It was written out to serve as an example for anyone who may be interested in the steps taken during this testing to add and edit peer review activities. NOTE that the access codes, etc listed here will not work for anyone else, as they were deleted shortly after this log was created.

##1. Register the Group IDs that I'm going to use

###Get an access token

```script
curl -i -L -k -H 'accept:application/json' \
	-d 'client_id=<your client id here>' \
	-d 'client_secret=<your client secret here>' \
	-d 'scope=/group-id-record/update' \
	-d 'grant_type=client_credentials' \
	'http://api.qa.orcid.org/oauth/token'
```

response token

```script
{"access_token" : "<access token>",
 "token_type"   : "bearer",
 "refresh_token" : "<refresh token>"
 "expires_in"   : 631138518,
 "scope"         : "/group-id-record/update",
 "orcid"         : null}
```


###Register Groups

| File | Resulting location |
| ---- | ----------------- |
| grouptest1.xml | https://api.sandbox.orcid.org/v2.0_rc1/group-id-record/1000 |
| grouptest2.xml | https://api.sandbox.orcid.org/v2.0_rc1/group-id-record/1001 |
| grouptest3.xml | https://api.sandbox.orcid.org/v2.0_rc1/group-id-record/1002 |


```script
curl -L -i -k -H 'Content-type: application/vnd.orcid+xml' \
	-H 'Authorization: Bearer <access token>' \
	-d '@<FILE>' \
	-X POST 'http://api.sandbox.orcid.org/v2.0_rc1/group-id-record'
```

response

```script
HTTP/1.1 201 Created
...
Location: <RESULTING LOCATION>
...
```

###Update Groups that have already been added

How to update a Group ID that already exists. _**IMPORTANT NOTE**: Before updating, the put-code attribut value (matching the group-id-record number) must be included in the opening ```<group-id:group-id-record>``` element._

```script
curl -L -i -k -H 'Content-type: application/vnd.orcid+xml' \
	-H 'Authorization: Bearer <access token>' \
	-d '@<FILE>' \
	-X PUT '<RESULTING LOCATION>'
```

###See the Groups that are already available

How to see a paged list of the Groups that already exist _(shows the first page of 10 groups)_
```script
curl -L -i -H 'Content-Type: application/vnd.orcid+xml' \
	-H 'Authorization: Bearer <access token>' \
	'http://api.sandbox.orcid.org/v2.0_rc1/group-id-record/?page-size=10&page=1' 
```

How to see a specific Group _(once the group location is known)_
```script
curl -L -i -H 'Content-Type: application/vnd.orcid+xml' \
	-H 'Authorization: Bearer <access token>' \
	'<GROUP LOCATION>' 
```

##2. Get an access token for adding peer review

###Display the OAuth window (sandbox) - activities/update
```
https://sandbox.orcid.org/oauth/authorize?\
	client_id=<client-id>&\
	response_type=code&\
	scope=/activities/update&\
	redirect_uri=https://developers.google.com/oauthplayground
```

###Get the access token from the code

code ```<authorization-code>```


```script
curl -i -L -k -H 'accept:application/json' \
	-d 'client_id=<client-id>' \
	-d 'client_secret=<client-secret>' \
	-d 'grant_type=authorization_code' \
	-d 'code=<authorization-code>' \
	-d 'redirect_uri=https%3A%2F%2Fdevelopers.google.com%2Foauthplayground' \
	'https://api.sandbox.orcid.org/oauth/token'

```

```bash
HTTP/1.1 200 OK
...

{"access_token" : "<access-token>",
 "token_type"   : "bearer",
 "refresh_token": "<refresh-token>",
 "expires_in"   : 631138518,
 "scope"        : "/activities/update",
 "orcid"        : "<ORCID-ID-Path>",
 "name"         : "<User's Name>"}
```
##3. Post the peer review activities

| File | Resulting location |
| ---- | ----------------- |
| reviewtest1.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1008 |
| reviewtest2.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1009 |
| reviewtest3.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1010 |
| reviewtest4.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1011 |
| reviewtest5.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1012 |
| reviewtest6.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1013 |
| reviewtest7.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1014 |
| reviewtest8.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1015 |
| reviewtest9.xml | https://api.sandbox.orcid.org/v2.0_rc1/0000-0002-4955-8190/peer-review/1016 |


```script
curl -i -k -H 'Content-Type: application/orcid+xml' \
	-H 'Authorization: Bearer <access-token>' \
	-d '@<FILE>' \
	-X POST 'http://api.sandbox.orcid.org/v2.0_rc1/<ORCID-ID-Path>/peer-review'
```

response

```script
HTTP/1.1 201 Created
Server: nginx/1.1.19
Content-Type: application/vnd.orcid+xml; qs=5;charset=UTF-8
Date: Wed, 05 Aug 2015 22:50:19 GMT
Location: <RESULTING LOCATION>
Access-Control-Allow-Origin: *
Connection: keep-alive
Set-Cookie: X-Mapping-fjhppofk=A9DC5EED7BF796E67E5C882A85C51FA5; path=/
Content-Length: 0
```

#References and resources

* [ORCID API v2.0_rc1 Peer Review Guide](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/record_2.0_rc1/peer-review-guide-v2.0_rc1.md)
* [Peer Review XSD](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/record_2.0_rc1/peer-review-2.0_rc1.xsd)
* [Peer Review sample XML](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/record_2.0_rc1/samples/peer-review-2.0_rc1.xml)
* [Group ID API Documentation](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/group-id-2.0_rc1/README.md)
* [Group ID XSD](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/group-id-2.0_rc1/group-id-2.0_rc1.xsd)
* [Group ID Sample XML](https://github.com/ORCID/ORCID-Source/blob/master/orcid-model/src/main/resources/group-id-2.0_rc1/samples/group-id-2.0_rc1.xml)

#Formatted XML _(used for these examples)_
The following XML was used to add records to an ORCID record. This data was used to match the following screen shot:

![screenshot](https://www.evernote.com/l/AA2otYhI_c5JNKZfyEacLB1tQQsE3WiNRcsB/image.png)

##Group IDs
###Group 1

File: [grouptest1.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/grouptest1.xml)

| Field | Value |
| ----- | ----- |
| **Group display name** | Journal of Neuroscience |
| **Group type/identifier** | issn:0270-6474 |
| **Group description** | The Journal of Neuroscience is the official journal of the Society for Neuroscience. The Journal of Neuroscience publishes papers on a broad range of topics of general interest to those working on the nervous system. http://www.jneurosci.org/ |
| **Group type** | journal |

###Group 2

File: [grouptest2.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/grouptest2.xml)

| Field | Value |
| ----- | ----- |
| **Group display name** | Wellcome Trust |
| **Group type/identifier** | ringgold:5072 |
| **Group description** | The Wellcome Trust is a global charitable foundation dedicated to improving health by supporting bright minds in science, the humanities and social sciences, and public engagement. http://www.wellcome.ac.uk/ |
| **Group type** | institution |

###Group 3

File: [grouptest3.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/grouptest3.xml)

| Field | Value |
| ----- | ----- |
| **Group display name** | Nature Neuroscience |
| **Group type/identifier** | issn:1097-6256 |
| **Group description** | Nature Neuroscience is a multidisciplinary journal that publishes papers of the highest quality and significance in all areas of neuroscience. The editors welcome contributions in molecular, cellular, systems and cognitive neuroscience, as well as psychophysics, computational modeling and diseases of the nervous system. No area is excluded from consideration, although priority is given to studies that provide fundamental insights into the functioning of the nervous system. http://www.nature.com/neuro/index.html |
| **Group type** | journal |

##Reviews
###Review 1

File: [reviewtest1.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest1.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: WTP059408;<br/>http://www.wellcome.ac.uk/Funding/Biomedical-science/Application-information/Committees/WTP059408.htm |
| **review-url** | http://www.wellcome.ac.uk/Funding/Biomedical-science/Application-information/Committees/WTP059408.htm |
| **review-type** | review |
| **completion-date** | 2014 |
| **review-group-id** | ringgold:5072 (Wellcome Trust) |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:5072 |

###Review 2

File: [reviewtest2.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest2.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: JN-2309823049; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 1996 |
| **review-group-id** | issn:0270-6474 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:174507 (Stanford University Press, Palo Alto, CA, US) |


###Review 3

File: [reviewtest3.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest3.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: N-23098922089; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2002 |
| **review-group-id** | issn:0270-6474 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:174507 (Stanford University Press, Palo Alto, CA, US) |

###Review 4

File: [reviewtest4.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest4.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: JN-2307823009; |
| **review-url** |  |
| **review-type** | Review |
| **completion-date** | 2009 |
| **review-group-id** | issn:0270-6474 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:174507 (Stanford University Press, Palo Alto, CA, US |

###Review 5

File: [reviewtest5.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest5.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | editor |
| **reviewidentifiers** | source-work-id: JN-2309826959; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2015-07 |
| **review-group-id** | issn:0270-6474 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:1216 (Elsevier BV, Amsterdam, Noord-Holland, NL) |

###Review 6

File: [reviewtest6.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest6.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: NPG-Neuro-230497826; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2005 |
| **review-group-id** | issn:1097-6256 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:53264 (NPG, London, London, GB) |

###Review 7

File: [reviewtest7.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest7.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: NPG-Neuro-230497837; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2014-11 |
| **review-group-id** | issn:1097-6256 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:53264 (NPG, London, London, GB) |

###Review 8

File: [reviewtest8.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest8.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: NPG-Neuro-230497848; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2014-03-20 |
| **review-group-id** | issn:1097-6256 |
| **subject-external-identifier** |  |
| **subject-container-name** |  |
| **subject-type** |  |
| **subject-title** |  |
| **subject-url** |  |
| **convening-organization** | ringgold:53264 (NPG, London, London, GB) |

###Review 9

File: [reviewtest9.xml](https://github.com/ORCID/ORCID-Example-Peer-Review/blob/master/reviewtest9.xml)

| Field | Value |
| ----- | ----- |
| **orcid-id** | qa.orcid.org/0000-0001-6356-0580 |
| **reviewer-role** | reviewer |
| **reviewidentifiers** | source-work-id: NPG-Neuro-230497859; |
| **review-url** |  |
| **review-type** | review |
| **completion-date** | 2013-02 |
| **review-group-id** | issn:1097-6256 |
| **subject-external-identifier** | DOI:230.3/0239x894.85 |
| **subject-container-name** | Nature Neuroscience |
| **subject-type** | journal-article |
| **subject-title** | Title of the publication that was reviewed |
| **subject-url** |  |
| **convening-organization** | ringgold:53264 (NPG, London, London, GB) |

