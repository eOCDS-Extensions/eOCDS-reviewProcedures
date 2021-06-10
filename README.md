# ReviewProcedures

Extension to cover the legal actions which allow EOs to request the enforcement of public procurement regulations and their rights under those regulations in cases where CAs fail to comply with the legal framework for public procurement.

## Background

Detailed provisions relating to the types of remedy available are generally subject to local law. But generally speaking,  four types of remedies are available: 
- Interim measures
- Set aside of decisions 
- Annulment of a Concluded Contract
- Damages
- Alternative penalties ([suggested](https://github.com/uStudioCompany/eOCDS-reviewProcedures/issues/3) by @JachymHercher)

### Types of remedies
#### Interim measures

Interim measures are provisional measures taken in relation to the contract notice and any contracting decision, including the contract award decision. The aim of interim measures is to prevent the creation of unalterable situations and to avoid the continuation of the contract award procedure without an economic operator that would otherwise have been able to participate and possibly be awarded the contract. These aims may only be achieved if the local legal system provides an effective, simple and speedy possibility of obtaining interim relief and if the competent review body is not reluctant to grant interim relief as a matter of principle.

The following interim measures can typically be requested:
- Suspension of the implementation of any decision taken by the contracting authority
- Suspension of the whole contract award procedure

#### Set aside of decisions

The application for the set-aside remedy cancels or renders ineffective a contracting decision taken unlawfully or otherwise, corrects an unlawful situation. The aim of set-aside is to correct proven irregularities. This aim is only achieved if the local legal system provides an effective possibility of canceling an unlawful specification or contracting decision and if the competent review body reviews the reasonableness of contracting decisions.

The following measures can typically be ordered:
- Removal of discriminatory technical, economic or financial specifications in the contract notice, tender documents or any other document relating to the contract award procedure;
- Annulment of an unlawful contracting decision
- Positive correction of any unlawful document or contracting decision, for example, an order of the contracting authority to amend or delete an unlawful clause in the tender documents or to reinstate an economic operator that had been unlawfully excluded.

#### Annulment of a Concluded Contract

The annulment of an already concluded contract or a declaration that the contract is null and void by a third party to the contract is an available remedy, which applied at least if the contract is concluded during the standstill period. The possibility to annul a concluded contract is a crucial remedy since in jurisdictions without this possibility damages remain the only available remedy after this event.  

#### Damages

Damages are compensation paid to economic operators harmed by an infringement of the public procurement rules. The procedure and venue for bringing claims for damages depends on local legislation, which sets the filing rules, deadlines, requirements of proof, and extent of compensation (for example, the conditions under which tendering costs can be recovered). This remedy aims to compensate harmed economic operators. 

#### Alternative penalties

Alternative penalties are, for example, reducing the duration of the contract or fines for the contracting authority. (The difference between damages and fines is that the damage goes to the company, while the fine goes to the public budget.)

### Timeframe

Review proceedings may not be brought at any given point in time. Applicants for review normally have to initiate proceedings within certain time limits, counting from the moment in time when they learned about the alleged infringements of procurement law. 
 
#### Review period

This is a certian duration of time counting from the date of publication of Contract Notice and serves as a period to complain on published conditions: discriminatory technical, economic or financial specifications in the contract notice, tender documents or any other document relating to the contract award procedure.  

#### Standstill period

Contracting authorities are required to wait for a certain number of days between the contract award decision and the conclusion of the contract with the successful tenderer. This period allows rejected or not evaluated tenderers to challenge the contracting authoritys' decision not to award the contract to them, if they think that such a decision was unlawful, and therefore to prevent the contract from being concluded on the basis of an improper award decision.

## Approach

E.g., there is a procurement process divided into two lots with a set of the related conditions and requirements expressed as a criteria array (see [ESPD2OCDS](https://github.com/uStudioCompany/espd2ocds) for more details). A review period for complaints on conditions is scheduled and established initially.

```
{
  "tender": {
    "lots": [
      {
        "id": "lot-1"
      },
      {
        "id": "lot-2"
      }
    ],
    "reviewPeriod": {},
    "criteria": []
  }
}
```

### Complaints on conditions
Somewhere during submission period (within determined review period), a complaint on the conditions appears. This complaint containts: 
- complainant info
- high-level description of complaint
- responsible party
- initial schedule
- any documents related to this complaint 
- objections (basically what *complainant* wants *responsible party* to do. But this specific information provided in a bit structured way: not only free-text description but also):
 - machine-readable relations on kind and precise id of the object of complaint (in this case - lot)
 - classification according to Review Bodies' approach to systemize subjects of complaint (in Moldova, for example, there is a classifier of twelve main subjects of complaint)
 - set of requested actions to be taken to satisfy an objection

And as [Chris Smith mentioned](https://github.com/open-contracting/standard/issues/598#issuecomment-346432364) - this first appeal addressed to the Procuring Entity. And if not satisfied - to Review Body

```
{
  "reviewProceedings": {
    "complaints": [
      {
        "id": "complaint-1",
        "date": "2019-02-21T08:37:54Z",
        "status": "pending",
        "description": "Complaint on conditions",
        "complainant": {
          "id": "",
          "name": ""
        },
        "addressedTo": "procuringEntity",
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "description": "Precise date when complaint has to be either accepted or declined",
            "dueDate": "2019-02-24T08:00:00Z",
            "status": "scheduled"
          }
        ],
        "documents": [],
        "objections": [
          {
            "id": "objection-1-1",
            "description": "Objections' brief description",
            "relatesTo": "requirement",
            "relatedItem": "requirement-1",
            "classification": {
              "scheme": "",
              "id": "",
              "description": ""
            },
            "arguments": [
              { 
                "id": "argument-1-1-1",
                "description": "A substance of an argument",
                "evidences": [] 
              }
            ],
            "requestedRemedies": [
              {
                "id": "request-1-1-1",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Remove discriminatory specifications"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

As it is shown in an example, a number of service-data to be generated automatically and added to a data-set. Such as:
- complaint id
- registration date
- initial status of this complaint
- a milestone of decision on acceptance of this complaint for a farther review

Here and below the array of complaints included into the reviewProceedings object on the top of release because reviews and decisions against received complaints should be designed as a separate array instead of additional attributes of complaints item.
Thus, the requested body (in this case - Procuring Entity) has accepted a complaint for review. 

```
{
  "reviewProceedings": {
   "complaints": [
      {
        "id": "complaint-1",
        "date": "2019-02-21T08:37:54Z",
        "status": "active",
        "statusDetails": "accepted"
      }
    ],
    "reviews": [
      {
        "id": "review-1",
        "date": "2019-02-23T22:10:00Z",
        "status": "active"
      }
    ]
  }
}
```

In order to reflect its decision, PE reviews each objection and the arguments one by one with reflecting a result of such a review: 

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-1",
        "date": "2019-02-23T22:10:00Z",
        "status": "complete"
        "relatedComplaints": [
          "complaint-1"
        ],
        "milestones": [
          {
            "id": "",
            "type": "x_review",
            "dueDate": "2019-02-27T18:00:00Z",
            "dateMet": "2019-02-24T10:00:00Z"
          }
        ],
        "objectionResponses": [
          {
            "id": "response-1-1-1",
            "date": "2019-02-24T10:00:00Z",
            "relatedArgument": "argument-1-1-1",
            "decision": "declined",
            "justification": "Because we are disagree"
          }
        ],
         "decisions": [
          {
            "id": "1",
            "date": "",
            "type": "decree",
            "internalID": "",
            "decisionDate": "",
            "description": "",
            "resolution": "",
            "conclusions": [
              {
                "id": "conclusion-1-1",
                "relatedObjection": "objection-1-1",
                "status": "declined"
              }
            ],
            "documents": []
          }
        ]
      }
    ]
  }
}
```

By rejecting the only objection, the Procuring Entity rejects the entire complaint. The complaint goes to a negative status. 

```
{
  "reviewProceedings": {
    "complaints": [
      {
        "id": "complaint-1",
        "date": "2019-02-21T08:37:54Z",
        "status": "complete",
        "statusDetails": "declined"
      }
    ]
  }
}
```

The complainant goes to the Review Body. Indicating rejected complaint-1 as previousProceeding, complainant requests three actions instead of one:

```
{
  "reviewProceedings":{
    "complaints":[
      {
        "id": "complaint-2",
        "date": "2019-02-24",
        "status": "pending",
        "description": "Complaint brief description",
        "complainant": {},
        "addressedTo": "reviewBody",
        "previousProceeding": "complaint-1",
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "dueDate": "2019-02-27T08:00:00Z",
            "status": "scheduled"
          }
        ],
        "documents": [],
        "objections": [
          {
            "id": "objection-2-1",
            "description": "Objections' brief description",
            "relatesTo": "requirement",
            "relatedItem": "requirement-1",
            "classification": {
              "scheme": "",
              "id": "",
              "description": ""
            },
            "arguments": [
              { 
                "id": "argument-2-1-1",
                "description": "A substance of an argument",
                "evidences": [] 
              }
            ],
            "requestedRemedies": [
              {
                "id": "request-2-1-1",
                "description": "Suspension of the implementation of procurement process",
                "type": "interimMeasure",
                "source": "reviewBody"
              },
              {
                "id": "request-2-1-2",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Remove discriminatory specifications in the contract notice"
              },
              {
                "id": "request-2-1-3",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Extension of initial submission deadline up to  ..."
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Complainant requested Review Body:
- to suspend the procedure to avoid possible negative consequences
- to prescribe Procuring Entity to make changes according to objections' details
- to prescribe Procuring Entity to extend the initial submission deadline

Review Body accepts the complaint and publishes the relevant decision together with:
- conclusion on acceptance
- estimated schedule for this review process

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-2",
        "date": "2019-02-27T07:42:00Z",
        "status": "active"
        "relatedComplaints": [
          "complaint-2"
        ],
        "decisions": [
          {
            "id": "1",
            "date": "",
            "type": "acceptance",
            "internalID": "",
            "decisionDate": "",
            "description": "",
            "resolution": "",
            "conclusions": [
              {
                "id": "conclusion-1-1",
                "relatedObjection": "objection-2-1",
                "status": "accepted"
              }
            ],
            "documents": []
          }
        ],
        "milestones": [
          {
            "id": "1",
            "type": "x_initiation",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "met"
          },
          {
            "id": "2",
            "type": "x_review",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "scheduled",
            "documents": []
          },
          {
            "id": "3",
            "type": "x_hearings",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "scheduled"
          },
          {
            "id": "4",
            "type": "x_resolution",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "scheduled"
          },
          {
            "id": "5",
            "type": "x_decision",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "scheduled"
          }
        ],
        "parties": [
          {
            "id": "",
            "name": "",
            "roles": [
              "complainant"
            ],
            "persons": [
              {
                "id": "",
                "name": "",
                "identifier": {},
                "telephone": "",
                "email": "",
                "businessFunctions": []
              }
            ]
          },
          {
            "id": "",
            "name": "",
            "roles": [
              "responder"
            ],
            "persons": [
              {
                "id": "",
                "name": "",
                "identifier": {
                  "id": "",
                  "legalName": ""
                },
                "telephone": "",
                "email": "",
                "businessFunctions": []
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Review Body also requests Procuring Entity to respond on augments explained by a Complainant and PE responding with a justification against each argument of each objection of this specific complaint:

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-2",
        "date": "2019-02-27T07:42:00Z",
        "status": "active",
        "statusDetails": "accepted",
        "objectionResponses": [
          {
            "id": "response-2-1-1",
            "date": "2019-02-27T12:04:00Z",
            "relatedArgument": "request-2-1-2",
            "justification": ""
          }
        ]
      }
    ]
  }
}
```

Once responses are received from Procuring Entity, Review Body is able to is able to conclude a decision: 

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-2",
        "date": "",
        "status": "active",
        "statusDetails": "satisfied",
        "decisions": [
          {
            "id": "1",
            "date": "",
            "type": "decree",
            "internalID": "",
            "decisionDate": "",
            "title": "",
            "description": "",
            "resolution": "",
            "statements": [
              {
                "id": "",
                "relatedArgument": "argument-2-1-1",
                "status": "satisfied",
                "justification": ""
              }
            ],
            "conclusions": [
              {
                "id": "conclusion-1",
                "relatedObjection": "objection-2-1",
                "status": "satisfied"
              }
            ],
            "decrees": [
              {
                "id": "",
                "relatedRemedy": "request-2-1-1",
                "status": "satisfied"
              },
              {
                "id": "",
                "relatedRemedy": "request-2-1-2",
                "status": "satisfied"
              },
              {
                "id": "",
                "relatedRemedy": "request-2-1-3",
                "status": "satisfied"
              }
            ],
            "documents": []
          }
        ]
      }
    ]
  }
}
```

Complaint is fully satisfied. Procuring Entity obliged to implement actions prescribed by Review Body decision within indicated deadline (milestone.type:x_implementation). Such a measures have to be reported once implemented:

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-2",
        "date": "",
        "status": "complete",
        "statusDetails": "resolved",
        "decreeResponses":[
          {
            "id":"response",
            "relatedDecree":"",
            "isResolved":true,
            "description":"",
            "date":""
          }
        ]
      }
    ]
  }
}
```

### Complaints on decisions

Procurement process passed the tendering and now it is in the evaluation stage. As mentioned above - it's two lots inside. Let's assume that four offers were submitted by two tenderers: one bid from each tenderer for each lot from those two. So we have a picture:

|            | tenderer1       | tenderer2        |
| :--------- |:----------------| :----------------|
| lot-1      | award-1:pending | award-2:pending  |
| lot-2      | award-3:pending | award-4:pending  |

The Procuring Entity holds qualification and decides: 
1. to disqualify tenderer1 from lot-1
2. to award tenderer2 in lot-1
3. to award tenderer2 in lot-2 without qualification of tenderer-1 

So the picture now is following: 

|            | tenderer1                                  | tenderer2                             |
| :--------- |:-------------------------------------------| :-------------------------------------|
| lot-1      | award-1:pending/statusDetails:unsuccessful | award-2:pending/statusDetails:active  |
| lot-2      | award-3:pending                            | award-4:pending/statusDetails:active  |

> *statusDetails* used here instead of native *status* in order to indicate an intermediate state, which reflects a decision regarding this *award*. But till the end of standstill period, *award* itself is in status:pending (since this *award* might be cancelled due to the decision of Review Body)

And being affected, tenderer1 rises with a complaint: Tenderer1 wants to appeal on both: his disqualification from lot-1 and awarding of tenderer 2 in lot-2. 

> In different legislative frameworks it may be permitted or prohibited to appeal or review two cases within a single procedure. That's why the proposed approach designed in a way that will allow covering different needs and permissions:
>- united appeal covering several cases submitted by tenderer - one review procedure 
>- several appeals submitted by tenderer or tenderers - several review procudures
>- several appeals submitted by tenderer or tenderers - one review procudure

```
{
  "reviewProceedings":{
    "complaints":[
      {
        "id": "complaint-3",
        "date": "2019-03-10T11:00:00Z",
        "status": "pending",
        "description": "Complaint brief description",
        "complainant": {},
        "addressedTo": "reviewBody",
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "dueDate": "2019-03-13T08:00:00Z",
            "status":"scheduled"
          }
        ],
        "objections": [
          {
            "id": "objection-3-1",
            "description": "Objections' brief description",
            "relatesTo": "award",
            "relatedItem": "award-4",
            "classification": {
              "scheme": "",
              "id": "",
              "description": ""
            },
            "requestedRemedies": [
              {
                "id": "request-3-1-1",
                "description": "Suspension of the implementation of decision taken by CA",
                "type": "interimMeasure",
                "source": "reviewBody"
              },
              {
                "id": "request-3-1-2",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Annulment of an unlawful contracting decision"
              }
            ]
          },
          {
            "id": "objection-3-2",
            "description": "Objections' brief description",
            "relatesTo": "award",
            "relatedItem": "award-1",
            "classification": {
              "scheme": "",
              "id": "",
              "description": ""
            },
            "requestedRemedies": [
              {
                "id": "request-3-2-1",
                "description": "Suspension of the implementation of decision taken by CA",
                "type": "interimMeasure",
                "source": "reviewBody"
              },
              {
                "id": "request-3-2-2",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Annulment of an unlawful contracting decision"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

In another words, tenderer1 asked Review Body:
1. to suspend awarding and following contracting process for lot-1
2. to prescribe Procuring Entity to annul his decision to disqualify tenderer1 (which means that PE has to re-decide on tenderers1 offer. Not neсessary to award it but disqualify again on another reason if any) 
3. to suspend awarding and following contracting process for lot-2
4. to prescribe Procuring Entity to annul his decision to award tenderer2 (which means that PE has to re-decide on tenderers2 offer. Not necessary to disqualify it but reward again by first correcting the inconsistencies indicated in the objections) 

Review Body accepts this complaint, reviews it and decides  in a same way as in case of complaint on conditions.
