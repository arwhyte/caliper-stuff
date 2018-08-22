# Proposal: Caliper QTI entity mixins

I’ve been rethinking the current approach of attempting to add coarse-grained Caliper “QTI” entities to the Caliper spec.   I now consider it a mistake to add entities that are nothing more than pale reflections of entities that are fully described in the QTI spec.  Such constructions end up satisfying no one.  I’ve mentioned my misgivings on the last couple of Caliper calls along with a proposal that I consider a better approach.

I propose that we do the following.  Instead of adding what I call faux QTI Entities to the Caliper vocabulary along with a few cherry-picked QTI properties I recommend that we embrace fully JSON-LD’s vocabulary mixin capabilities and express QTI Entities as JSON in all their glory and complexity.  Below you will find a QTI `AssessmentResult` expressed as a JSON-LD document.  I encountered only one issue in this bit of work, representing a QTI Value object’s value string property.

I believe this vocabulary “mixin” strategy is preferable to what we've attempted to model to date
 with respect to QTI.  Indeed, I believe Caliper should consider adopting this approach for all entities we might choose to represent that are more fully described in sibling specifications (e.g., LTI Deep Linking, Open Badges).   

More particularly, in the case of QTI, we do this: 

1. We create a Caliper QTI JSON-LD context that maps all relevant QTI terms to a namespaced QTI IRI in format prescribed in spec central.  In this way we can drop into a Caliper Event native QTI entities.  See III below which draws its terms from the QTI Results Reporting Information Model, v2p2.  This approach avoids mixing Caliper with QTI terms, avoiding term collisions, etc.

2. QTI entities (`Assessment`, `AssessmentPart`, `AssessmentSection`, `AssessmentResult`, etc.) that are 
described in a Caliper event will each reference the QTI remote context directly (i.e., “@context”: "https://purl.imsglobal.org/spec/qti/v2p2/context/“).  Doing so will override the Caliper Event context and protect QTI term mappings from being overwritten by Caliper mappings if a JSON-LD parser is used to transform the event.  See I and II below which provides a roughed-in Caliper `AssessmentItemEvent` that references two QTI entities and as well as mock up of an QTI `AssessmentResult` expressed as JSON-LD.

3. QTI entities expressed as JSON will be provisioned with @context, id and type properties (the latter two properties will be aliased as @id and @type in the Caliper QTI JSON-LD context).  No other Caliper terms will be added unless a need is determined to do so.  Otherwise, each QTI entity  will be provisioned with properties as defined in the relevant QTI spec document.  Each QTI property referenced will be mapped as a term in the Caliper QTI JSON-LD context.

4. In cases where the id property is considered artificial or unnecessary we can either 1) exclude it or 2) require implementors to generate a UUID for its value.

### I. Skeletal Caliper Event illustrating QTI vocabulary mixins

In the following skeletal example both the object of the interaction and the generated entity are
 both entities whose terms are mapped to QTI IRIs as described in the remote https://purl
 .imsglobal.org/spec/qti/v2p2/context/.  The QTI JSON objects are themselves modeled directly on 
 QTI XML representations.  All other terms are mapped to the Caliper remote context.

```
{
  "@context": "http://purl.imsglobal.org/ctx/caliper/v1p1",
  "id": "urn:uuid:e5891791-3d27-4df1-a272-091806a43dfb",
  "type": "AssessmentItemEvent",
  "actor": {
    "id": "https://example.edu/users/554433",
    "type": “Person”                                               <— Caliper Entity
  },
  "action": “Submitted",
  "object": {
    "@context": "https://purl.imsglobal.org/spec/qti/v2p2/context/",
    "id": “UUID",
    "type": “AssessmentItem”,                                      <— QTI Entity
    "identifer": "A QTI identifier”,
    . . . other QTI properties
  },
  "generated": {
    "@context": "https://purl.imsglobal.org/spec/qti/v2p2/context/",
    "id": "UUID",
    "type": “ItemResult”,                                           <— QTI Entity
    "identifer": "A QTI identifier”,  
     . . . other QTI properties
  },
  "eventTime": "2016-11-15T10:15:12.000Z",
  "edApp": {
    "id": "https://example.edu",
    "type": "SoftwareApplication”                                   <— A Caliper Entity
  },
  "group": {
    "id": "https://example.edu/terms/201601/courses/7/sections/1",
    "type": “CourseSection”                                         <— A Caliper Entity
  },
  "membership": {
    "id": "https://example.edu/terms/201601/courses/7/sections/1/rosters/1",
    "type": “Membership"                                            <— A Caliper Entity
  },
  "session": {
    "id": "https://example.edu/sessions/1f6442a482de72ea6ad134943812bff564a76259",
    "type": “Session”                                               <— A Caliper Entity

  }
}
```

