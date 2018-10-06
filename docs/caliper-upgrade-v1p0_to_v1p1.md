# Caliper 1.0 to 1.1 Upgrade Notes

This document focuses on the steps required to updating _existing_ Caliper 1.0 event streams to Caliper 1.1.

## 1.0 Terminology

<a name="iriDef"></a>__IRI__: The Internationalized Resource Identifier (IRI) extends the Uniform Resource Identifier ([URI](#uriDef)) scheme by using characters drawn from the Universal character set rather than US-ASCII per [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt).  [Linked Data](#linkedData) rely on IRIs to refer to most nodes and properties.

<a name="iso8601Def"></a>__ISO 8601__: Caliper data and time values are formatted per ISO 8601 with the addition of millisecond precision.  The format is yyyy-MM-ddTHH:mm:ss.SSSZ where 'T' separates the date from the time while 'Z' indicates that the time is set to UTC.

<a name="termDef"></a>__Term__: a word or short expression that expands to an [IRI](#iriDef) when mapped to a JSON-LD [context](#contextDef) document. Terms are employed by Caliper as `type` property string values in order to distinguish between various JSON representations of entities and events defined by the Caliper information model.

<a name="urnDef"></a>__URN__: A Uniform Resource Name ([URN](#urnDef)) is a type of [URI](#uriDef) that provides a persistent identifier for a resource that is bound to a defined namespace.  Unlike a [URL](#urlDef) a [URN](#urnDef) is location-independent and provides no means of accessing a representation of the named resource.

<a name="uuidDef"></a>__UUID__: a 128-bit identifier that does not require a registration authority to assure uniqueness.  However, absolute uniqueness is not guaranteed although the collision probability is considered extremely low. Caliper recommends use of randomly or pseudo-randomly generated version 4 UUIDs.  Each Caliper [Event](#event) MUST be assigned a UUID that is expressed as a [URN](#urnDef) using the form `urn:uuid:<UUID>` as described in [RFC 4122](#rfc4122).

## 2.0 Caliper 1.1 JSON-LD Context
The IMS-hosted remote [Caliper JSON-LD Context](http://purl.imsglobal.org/ctx/caliper/v1p1) document that maps Caliper terms to IRIs was rewritten in its entirety for 1.1.

Each Caliper `Event` emitted by an event producer MUST include a `@context` property that references the Caliper 1.1 JSON-LD Context IRI. A Caliper JSON-LD document governed by a single "top-level" context is limited to referencing the [Caliper JSON-LD Context](http://purl.imsglobal.org/ctx/caliper/v1p1) only:

```
{
  "@context": "http://purl.imsglobal.org/ctx/caliper/v1p1",
  "id": "urn:uuid:3a648e68-f00d-4c08-aa59-8738e1884f2c",
  "type": "Event",
  "actor": {
    "id": "https://example.edu/users/554433",
    "type": "Person"
  },
  "action": "Created",
  "object": {
    "id": "https://example.edu/terms/201801/courses/7/sections/1/resources/123",
    "type": "Document"
  },
  "eventTime": "2018-11-15T10:15:00.000Z"
}
```

 Multiple "top-level" contexts may be combined by using an array. The [Caliper JSON-LD Context](http://purl.imsglobal.org/ctx/caliper/v1p1) MUST always be listed as the last element in the array since JSON-LD parsers employ a [most-recently-defined-wins mechanism](https://w3c.github.io/json-ld-syntax/#advanced-context-usage) when processing duplicate context_type terms in order to ensure that Caliper terms retain their primacy.

```
{
  "@context": ["https://schema.org/docs/jsonldcontext.json", "http://purl.imsglobal.org/ctx/caliper/v1p1"]
  . . .
}
```

The rules governing JSON-LD `@context` keyword usage are described in more detail in the Caliper Analytics® Specification, version 1.1, [section 4.1](https://github.com/IMSGlobal/caliper-spec/blob/master/caliper-spec.md#41-json-ld-context).

If you use a JSON_LD parser to transform Caliper events you should consider caching the referenced Context document(s) locally as [recommended](https://json-ld.org/spec/latest/json-ld-api-best-practices/#cache-context) by the JSON-LD community.

## 3.0 Event model changes

### 3.1 Event property changes
The following property additions and deprecations apply to all `Event` types.

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| id | UUID | Required | Each [Event](#event) MUST be provisioned with a [UUID](#uuidDef).  The UUID MUST be expressed as a [URN](#urnDef) using the form `urn:uuid:<UUID>` per [RFC 4122](#rfc4122).  A version 4 [UUID](#uuidDef) is recommended. |
| type | Term | Required | Replaces previous use of the [JSON-LD](#jsonldDef) `@type` keyword. The string value MUST be set to the corresponding [Term](#termDef) rather than the full [IRI](#iriDef), e.g. *MediaEvent*. |
| ~~@type~~ | [IRI](#iriDef) | Deprecated | Replaced by `type`.  |
| action | Term | Required | The string value MUST be set to the corresponding [Term](#termDef) rather than the full [IRI](#iriDef), e.g. *Started*. |
| referrer | [Entity](#entity) &#124; [IRI](#iriDef) | Optional  | An [Entity](#entity) that represents the referring context. A [SoftwareApplication](#softwareApplication) or [DigitalResource](#digitalResource) will typically constitute the referring context.  The `referrer` value MUST be expressed either as an object or as a string corresponding to the referrer's [IRI](#iriDef). In the case of [NavigationEvent](#navigationEvent) `referrer` supersedes the deprecated `navigatedFrom` property.  |
| session | [Session](#session) &#124; [IRI](#iriDef) | Optional | The current user [Session](#session).  The `session` value MUST be expressed either as an object or as a string corresponding to the session's [IRI](#iriDef). |
| federatedSession | [LtiSession](#ltiSession) &#124; [IRI](#iriDef) | Optional | Caliper 1.1 changed the value type to [LtiSession](#ltiSession), a sub type of [Session](#session) that includes an LTI-related `messageParameters` property. If the [Event](#event) occurs within the context of an [LTI](#ltiDef) tool launch, the actor's tool consumer [LtiSession](#ltiSession) MAY be referenced.  The `federatedSession` value MUST be expressed either as an object or as a string corresponding to the federatedSession's [IRI](#iriDef).  |
| extensions | Object | Optional | A map of additional attributes not defined by the model MAY be specified for a more concise representation of the [Event](#event). |

### 3.2 NavigationEvent property changes
The Caliper 1.0 `NavigationEvent` included a `navigatedFrom` property. It has been deprecated in favor of `referrer` (see above).

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| ~~navigatedFrom~~ | [DigitalResource](#digitalResource), [SoftwareApplication](#softwareApplication) | Deprecated | Replaced by `referrer`. |

## 4.0 Entity model changes

### 4.1 Entity property changes
The following property additions and deprecations apply to all `Entity` types.

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| id | [IRI](#iriDef) | Required  | A valid [IRI](#iriDef) MUST be specified. The [IRI](#iriDef) MUST be unique and persistent. The [IRI](#iriDef) SHOULD also be dereferenceable, i.e., capable of returning a representation of the resource. A [URI](#uriDef) employing the [URN](#urnDef) scheme MAY be provided in cases where a [Linked Data](#linkedDataDef) friendly HTTP URI is either unavailable or inappropriate. |
| ~~@id~~ | [IRI](#iriDef) | Deprecated  | Replaced by `id`. |
| type  | [Term](#termDef) | Required | A string value corresponding to the [Term](#termDef) defined for the [Entity](#entity) in the external IMS Caliper JSON-LD [context](http://purl.imsglobal.org/ctx/caliper/v1p1) document.  For a generic [Entity](#entity) set the `type` value to the term *Entity*.  If a subtype of [Entity](#entity) is created, set the type to the [Term](#termDef) corresponding to the subtype utilized, e.g., *VideoObject*. |
| ~~@type~~ | [IRI](#iriDef)  | Deprecated | Replaced by `type`. |

### 4.2 DigitalResource property changes
The following property additions and deprecations apply to all `DigitalResource` types, including `MediaObject`, `AudioObject`, `ImageObject`, and `VideoObject`.

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| ~~alignedLearningObjective~~ | Array | Deprecated | Replaced by `LearningObjectives`. |
| creators | Array | Optional | An ordered collection of [Agent](#agent) entities, typically of type [Person](#person), that are responsible for bringing resource into being.  Each array item MUST be expressed either as an object or as a string corresponding to the item's [IRI](#iriDef). |
| learningObjectives | Array | Optional | Replaces the deprecated `alignedLearningObjective`. An ordered collection of one or more [LearningObjective](#learningobjective) entities that describe what a learner is expected to comprehend or accomplish after engaging with the resource.  Each array item MUST be expressed either as an object or as a string corresponding to the item's [IRI](#iriDef). |
| mediaType | string | Optional | A string value drawn from the list of [IANA](https://www.iana.org/assignments/media-types/media-types.xhtml) approved media types and subtypes that identifies the file format of the resource. |
| objectType | string | Deprecated | Use `type`. |

### 4.3 MediaLocation property changes
If a `MediaLocation.currentTime` value is set it MUST take the form of an ISO 8601 formatted duration string set to UTC.

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| currentTime | Duration | Optional | A time interval or duration that represents the current playback position measured from the beginning of an [AudioObject](#audioObject) or [VideoObject](#videoObject).  If a currentTime is specified the value MUST conform to the ISO 8601 duration format. |

### 4.4 Session property changes

| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| ~~actor~~ | [Person](#person) | Deprecated | Replaced by `user`. |
| user | [Person](#person) | Optional | The [Person](#person) who initiated the [Session](#session). |

### 4.5 SoftwareApplication property changes
| Property | Type | Disposition | Description |
| :------ | :-----| :----- | :---------- |
| version | string | new | Optional | A string value that designates the current form or version of the SoftwareApplication. |

### 4.6 Date/time and duration property values
The Caliper 1.0 Implementation Guide describes date/time and duration value types inconsistently.  In certain cases the values type is described as a "ISO-8601 timestamp"; in other cases as "long". The 1.0 `duration` data type is described as "xsd:duration".

Caliper 1.1 fully embraces the [ISO 8601](#iso8601Def) standard for representing dates and times for all date/time and duration property values.  Caliper 1.1 requires that date/time values MUST be expressed as an ISO 8601 date and time value expressed with millisecond precision. The value MUST be expressed using the format YYYY-MM-DDTHH:mm:ss.SSSZ set to UTC with no offset specified.

If a `duration` time interval is specified the value MUST conform to the ISO 8601 duration format.

In the case of a [MediaEvent](https://github.com/IMSGlobal/caliper-spec/blob/master/caliper-spec.md#mediaEvent) if the `object` of the interaction is an `AudioObject` or `VideoObject`, a `MediaLocation` SHOULD be specified as the `target` value in order to provide the `currentTime` in the audio or video stream that marks the action. If the currentTime is specified, the value MUST be an ISO 8601 formatted duration, e.g., "PT30M54S".

## 5.0 Event and Entity "thinning"
A Caliper 1.0 `Event` expressed as a JSON-LD document exhibits the following characteristics:

* Each `Entity` described therein as a JSON object is required to include a JSON-LD `@context` property even if the value duplicates the enclosing `Event` context (JSON-LD inheritance rules ignored).
* All optional `Event` and `Entity` properties must be referenced, even if the value is set to blank (''), empty (\[\]) or null.
* Caliper terms assigned as property values are expressed as an IRI.

Caliper 1.1 event and entity describe JSON-LD documents are considerably "thinner" than their Caliper 1.0 forebears. The thinning rules for Caliper 1.1 are as follows:

* If an `Event` includes entities with a `@context` value that duplicates the `Event` `@context` property value, the redundant `@context` properties are to be removed prior to serialization (JSON-LD context inheritance rules apply).
* All `Event` and `Entity` optional properties with a value set to blank (''), empty (\[\]) or null are to be removed prior to serialization.
* An `Entity` referenced by a Caliper `Event` can be expessed either as a JSON object or as a JSON string that corresponds to the object's IRI.
* Caliper 1.1 terms assigned as property values MUST be expressed using the Context property key _not_ the IRI value, e.g.,
  - "type": "Person"
  - "action": "Started"
  - "roles": ["Learner"]
  - "status": "Active"

The Caliper Technical Working Group provides Caliper 1.1 reference implementation libraries written in Java, Javascript, Python, PHP, Ruby, and .Net.  Each library provides thinning capabilities.  For example, the caliper-js 1.1 sensor  [clientUtils.js](https://github.com/IMSGlobal/caliper-js/blob/master/src/clients/clientUtils.js) file includes a replacer function that thins events and entities as part of the serialization process.

Examples of Caliper 1.0 and 1.1 events that illustrate the move to a more compact representation of Caliper events and entities are included below.

## 6.0 Caliper 1.1 Envelope
Caliper 1.1 `Event` and `Entity` data MUST be transmitted inside a Caliper `Envelope`, a purpose-built JSON data structure that includes metadata about the emitting `Sensor` and the data payload.

The Caliper 1.1 [Envelope](#envelope) includes a required `dataVersion` property, the string value for which MUST be set to the Caliper JSON-LD Context IRI:

```
{
	"sensor": "https://example.edu/sensors/1",
	"sendTime": "2018-11-15T11:05:01.000Z",
	"dataVersion": "http://purl.imsglobal.org/ctx/caliper/v1p1",
	"data": [...]
}

```

## 7.0 Caliper 1.1 MediaEvent
https://github.com/IMSGlobal/caliper-spec/blob/master/caliper-spec.md#mediaEvent

## Actions

| Event | New Actions | Deprecated Actions |
| :------ | :---------- | :----------------- |
|MediaEvent | Restarted | Rewound |

## Non-standard actions
The action "Still Watching" is not part of the Caliper 1.0 or 1.1 set of approved actions.  This Kaltura extension should be . . . .

```
"@context": ["https://corp.kaltura.com/caliper/ctx/v1p1", "http://purl.imsglobal.org/ctx/caliper/v1p1"]
```

The Kaltura JSON-LD Context would resemble the following document:

```
{
  "@context": {
    "kaltura": "https://corp.kaltura.com/caliper/vocab#",
    "Still Watching": "kaltura:Still%20Watching"
  }
}
```

### JSON-LD examples

#### Caliper 1.0 MediaEvent (resumed)
```
{
  "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
  "@type": "http://purl.imsglobal.org/caliper/v1/MediaEvent",
  "actor": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/user/arwhyte",
    "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
    "@type": "http://purl.imsglobal.org/caliper/v1/lis/Person",
    "name": "Anthony Whyte",
    "description": null,
    "extensions": {},
    "dateCreated": "2018-09-04T23:30:38.000Z",
    "dateModified": "2018-11-10T14:13:35.000Z"
  },
  "action": "http://purl.imsglobal.org/vocab/caliper/v1/action#Resumed",
  "object": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/media/1_u5xer7qs",
    "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
    "@type": "http://purl.imsglobal.org/caliper/v1/VideoObject",
    "name": "US Healthcare Unit 3.4",
    "description": null,
    "extensions": {
      "kaf:course_id": "213167"
    },
    "dateCreated": "2018-01-05T19:14:15.000Z",
    "dateModified": "2018-10-23T18:05:17.000Z",
    "objectType": [],
    "alignedLearningObjective": [],
    "keywords": [],
    "isPartOf": null,
    "datePublished": null,
    "version": null,
    "duration": 1180
  },
  "target": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/media/1_u5xer7qs",
    "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
    "@type": "http://purl.imsglobal.org/caliper/v1/MediaLocation",
    "name": null,
    "description": null,
    "extensions": {},
    "dateCreated": "2018-11-15T22:12:06.000Z",
    "dateModified": null,
    "objectType": [],
    "alignedLearningObjective": [],
    "keywords": [],
    "isPartOf": null,
    "datePublished": null,
    "version": null,
    "currentTime": 95
  },
  "generated": null,
  "eventTime": "2018-11-15T22:12:06.000Z",
  "edApp": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/app/KafEdApp",
    "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
    "@type": "http://purl.imsglobal.org/caliper/v1/SoftwareApplication",
    "name": "KAF",
    "description": "Kaltura Application Framework - LMS Integration",
    "extensions": {},
    "dateCreated": "2018-08-25T17:00:00.000Z",
    "dateModified": "2018-01-18T17:00:00.000Z"
  },
  "group": null,
  "membership": null,
  "federatedSession": null
}
```

#### Caliper 1.1 MediaEvent (Resumed)

```
{
  "@context": "http://purl.imsglobal.org/ctx/caliper/v1/Context",
  "@type": "MediaEvent",
  "actor": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/user/arwhyte",
    "@type": "Person",
    "name": "Anthony Whyte",
    "dateCreated": "2018-09-04T23:30:38.000Z",
    "dateModified": "2018-11-10T14:13:35.000Z"
  },
  "action": "Resumed",
  "object": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/media/1_u5xer7qs",
    "@type": "VideoObject",
    "name": "US Healthcare Unit 3.4",
    "dateCreated": "2018-01-05T19:14:15.000Z",
    "dateModified": "2018-10-23T18:05:17.000Z",
    "duration": "PT11M40S",
    "extensions": {
      "kaf:course_id": "213167"
    }
  },
  "target": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/media/1_u5xer7qs",
    "@type": "MediaLocation",
    "dateCreated": "2018-11-15T22:12:06.000Z",
    "currentTime": "PT01M35S"
  },
  "eventTime": "2018-11-15T22:12:06.000Z",
  "edApp": {
    "@id": "https://aakaf.mivideo.it.umich.edu/caliper/info/app/KafEdApp",
    "@type": "SoftwareApplication",
    "name": "KAF",
    "description": "Kaltura Application Framework - LMS Integration",
    "dateCreated": "2018-08-25T17:00:00.000Z",
    "dateModified": "2018-01-18T17:00:00.000Z"
  }
}
```




## <a name="reference"></a>References

<a name="iso8601'>ISO 8601 Date and time format. URL: https://www.iso.org/iso-8601-date-and-time-format.html

<a name="rfc3987"></a>__RFC 3987__.  IETF. M. Duerst and M. Suignard.  "Internationalized Resource Identifiers (IRIs).  January 2005.  URL: https://www.ietf.org/rfc/rfc3987.txt

<a name="rfc4122"></a>__RFC 4122__.  IETF. P. Leach, M. Mealling and R. Salz.  "A Universally Unique Identifier (UUID) URN Namespace."  July 2005.  URL: https://tools.ietf.org/html/rfc4122.
