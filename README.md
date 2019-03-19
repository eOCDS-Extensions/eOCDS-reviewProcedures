# ReviewProcedures

Extension to cover the legal actions which allow EOs to request the enforcement of public procurement regulations and their rights under those regulations in cases where CAs fail to comply with the legal framework for public procurement.

## Backgound

Detailed provisions relating to the types of remedy available are generally subject to local law. But generally speaking,  four types of remedies are available: 
- Interim measures
- Set aside of decisions 
- Annulment of a Concluded Contract
- Damages

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

### Timeframe

Review proceedings may not be brought at any given point in time. Applicants for review normally have to initiate proceedings within certain time limits, counting from the moment in time when they learned about the alleged infringements of procurement law. 
 
#### Review period

This is a certian duration of time counting from the date of publication of Contract Notice and serves as a period to complain on published conditions: discriminatory technical, economic or financial specifications in the contract notice, tender documents or any other document relating to the contract award procedure.  

#### Standstill period

Contracting authorities are required to wait for a certain number of days between the contract award decision and the conclusion of the contract with the successful tenderer. This period allows rejected or not evaluated tenderers to challenge the contracting authoritys' decision not to award the contract to them, if they think that such a decision was unlawful, and therefore to prevent the contract from being concluded on the basis of an improper award decision.

## Approach

So we have a procurement process divided into two lots. The review period for complaints on conditions is scheduled and established initially.