### II. Caliper QTI AssessmentResult entity mock-up

Below is a mock up of a QTI `AssessmentResult` expressed as JSON-LD.  As noted above in cases 
where the id property is considered artificial or unnecessary we can either 1) exclude it or 2) 
require implementors to generate a UUID for its value.

```
{
  "@context": "https://purl.imsglobal.org/spec/qti/v2p2/context/",
  "id": "IRI -> urn:uuid:<UUID>",
  "type": "AssessmentResult",
  "context": {
    "id": "IRI -> urn:uuid:<UUID>",
    "type": "Context",
    "sourcedId": "",
    "sessionIdentifier": ""
  },
  "testResult": {
    "id": "IRI -> urn:uuid:<UUID>",
    "type": "TestResult",
    "identifier": "",
    "datestamp": "ISO 8601 DateTime",
    "outcomeVariable": [{
      "id": "IRI -> urn:uuid:<UUID>",
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
        "id": "IRI -> urn:uuid:<UUID>",
        "type": "Value",
        "fieldIdentifier": "",
        "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
        "valueValue": "TODO RETHINK NAME OR VALUE EXPRESSED AS AN OBJECT"
      }]
    }]
  },
  "itemResult": [{
    "id": "IRI -> urn:uuid:<UUID>",
    "type": "ItemResult",
    "identifier": "",
    "sequenceIndex": 1,
    "datestamp": "ISO 8601 DateTime",
    "sessionStatus": "final | initial | pendingResponseProcessing | pendingSubmission",
    "candidateComment": "",
    "outcomeVariable": [{
      "id": "IRI -> urn:uuid:<UUID>",
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
        "id": "IRI -> urn:uuid:<UUID>",
        "type": "Value",
        "fieldIdentifier": "",
        "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
        "valueValue": "TODO RETHINK NAME"
      }]
    }],
    "responseVariable": [{
      "id": "IRI -> urn:uuid:<UUID>",
      "type": "ResponseVariable",
      "identifier": "",
      "cardinality": "multiple | ordered | record | single",
      "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
      "choiceSequence": ["Identifier01", "Identifier02", "Identifier03"],
      "correctResponse": {
        "id": "IRI -> urn:uuid:<UUID>",
        "type": "CorrectResponse",
        "interpretation": "",
        "value": [{
          "id": "IRI -> urn:uuid:<UUID>",
          "type": "Value",
          "fieldIdentifier": "",
          "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
          "valueValue": "TODO RETHINK NAME"
        }]
      },
      "candidateResponse": {
        "id": "IRI -> urn:uuid:<UUID>",
        "type": "CandidateResponse",
        "value": [{
          "id": "IRI -> urn:uuid:<UUID>",
          "type": "Value",
          "fieldIdentifier": "",
          "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
          "valueValue": "TODO RETHINK NAME"
        }]
      }
    }],
    "templateVariable": [{
      "id": "IRI -> urn:uuid:<UUID>",
      "type": "TemplateVariable",
      "identifier": "",
      "cardinality": "multiple | ordered | record | single",
      "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
      "value": [{
        "id": "IRI -> urn:uuid:<UUID>",
        "type": "Value",
        "fieldIdentifier": "",
        "baseType": "boolean | directedPair | duration | file | float | identifier | integer | pair | point | string | uri",
        "valueValue": "TODO RETHINK NAME"
      }]
    }]
  }]
}
```

### III. Caliper QTI JSON-LD context (AssessmentResult-related terms only)

