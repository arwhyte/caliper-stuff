## IMS Question and Test Interoperability (QTI): Assessment, Section and Item Information Model Version 3.0

Source: imsqti_asiv3p0_infoModel_v1p0cfvd3.html

### AssessmentTest
An `AssessmentTest` is comprised of one or more `TestPart` entities.

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentTest",
  "identifier": "",
  "title": "",
  "toolVersion": "",
  "outcomeDeclaration": [{
    "id": "IRI",
    "type": "OutcomeDeclaration",
    "identifier": "",
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
  "timelimits": {
    "type": "TimeLimits",
    "minTime": 3600,
    "maxTime": 7200,
    "allowLateSubmission": true
  },
  "stylesheet": [{
    "id": "",
    "type": "Stylesheet",
    "href": "URI",
    "type | mediaType": "NAME CONFLICT; value=Media type",
    "media": "",
    "title": ""
  }],
  "rubricBlock": [{
    "id": "",
    "type": "TestRubricBlock",
    "stylesheet": [],
    "contentBody": {
      "id": "",
      "type": "TestRubricBlockContentBody",
      "contentModel": {
        "id": "",
        "type": "TestRubricBlockContentModel",
        "flowControlModel": {},
        "include": {},
        "math": {}
      }
    },
    "catalogInfo": {
      "type": "CatalogInfo",
      "qti-catalog": {
        "id": "",
        "type": "Catalog",
        "qti-card": [{
          "id": "",
          "type": "Card",
          "cardSelection": {
            "id": "",
            "type": "CardSelection",
            "qti-html": {},
            "qti-file-href": {}
          },
          "qti-card-entry": {
            "id": "",
            "type": "CardEntry",
            "language": "",
            "default": true,
            "dataExtension": [],
            "qti-html-content": {},
            "qti-file-href": []
          }
        }]
      }
    }
  }],
  "testPart": [{}],
  "outcomeProcessing": {},
  "testFeedback": {}
}
```

### TestPart
A `TestPart` contains one or more `AssessmentSection` entities.

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "TestPart",
  "identifier": "",
  "navigationMode": {
    "type": "NavigationMode",
    "linear": "",
    "nonlinear": ""
  },
  "submissionMode": {
    "type": "SubmissionMode",
    "individual": "",
    "simultaneous": ""
  },
  "preCondition": [{
    "type": "LogicSingle",
    "logic": {}
  }],
  "branchRule": [{
    "type": "BranchRule",
    "target": {},
    "logic": {}
  }],
  "itemSessionControl": {
    "type": "itemSessionControl",
    "maxAttempts": 1,
    "showFeedback": false,
    "allowReview": true,
    "showSolution": false,
    "allowComment": false,
    "allowSkipping": true,
    "validateResponses": false
  },
  "timeLimits": {
    "type": "TimeLimits",
    "minTime": 3600,
    "maxTime": 7200,
    "allowLateSubmission": false
  },
  "assessmentSectionSelection": [{
    "type": "AssessmentSectionSelection",
    "assessmentSection": {},
    "assessmentSectionRef": {}
  }],
  "testFeedback": [{
    "type": "TestFeedback",
    "identifier": "",
    "title": "",
    "access": {},
    "outcomeIdentifier": "",
    "showHide": {},
    "stylesheet": [],
    "contentBody": {},
    "catalogInfo": []
  }]
}
```

### AssessmentSection
An `AssessmentSection` may itself comprise one or more `AssementSection` entities via an optional `SectionPart` array.

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentSection",
  "identifier": "",
  "title": "",
  "required": false,
  "fixed": false,
  "visible": true,
  "keepTogether": true,
  "preCondition": [{
     "type": "LogicSingle",
      "logic": {}
  }],
  "branchRule": [{
     "type": "BranchRule",
     "target": {},
     "logic": {}
  }],
  "itemSessionControl": {
     "type": "itemSessionControl",
     "maxAttempts": 1,
     "showFeedback": false,
     "allowReview": true,
     "showSolution": false,
     "allowComment": false,
     "allowSkipping": true,
     "validateResponses": false
  },
  "timeLimits": {
     "type": "TimeLimits",
     "minTime": 3600,
     "maxTime": 7200,
     "allowLateSubmission": false
  },
  "adaptive": {},
  "rubricBlock": [],
  "sectionPart": [{
     "id": "",
     "type": "SectionPart",
     "assessmentItemRef": {},
     "assessmentSection": {},
     "assessmentSectionRef": {}
    }]
}
```

### AssessmentItem

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
  "templateProcessing": {
	"type": "TemplateRuleGroup",
	"templateRuleGroup": []
  },
  "assessmentStimulusRef": [{
	"id": "",
	"type": "AssessmentStimulusRef",
	"identifier": "",
	"href": ""
  }],
  "companionMaterialsInfo": {},
  "stylesheet": [],
  "itemBody": {},
  "catalogInfo": [],
  "responseProcessing": {
	"id": "",
	"type": "",
	"template": "URI",
	"templateLocation": "URI",
	"responseRuleGroup": []
  },
  "modalFeedback": {}
}
```

### AssessmentStimulus

```json
{
  "@context": "https://purl.imsglobal.org/spec/qti/v3p0/context/",
  "id": "IRI",
  "type": "AssessmentStimulus",
  "identifier": "",
  "title": "",
  "label": "",
  "language": "",
  "toolName": "",
  "toolVersion": "",
  "stylesheet": [],
  "stimulusBody": {},
  "catalogInfo": []
}
```
