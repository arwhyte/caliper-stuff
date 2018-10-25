## IMS Question and Test Interoperability (QTI): Assessment, Section and Item Information Model Version 3.0

Source: imsqti_asiv3p0_infoModel_v1p0cfvd3.html

__Proposal__: we establish the following rules for the initial release of the Caliper QTI 
Profile that all _optional_ QTI _data_ properties (e.g., boolean, string, number) are excluded from 
Caliper QTI object representations. Optional object property values or arrays composed of objects
 would be targets for exclusion on a case-by-case basis.

### AssessmentTest
An `AssessmentTest` is comprised of one or more `TestPart` entities.

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required. |
| identifier | string | Required |
| title | string | Optional.  Is this property needed? |
| isPartOf | Entity | Optional |
| testPart | array | Optional |


```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentTest",
  "identifier": "",
  "title": "",
  "isPartOf": {
    "id": "",
    "type": "Entity"
  },
  "testPart": [{}]
}
```

### TestPart

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required  |
| isPartOf | `AssessmentTest` | Optional |
| assessmentSectionSelection | `AssessmentSectionSelection` | Optional |

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "TestPart",
  "identifier": "",
  "isPartOf": {
    "id": "IRI",
    "type": "AssessmentTest",
    "identifier": "",
    "isPartOf": {
      "id": "IRI",
      "type": "Entity [typically a Caliper DigitalResource]"
    }
  },
  "assessmentSectionSelection": [{
    "type": "AssessmentSectionSelection",
    "assessmentSection": {},
    "assessmentSectionRef": {}
  }]
}
```

### AssessmentSection

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required  |
| isPartOf | `TestPart` \| `AssessmentSection` | Optional |
| assessmentSectionSelection | `AssessmentSectionSelection` | Optional |

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentSection",
  "identifier": "",
  "title": "",
  "required": false,
  "isPartOf": {
    "id": "",
    "type": "TestPart | AssessmentSection",
    "identifier": "",
    "isPartOf": {}
  },
  "sectionPart": [{
    "id": "IRI",
    "type": "SectionPart",
    "assessmentItemRef": {},
    "assessmentSection": {},
    "assessmentSectionRef": {}
  }]
}
```

### AssessmentStimulus
TODO: Is modeling `AssessmentStimulus` required?

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required  |
| title | string | Optional.  Is this title needed? |
| label | string | Optional.  Is this title needed? |
| isPartOf | `AssessmentItem` | Optional |

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentStimulus",
  "identifier": "",
  "title": "",
  "label": "",
  "isPartOf": {}
}
```

### AssessmentItem

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required |
| isPartOf | TestPart \| AssessmentSection | Optional |

TODO: what properties can be removed from the example JSON-LD below?  What properties need to be 
added?

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentItem",
  "identifier": "",
  "title": "",
  "label": "",
  "language": "",
  "toolName": "",
  "toolVersion": "",
  "adaptive": false,
  "timeDependent": false,
  "isPartOf": {
      "id": "",
      "type": "TestPart | AssessmentSection",
      "identifier": "",
      "isPartOf": {}
    },
  "contextDeclaration": [{
    "id": "",
    "type": "ContextDeclaration",
    "identifier": "",
    "cardinality": "",
    "baseType": "",
    "defaultValue": {}
  }],
  "responseDeclaration": [{
    "id": "",
    "type": "ResponseDeclaration",
    "identifier": "",
    "cardinality": "",
    "baseType": "",
    "defaultValue": {},
    "correctResponse": {},
    "mapping": {},
    "areaMapping": {}
  }],
  "outcomeDeclaration": [{
    "id": "",
    "type": "OutcomeDeclaration",
    "identifier": "",
    "cardinality": "",
    "baseType": "",
    "view": {},
    "interpretation": "",
    "longInterpretation": "IRI",
    "normalMaximum": 100,
    "normalMinimum": 0,
    "masteryValue": 80,
    "externalScored": {},
    "variableIdentifierRef": {},
    "defaultValue": {},
    "lookupTable": {}
  }],
  "templateDeclaration": [{
    "id": "",
    "type": "TemplateDeclaration",
    "identifier": "",
    "cardinality": "",
    "baseType": "",
    "paramVariable": false,
    "mathVariable": false,
    "defaultValue": {}
  }],
  "assessmentStimulusRef": [{
    "id": "",
    "type": "AssessmentStimulusRef",
    "identifier": "",
    "href": ""
  }],
  "itemBody": {},
  "modalFeedback": {}
}
```