QTI features an extensive vocabulary.  The JSON-LD context below is partial in scope and only
maps terms to IRIs that are associated with a QTI `AssessmentResult`.  It's purpose is to 
illustrate how QTI terms would be described in a JSON-LD context document.  

Note: QTI entities, object properties, data properties and enumerated constants are included.

```
{
  "@context": {
    "id": "@id",
    "type": "@type",

    "qti": "https://purl.imsglobal.org/spec/qti/vocab#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",

    "AssessmentResult": "qti:AssessmentResult",
    "CandidateResponse": "qti:CandidateResponse",
    "CorrectResponse": "qti:CorrectResponse",
    "Context": "qti:Context",
    "ItemResult": "qti:ItemResult",
    "OutcomeVariable": "qti:OutcomeVariable",
    "ResponseVariable": "qti:ResponseVariable",
    "TemplateVariable": "qti:TemplateVariable",
    "TestResult": "qti:TestResult",
    "Value": "qti:Value",

    "candidateResponse": {"@id": "qti:candidateResponse", "@type": "@id"},
    "choiceSequence": {"@id": "qti:choiceSequence", "@type": "@id", "@container": "@list"},
    "context": {"@id": "qti:context", "@type": "@id"},
    "outcomeVariable": {"@id": "qti:outcomeVariable", "@type": "@id", "@container": "@list"},
    "responseVariable": {"@id": "qti:responseVariable", "@type": "@id", "@container": "@list"},
    "templateVariable": {"@id": "qti:templateVariable", "@type": "@id", "@container": "@list"},
    "testResult": {"@id": "qti:testResult", "@type": "@id"},
    "value": {"@id": "qti:value", "@type": "@id"},

    "baseType": {"@id": "qti:baseType", "@type": "@vocab"},
    "candidateComment": {"@id": "qti:candidateComment", "@type": "xsd:string"},
    "cardinality": {"@id": "caliper:cardinality", "@type": "@vocab"},
    "datestamp": {"@id": "qti:datestamp", "@type": "xsd:dateTime"},
    "fieldIdentifier": {"@id": "qti:fieldIdentifier", "@type": "xsd:string"},
    "identifier": {"@id": "qti:identifier", "@type": "xsd:string"},
    "interpretation": {"@id": "qti:interpretation", "@type": "xsd:string"},
    "longInterpretation": {"@id": "qti:longInterpretation", "@type": "xsd:string"},
    "masteryValue": {"@id": "qti:masteryValue", "@type": "xsd:decimal"},
    "normalMaxiumum": {"@id": "qti:normalMaxiumum", "@type": "xsd:decimal"},
    "normalMinimum": {"@id": "qti:normalMinimum", "@type": "xsd:decimal"},
    "sequenceIndex": {"@id": "qti:sequenceIndex", "@type": "xsd:nonNegativeInteger"},
    "sessionIdentifier": {"@id": "qti:sessionIdentifier", "@type": "xsd:string"},
    "sessionStatus": {"@id": "qti:sessionStatus", "@type": "@vocab"},
    "sourcedId": {"@id": "qti:sourcedId", "@type": "xsd:string"},
    "view": {"@id": "qti:view", "@type": "@vocab"},

    "valueValue": {"@id": "qti:valueValue", "@type": "xsd:string”},  <— Hack.  Need a field to hold
     the value if Value is a {}

    "multiple": "qti:multiple",
    "ordered": "qti:ordered",
    "record": "qti:record",
    "single": "qti:single",

    "boolean": "qti:boolean",
    "directedPair": "qti:directedPair",
    "duration": "qti:duration",
    "file": "qti:file",
    "float": "qti:float",
    "integer": "qti:integer",
    "pair": "qti:pair",
    "point": "qti:point",
    "string": "qti:string",
    "url": "qti:url",

    "author": "qti:author",
    "candidate": "qti:candidate",
    "proctor": "qti:proctor",
    "scorer": "qti:scorer",
    "testConstructor": "qti:testConstructor",
    "tutor": "qti:tutor",

    "final": "qti:final",
    "initial": "qti:initial",
    "pendingResponseProcessing": "qti:pendingResponseProcessing",
    "pendingSubmission": "qti:pendingSubmission"
  }
}
```