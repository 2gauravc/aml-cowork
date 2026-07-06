# Discovery Sprint

## Objective

The objective of the Discovery Sprint is to demonstrate a new way of creating a digital workforce.

Using KYC as the business context, the sprint will test if digital workers (agents) can be created faster with a fundamentally different operating model than traditional technology delivery. Rather than treating this as another technology project, the sprint  will evaluate how we can create, maintain and improve digital workers at a radically faster pace - using the modern method of co-creating agents with AI.

The outcome of the Discovery Sprint is more than a KYC agent Proof of Concept. It is a blueprint for how the bank can build and scale a digital workforce. If successful, the new blueprint can be extended beyond KYC - to transform knowledge work across the enterprise.

## Transformation Pillars 

The Discovery Sprint is anchored on two transformation pillars.

### 1. New technology delivery model for creating AI digital workforce (Shift Left)

Today's delivery model relies on sequential handoffs between business users, business analysts, technology and testing teams. While this model has been effective for building workflow systems, new AI offers a co-creating model that is faster, agile and closer to the ultimate beneficiary - the knowledge worker. 

The sprint will test a **Shift Left** model where small teams, led by the knowledge worker, co-create digital workers. This dramatically reduces translation layers, shortens the feedbak loop and time to market. 

In this model specialized technology and data teams do not deliver individual initiatives. Rather the specialized teams focus on building: 
a) Shared co-work platform 
b) Core capabilities as APIs (eg. KYC.com integration, account txn data T2/T3)
c) Automated AI evaluation frameworks 
d) Automated testing 

The objective is not simply to deliver faster, but to enable the organisation to create many digital workers in parallel rather than a few large technology initiatives in a year.

### 2. The New Role - Knowledge Worker of the future

The knowledge worker evolves to doing more than providing user requirements and testing system features. The knowledge worker of the future is an active creator, trainer and supervisor of the digital workforce. They encode business knowledge, improve digital worker capabilities and provide human judgement where it adds the greatest value. 

## Success Criteria

The Discovery Sprint will be considered successful if it demonstrates the following:

### 1. Shift Left Delivery Model

Demonstrate that small teams of 2-4 people, with the KYC CORE analyst at the forefront, can:
 - Create a meaningful KYC digital worker using an Agent Studio Platform. the Agent will use mock data / documents (to be created by AI) 
 - This can be done in a short time window of 1-2 weeks. The team will minimize dependence on traditional SDLC process by enablig rapid co-cration with AI 

 Objective is to discover what becomes possible when we remove today's delivery assumptions and leverage frontier AI capabiities. 

### 2. Human-by-Exception Decision Model

Demonstrate that digital workers can perform routine KYC activities, enabling humans to focus on judgement, exception handling and on improving the digital workers. 

Discover how far today's fronteir AI can be in doing end to end KYC activities:

1. Which KYC activities can be performed reliably by AI today?
2. Which activities need human judgement? 
3. What activities can be performed by AI with technology / policy evolution ?  
4. What kind of governance is needed to make this work including - explainability, evaluation, audit trail, prompt versioning (and back testing), confidence thresholds and human feedback  

## Proof Scenarios 

The agent will complete a new CDD (onboarding) or update a CDD (for review). The CDD has 3 sections: 1) Customer Business Profile (CBP) 2) Ownership & Control (OC) 3) Account Activity Review (AAR). 

### 1. Customer Business Profile (CBP): 
Create or update a customer business profile and produce outcome in a pre-defined format (JSON). CBP schema fields 
a) commpany name, b) Biz registration Number c) incorporation date d) incorporation country e) paid-up capital (amount, currency) f) Legal Form (e.g. Private Limited), g) Registered Address h) Company Status (e.g. active, dissolved) i) entity type (e.g. Commercial Corporate) 
j) listing status (Listed, NASDAQ) k) Industry l) PBA m) shell typology risk  
n) high risk industry flags (GAP) o) independent risk assessment (GAP)

1) Information retrieval via TOOLS
a) Registry Business Profile PDF Document (biz registration number, incorporation date, incorporation country, paid-up capital, legal form, registered address, company status)
b) Listing register Spreadsheet (exchange, listed company name, ticker, market cap) 
c) Industry classification master (SSIC) Spreadsheet 
d) Company Profile via Search in a JSON Document. The JSON has fake Web search results 
e) Linkedin Profiles - via Search in a JSON Document. The JSON has fake LInkedIn Profiles 

2) Execution of business logic via SKILLS
a) Extract from PDF document (registry business profile) 
b) Determine Entity Type (e.g. Commercial Corporate)
c) Determine listing status 
d) Industry Classifier 
e) High Risk Industry Assessment 
f) Principal Business Activity Writer 
g) Shell Typology Risk Detection 

3) Missing Infor / Costly Misses / Red Flag (Risk Reasoning) via SKILLS
a) Missing information / documents: Assess information / documents complete. Assess document recency. 
b) Information Consistency - Assess if information consistent or conflicting 
c) Detect Costly Errors - Wrong listing status, wrong industry classification, missed high risk industry, missed shell Risk detection (2f). Unsupported , implausible conclusions 
d) Policy exceptions - e.g. dated proof of address 
e) Escalation required for human review (if needed)

