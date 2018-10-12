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
  "isPartOf": {
    "id": "",
    "type": "Entity"
  },
  "testPart": [{}]
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
An `AssessmentSection` may itself comprise one or more `AssementSection` entities via an optional `SectionPart` array.

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

### AssessmentStimulus

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