### AssessmentResult

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required |
| isPartOf | ? | Optional ?? |
| context | `Context` | Required |
| testResult | `TestResult` | Optional |
| itemResult | array | Optional |

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v2p2/context/",
  "id": "IRI",
  "type": "AssessmentResult",
  "context": {
    "id": "IRI",
    "type": "Context",
    "sourcedId": "",
    "sessionIdentifier": ""
  },
  "testResult": {
    "id": "IRI",
    "type": "TestResult",
    "identifier": "",
    "datestamp": "ISO 8601 DateTime",
    "itemVariable": [{}]
  },
  "itemResult": []
}
```

### TestResult
QUESTION?: can a `TestResult` include `outcomeVariable`, `responseVariable`, AND 
`templateVariable` properties (all three) or is it the case that only a single concrete instance of 
the abstract `itemVariable` property that is permitted to be expressed?

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required |
| _itemVariable_ | array | Optional. Concrete instance of abstract ItemVariable, ie., `OutcomeVariable`, `ResponseVariable`, `TemplateVariable`. |

```json
{
  "id": "IRI",
  "type": "TestResult",
  "identifier": "",
  "datestamp": "ISO 8601 DateTime",
  "outcomeVariable | responseVariable | templateVariable": [{}]
}
```

### ItemResult
QUESTION?: can a `ItemResult` include `outcomeVariable`, `responseVariable`, AND 
`templateVariable` properties (all three) or is it the case that only a single concrete instance of 
the abstract `itemVariable` property that is permitted to be expressed?


| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required |
| sequenceIndex | integer | Optional |
| datestamp | string | Required |
| sessionStatus | Enum | Required |
| candidateComment | string | Optional |
| _itemVariable_ | array | Optional. Concrete instance of abstract ItemVariable, ie., `OutcomeVariable`, `ResponseVariable`, `TemplateVariable`. |


```json
{
  "id": "IRI",
  "type": "ItemResult",
  "identifier": "",
  "sequenceIndex": 1,
  "datestamp": "ISO 8601 DateTime",
  "sessionStatus": "final | initial | pendingResponseProcessing | pendingSubmission",
  "candidateComment": "",
  "outcomeVariable | responseVariable | templateVariable": [{}]
}
```

### OutcomeVariable
TODO Optional data properties are targets for removal.

| Property | Type | Disposition |
| :------- | :--- | :---------- |
| id | IRI | Required |
| type | Term | Required |
| identifier | string | Required |
| cardinality | Enum | Required |
| baseType | Enum | Optional |
| view | `View` | Optional |
| interpretation | string | Optional |
| longInterpretation | URI | Optional |
| normalMaximum | Double | Optional |
| normalMinimum | Double | Optional |
| masteryValue | Double | Optional |
| value | array | Optional |

```json
{
  "id": "IRI",
  "type": "OutcomeVariable",
  "identifier": "",
  "cardinality": "multiple | ordered | record | single",
  "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
  "view": ["author | candidate | proctor | scorer | testConstructor | tutor", ""],
  "interpretation": "",
  "longInterpretation": "URI",
  "normalMaxiumum": 100.0,
  "normalMinimum": 10.0,
  "masteryValue": "90.0",
  "value": [{
    "id": "IRI",
    "type": "Value",
    "fieldIdentifier": "",
    "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
    "valueValue": "TODO RETHINK NAME OR VALUE EXPRESSED AS AN OBJECT"
  }]
}
```

### ResponseVariable

```json
{
  "id": "IRI",
  "type": "ResponseVariable",
  "identifier": "",
  "cardinality": "multiple | ordered | record | single",
  "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
  "choiceSequence": ["Identifier01", "Identifier02", "Identifier03"],
  "correctResponse": {
    "id": "IRI",
    "type": "CorrectResponse",
    "interpretation": "",
    "value": [{
      "id": "IRI",
      "type": "Value",
      "fieldIdentifier": "",
      "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
      "valueValue": "TODO RETHINK NAME"
    }]
  },
  "candidateResponse": {
    "id": "IRI",
    "type": "CandidateResponse",
    "value": [{
      "id": "IRI -> urn:uuid:<UUID>",
      "type": "Value",
      "fieldIdentifier": "",
      "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
      "valueValue": "TODO RETHINK NAME"
    }]
  }
}
```

### TemplateVariable

```json
{
  "id": "IRI ",
  "type": "TemplateVariable",
  "identifier": "",
  "cardinality": "multiple | ordered | record | single",
  "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
  "value": [{
    "id": "IRI",
    "type": "Value",
    "fieldIdentifier": "",
    "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
    "valueValue": "TODO RETHINK NAME"
  }]
}
```