4) Orchestrating Agent 
a) Populate CBP AGENT (Operates skills 2a, 2b..)
b) Independent Challenge CBP AGENT (Operates skills 3a, 3b, ...)

### 2. Ownership & Control (OC): 
Create or update company ownership structure and produce outcome in pre-defined JSON format. 
OC Schema Fields 
a) Ownership structure (graph) b) UBOs c) Related Parties (shareholders, directors, auth signatories) d) nominee arrangement assessment e) Complex Structure assessment f) independent risk assessment (GAP)

1) Information retrieval via TOOLS
a) Registry Business Profile / CoI PDF document (this entity, other entities in layers) 
b) AO Form PDF Document (identifies authorized signatories) 
c) Change of Mandate Form PDF Document
d) Due Diligence Form (Nominee Declaration) (PDF)

2) Execution of business logic via SKILLS
a) Unwrap ownership structure 
b) Identify UBO 
c) Extract from document (registry business profile, AO Form, Change of Mandate, DD Form) 

3) Missing Infor / Costly Misses / Red Flag (Risk Reasoning) via SKILLS
a) Missing information / documents: Assess information / documents complete. Assess document recency.
b) Information Consistency - Assess if information consistent or conflicting 
c) Detect Costly Errors - Wrong unwrapping, missed UBO. Unsupported , implausible conclusions 
d) Policy exceptions  
e) Escalation required for human review (if needed)

4) Orchestrating Agent 
a) Populate CBP AGENT (Operates skills 2a, 2b..)
b) Independent Challenge CBP AGENT (Operates skills 3a, 3b, ...)

### 3. Account Activity Review (AAR): 
Create an account activity review and produce outcome in pre-defined JSON format. 
AAR Schema Fields 
a) review period (from, to) b) transaction profile assesssment (volume, count, currency) 
c) counterparty assessment d) plausibility assessment e) independent risk assessment (GAP) 

1) Information retrieval via TOOLS
a) Account Activity (Transactions) via Spreadsheeet 
b) Company Profile via Search in a JSON Document. The JSON has fake Web search results 
c) Counterparty Profile via Search in a JSON Document. The JSON has fake Web search results 
d) High risk country list via spreadsheet 

2) Execution of business logic via SKILLS
a) Transaction profile assessment  
b) Counterparty assessment 
c) Summarize AAR 

3) Missing Infor / Costly Misses / Red Flag (Risk Reasoning) via SKILLS
a) Missing counterparties in txn data 
b) More information needed from customer to corroborate the transactions 
c) Red Flag: Mule / Scam typology 
d) Unexplained / implausible transactions 

4) Orchestrating Agent 
a) Populate AAR AGENT (Operates skills 2a, 2b..)
b) Independent Challenge CBP AGENT (Operates skills 3a, 3b, ...)

### 4. Independent Challenge (Checker / Risk Review)
Provide independent challenge by identifying errors, omissions, inconsistencies and unsupported conclusions. Identify need for more docs / info to corroborate the conclusions and to meet policy objectives. 

1) Information retrieval via TOOLS
Retrieve the risk assessment section of CBP, OC and AAR. 

2) Execution of business logic via SKILLS
a) Identify potential costly misses not remediated  
b) Identify potential 


Note: 
1. Information sources will be limited to spreadsheets, JSON/Text files and PDF documents in the discovery sprint. 
2. A fake KYC policy document will be created. It will articulate ID&V requirements, document acceptability requirements (incl recency) UBO identification, high risk industries and general risk guidelines.  


## Platform / Tools 

### Agent studio (Business environment to create, test and improve digital workers)
Claude co-work 

### Agents 

1. Maker Agent - A) Onboarding , B) Review
2. Independent Challenge Agent 

### Skills (Knowledge/Logic/Rules of how to complete a process task)

1. Establish Customer identity (unique ID, incorporation date etc.) - 1A
2. Determine Listing Status - 1A
3. Determine customer industry - 1A
4. Generate PBA - 1A
5. Assess Shell Risk - 1A
6. Unwrap ownership structure 
7. Identify UBO & Controlling Parties 
8. Identfy related parties 
9. Nominee Arrangement Assessment 
10. Complex structure Assessment 
11. Transaction Profile Analysis (volume, count, counterparties, countries) 
12. Transaction counterpaty assessment (of plausibility)
13. AAR - Summary Narrative generation 
14. Business Profile Review 
15. UBO & Shareholders Review 
16. RP Identification and Details Review 
17. AAR review
18. Evidence Validation 
19. Doc & Information Gap Analysis 


## Tools (Generic APIs - give input get output)
1. Registry look-up (single, recursive) 
2. Stock Exchange search 
3. Knowledge Search 
4. OCR document parser (separate, classify)
5. retrieve customer txns (details, summary)
6. Check country risk rating 

## Knowledge Base 

Policy Extracts: 
1. UBO Identification 
2. ID&V requirements 

##   (Structured and Unstructued)

1. Fake company register KYC.com
    a) Company unique ID, incorporation date, incorporation country (structured data)
    b) shareholders
        i)  Corporate - company unique ID, incorporation date, incorporation country (structured data)
        ii) Individual - name, ID, nationality, date of birth (structured data)
    c) directors - name, ID, nationality, date of birth (structured data) 
    d) company business registration document (document PDF) 
2. 

## Documents 

TBD 

 