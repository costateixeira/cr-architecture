---
title: Overview 
has_children: true
nav_order: 1
---

# Exchange overview

The exchange of patient data can be seen as follows:

1. Data is available or entered in a system - per event, or existing aggregated data
2. Data is submitted in a given format to the interoperability layer
2.1. Several options are possible, and they may imply different interface specifications
3. The interoperability layer may pre-process the Bundle 
4. The Bundle containing Case data is saved to a repository





{% plantuml %}
@startuml
hide footbox
title save client-level data 
    participant "Case Report\nSubmitter" as POC
    participant "Interoperability\nLayer" as IOL
    participant "Patient-Level \n Data Repository" as R
    
    POC->>POC: 1. Create [[./content-1.html{Content 1 specifications} FHIR Bundle]]
    POC->>IOL: 2. [[./transaction-1.html{Transaction 1 specifications} Send FHIR Bundle]] [PUT]
    IOL->>IOL: 3. Pre-process bundle
    IOL->>R: 4. Save modified bundle
    R->>IOL: 5. Response
@enduml
{% endplantuml %}


## Case Data Creation options

{% plantuml %}
@startuml
hide footbox
title Patient-level data 
    box Case Report Submitter
      participant "Case Form\nSubmitter" as POC1
      participant "EHR FHIR\nInterface" as POC2
      participant "Data\nImport" as POC3
    end box
    box Interoperability Layer
        participant "Questionnaire\nExtraction" as IOL
    end box
    participant "Patient-Level \n Data Repository" as R

    alt FHIR API
'      POC2->>POC2: 1. [[./content-1.html Create FHIR Bundle]]
      POC2->>IOL: T1. [[./transaction-1.html{Transaction 1 specifications} Send FHIR Bundle]]
      activate IOL
      activate POC2
      deactivate POC2
      deactivate IOL
    else Episode data
      POC1->>POC1: C1. [[./content-2.html Questionnaire]]
      activate POC1
      POC1->>IOL: T2. [[./transaction-2.html Submit Questionnaire]]
      deactivate POC1
      activate IOL
      IOL->>IOL: O1. [[./operation-1.html Extract]]
      deactivate IOL

    else Legacy data import
      POC3->>POC3: O2. [[./operation-2.html Batch convert to Bundles]]
      activate POC3
      POC3->>IOL: T1. [[./transaction-1.html{Transaction 1 specifications} Send FHIR Bundle]]
      activate IOL
      deactivate POC3
      deactivate IOL
      ...
    end

    IOL->>IOL: 3. Pre-process bundle
    IOL->>R: 4. Save modified bundle
    R->>IOL: 5. Response
@enduml
{% endplantuml %}


## Architecture blocks

<a name="bundle-desc"></a>

here is the description