### Complaints on conditions

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
    "reviewPeriod": {}
  }
}
```
Somewhere during submission period (within determined review period), a complaint on the conditions appears. This complaint containts: 
- complainer info
- responsible party
- initial schedule
- documents 
- objections (basically what *complainer* wants *responsible party* to do. But this specific information provided in a bit structured way: not only free-text description but also):
 - machine-readable relations on kind and precise id of the object of complaint (in this case - lot)
 - classification according to Review Bodies' approach to systemize subjects of complaint (in Moldova, for example, there is a classifier of twelve main subjects of complaint)
 - set of requested actions to be taken to satisfy an objection

And as [Chris Smith mentioned](https://github.com/open-contracting/standard/issues/598#issuecomment-346432364) - this first appeal addressed to the Procuring Entity. It works in the same way in Ukraine: first, you have to ask Procuring Entity and if not satisfied - second kick - Review Body

```
{
  "reviewProceedings": {
    "complaints": [
      {
        "id": "complaint-1",
        "date": "2019-02-21T08:37:54Z",
        "status": "pending",
        "description": "Complaint on conditions",
        "complainer": {},
        "adressedTo": "procuringEntity",
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "description": "Precise date when complaint has to be either accepted or declined",
            "dueDate": "2019-02-24T08:00:00Z",
            "status": "scheduled"
          },
          {
            "id": "2",
            "type": "x_review",
            "description": "Indicative date when complaint has to be reviewed",
            "dueDate": "2019-02-28T18:00:00Z",
            "status": "scheduled"
          }
        ],
        "documents": [],
        "objections": [
          {
            "id": "objection-1-1",
            "description": "Objections' brief description",
            "status": "pending",
            "relatesTo": "lot",
            "relatedItem": "lot-1",
            "classification": {
              "scheme": "MD-ANSC",
              "id": "DA-3 ",
              "description": "Discriminatory technical, economic or financial specifications"
            },
            "requestedActions": [
              {
                "id": "request-1-1-1",
                "type": "setAside",
                "source": "procuringEntity",
                "description": "Remove discriminatory technical, economic or financial specifications"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Here and below array of *complaints* included into the *review Proceedings* object on the top of *release* because reviews and decisions against received *complaints* should be designed as a separate *array* instead of additional attribetues of *complaints* item:

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-1",
        "date": "2019-02-23T22:10:00Z",
        "status": "complete",
        "statusDetails": "negative",
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
            "requestedAction": "request-1-1-1",
            "decision": "rejected",
            "justification": "Because we are desagree"
          }
        ],
        "documents": []
      }
    ]
  }
}
```
As we can see, by rejecting the only requested action, the Procuring Entity rejected the complaint. First complaint goes to *status:rejected* and *complainer* goes to the second kick: Review Body. Indicating rejected *complaint-1* as *previous proceeding*, *complainer* requests three *actions* instead of one: 

```
{
  "reviewProceedings":{
    "complaints:[
      {
        "id": "complaint-2",
        "date": "2019-02-24T11:00:00Z",
        "status": "pending"
        "description": "Complaint brief description",
        "complainer": {},
        "adressedTo": "reviewBody",
        "previousProceeding": "complaint-1"
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "dueDate": "2019-02-27T08:00:00Z",
            "status": "scheduled"
          },
          {
            "id": "2",
            "type": "x_review",
            "dueDate": "2019-03-05T18:00:00Z",
            "status": "scheduled"
          }
        ],
        "documents": [],
        "objections": [
          {
            "id": "objection-2-1",
            "description": "Objections' brief description",
            "status": "pending",
            "relatesTo": "lot",
            "relatedItem": "lot-1",
            "classification": {
              "scheme": "MD-ANSC",
              "id": "DA-3",
              "description": "Discriminatory technical, economic or financial specifications in the contract notice"
            },
            "requestedActions": [
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
                "description": "Remove discriminatory technical, economic or financial specifications in the contract notice"
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
Complainer requested Review Body:
1. to suspend the procedure to avoid possible negative consequences
2. to prescribe *Procuring Entity* to make changes according to objections' details
3. to prescribe *Procuring Entity* to extend the initial submission deadline

Review Body accepts the complaint and publishes the decision:

```
{
  "reviewProceedings": {
    "reviews": [
      {
        "id": "review-2",
        "date": "2019-02-27T07:42:00Z",
        "status": "complete",
        "statusDetails": "positive",
        "relatedComplaints":[ 
          "complaint-2"
        ],
        "objectionResponses": [
          {
            "id": "response-2-1-1",
            "date": "2019-02-27T12:02:00Z",
            "requestedAction": "request-2-1-1",
            "decision": "satisfied",
            "justification": ""
          },
          {
            "id": "response-2-1-2",
            "date": "2019-02-27T12:04:00Z",
            "requestedAction": "request-2-1-2",
            "decision": "satisfied",
            "justification": ""
          },
          {
            "id": "response-2-1-3",
            "date": "2019-02-27T12:10:00Z",
            "requestedAction": "request-2-1-3",
            "decision": "satisfied",
            "justification": ""
          }
        ],
        "documents": [],
        "milestones": [
          {
            "id": "1",
            "type": "x_review",
            "dueDate": "2019-03-05T18:00:00Z",
            "dateMet": "2019-02-27T12:10:00Z"
          },
          {
            "id": "2",
            "type": "x_implementation",
            "dueDate": "2019-03-01T18:00:00Z"
          }
        ]
      }
    ]
  }
}
```

Complaint is fully satisfied. Procuring Entity obliged to implement actions prescribed by Review Body decision within indicated deadline (*milestone.type:x_implementation*)

> An issue here: [**How to arrange a decision implementation steps**](https://github.com/PaulBoroday/eOCDS-reviewProcedures/issues/1)

### Complaints on decisions

Procurement process passed the submission and now it is on evaluation stage. As mentioned above - it's two lots inside. 
Lets assume that four offers were submitted by two tenderers: one bid from each tenderer for each lots from those two. So we have a picture:

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
>- sevaral appeals submitted by tenderer or tenderers - sevaral review procudures
>- sevaral appeals submitted by tenderer or tenderers - one review procudure

> An issue here - [**United appeal covering several cases submitted by tenderer - splitting into several review procedures. Does it makes sense?**](https://github.com/PaulBoroday/eOCDS-reviewProcedures/issues/2)

```
{
  "reviewProceedings":{
    "complaints:[
      {
        "id": "complaint-3",
        "date": "2019-03-10T11:00:00Z",
        "status": "",
        "description": "Complaint brief description",
        "complainer": {},
        "adressedTo": "reviewBody",
        "milestones": [
          {
            "id": "1",
            "type": "x_acceptance",
            "dueDate": "2019-03-13T08:00:00Z",
            "status":"scheduled"
          },
          {
            "id": "2",
            "type": "x_review",
            "dueDate": "2019-03-17T18:00:00Z",
            "status":"scheduled"
          }
        ],
        "documents": [],
        "objections": [
          {
            "id": "objection-3-1",
            "description": "Objections' brief description",
            "status": "pending",
            "relatesTo": "award",
            "relatedItem": "award-4",
            "classification": {
              "scheme": "MD-ANSC",
              "id": "RP-3 ",
              "description": "Acceptance by the CA of offers of other non-compliant or unacceptable participants"
            },
            "requestedActions": [
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
            "status": "pending",
            "relatesTo": "award",
            "relatedItem": "award-1",
            "classification": {
              "scheme": "MD-ANSC",
              "id": "RP-2",
              "description": "Rejection of the submitted offer as inappropriate or unacceptable"
            },
            "requestedActions": [
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

Review Body accepts this appeal and decides to satisfy it partially:

```
{
   "reviewProceedings": {
    "reviews": [
      {
        "id": "review-3",
        "date": "2019-03-13T07:50:00Z",
        "status": "complete",
        "statusDetails": "partialyPositive",
        "relatedComplaints": [
          "complaint-3"
        ],
        "objectionResponses": [
          {
            "id": "response-3-1-1",
            "date": "2019-03-17T14:00:00Z",
            "requestedAction": "request-3-1-1",
            "decision": "satisfied",
            "justification": ""
          },
          {
            "id": "response-3-1-2",
            "date": "2019-03-17T14:25:00Z",
            "requestedAction": "request-3-1-2",
            "decision": "satisfied",
            "justification": ""
          },
          {
            "id": "response-3-2-1",
            "date": "2019-03-17T14:58:00Z",
            "requestedAction": "request-3-2-1",
            "decision": "rejected",
            "justification": ""
          },
          {
            "id": "response-3-2-2",
            "date": "2019-03-17T15:00:00Z",
            "requestedAction": "request-3-2-2",
            "decision": "rejected",
            "justification": ""
          }
        ],
        "documents": [],
        "milestones": [
          {
            "id": "1",
            "type": "x_review",
            "dueDate": "2019-03-17T18:00:00Z",
            "dateMet": "2019-03-17T15:00:00Z"
          },
          {
            "id": "2",
            "type": "x_implementation",
            "dueDate": "2019-03-19T18:00:00Z"
          }
        ]
      }
    ]
  }
}
```

