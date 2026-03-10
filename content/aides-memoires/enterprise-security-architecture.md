---
title: "Enterprise Security Architecture"
summary: "Compilations des points importants du livre 'Enterprise Security Architecture: A Business-Driven Approach' par John Sherwood"
date: "2026-01-16"
categories: ["mémo"]
tags: ["cyber-secu", "architecture", "livre"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Part 1: Introduction

---

### Chapitre 1 : The Meaning of Security

---

### Chapter 2 : The Meaning of Architecture

---

### Chapter 3 : Security Architecture Model

The approach to developing an enterprise security architecture that is proposed in this book is based upon a six-layer model. This model is used as the basis of an architecture development process. By following the development of the enterprise architecture in line with the layers of the model, the methodology becomes somewhat self-evident.

#### The SABSA® Model

To establish a layered model of how a security architecture is created, it is useful to return for a moment to the use of the word in its conventional sense: the construction of buildings.  
The SABSA® Model comprises six layers. Each layer represents the view of a different player in the process of specifying, designing, constructing and using the building.

| Layer | Domain |
| --- | --- |
| The Business View | Contextual Security Architecture |
| The Architect’s View | Conceptual Security Architecture |
| The Designer’s View | Logical Security Architecture |
| The Builder’s View | Physical Security Architecture |
| The Tradesman’s View | Component Security Architecture |
| The Facilities Manager’s View | Operational Security Architecture |

There is another configuration of these six layers.

![SABSA® model diagram](/images/SABSA-model-diagram.png)

In this diagram Operational Security Architecture has been placed vertically across the other five layers. This is because operational security issues arise at each and every one of the other five layers. Operational security has a meaning in the context of each of these other layers.

For detailed analysis of each of the six layers, the SABSA® Model also uses the same six questions that are used in the Zachman Framework and which were articulated by Rudyard Kipling in his poem ‘I Keep Six Honest Serving-Men’:  
What, Why, How, Who, Where and When

To maintain the parallel with building construction, the questions are used as follows:

- What sort of building is needed?
- Why do you want this building? The goals that you want to achieve.
- How will it be used? The detailed functional description.
- Who will use the building, including the types of people, their physical mobility, the numbers of them expected, and so on?
- Where should it be located, and what is its geographical relationship to other buildings and to the infrastructure (such as roads, railways etc)?
- When will it be used? The times of day, week, year, and the pattern of usage over time.

When designing a secure business information system, the questions become :

- What type of information system is it and for what will it be used?
- Why will it be used?
- How will it be used?
- Who will use it?
- Where will it be used?
- When will it be used?

These are the characteristic questions that you must ask. From the analysis of the replies you receive, you should be able to gain an understanding of the business requirements for the secure system. From those you should be able to synthesise a systems architecture and a security architecture that meets those requirements.

In the SABSA® Model this business view is called the contextual security architecture. It is a description of the business context in which your secure systems must be designed, built and operated.

There simply is no substitute for doing architecture work the proper way. You may try to take shortcuts, but your efforts will most likely result in failure, which costs the business more money, delivers less benefit, and destroys the confidence that business people may have in information and communications technology as the means to enable business development.

Each layer is also analysed vertically using six key questions. In the model presented here, the contextual security architecture is concerned with:

- What? The business, its assets to be protected (brand, reputation, etc.) and the business needs for information security (security as a business enabler, secure electronic business, operational continuity and stability, compliance with the law, etc.);
- Why? The business risks expressed in terms of assets, goals, success factors and the threats, impacts and vulnerabilities that put these at risk, driving the need for business security (brand protection, fraud prevention, loss prevention, legal obligations, business continuity, etc.);
- How? The business processes that require security (business interactions and transactions, business communications, etc.);
- Who? The organisational aspects of business security (management structures, supply chain structures, out-sourcing relationships, strategic partnerships);
- Where? The business geography and location-related aspects of business security (the global village market place, distributed corporate sites, remote working, etc.);
- When? The business time dependencies and time-related aspects of business security in terms of both performance and sequence (business transaction throughput, lifetimes and deadlines, just-in-time operations, time-to-market, etc.).

#### The Architect’s View

*An architect is a visionary who creates the concept of how the system will be built, and sets the design rules.*

The architect’s view is the overall concept by which the business requirements of the enterprise may be met. Thus this layer of the architectural model is also referred to as the conceptual security architecture. It defines principles and fundamental concepts that guide the selection and organisation of the logical and physical elements at the lower layers of abstraction.

When describing the enterprise security architecture, this is the place to describe the security concepts and principles that you will use. These include:

- What you want to protect, expressed in the SABSA® Model in terms of a SABSA® Business Attributes Profile?
  - &rArr; What? — Business Attributes
- Why the protection is important, in terms of control objectives?
  - &rArr; Why? — Control Objectives – the motivation for security
- How you want to achieve the protection, in terms of high-level technical and management security strategies?
  - &rArr; How? — Major security strategies and layering principles
- Who is involved in security management, in terms of entity relationship models, and the trust framework within which entities interact with one another?
  - &rArr; Who? — The security entities and their trust relationships
- Where you want to achieve the protection conceptualised in terms of security domains?
  - &rArr; Where? — The security domain model
- When is the protection relevant, in terms of both points in time and periods of time?
  - &rArr; When? — The time dependence of security

#### The Designer’s View

*The designer must realise the architect’s vision as a meaningful design.*

The designer has to interpret the architect’s conceptual vision and turn it into a logical structure that can be engineered to create a real building.

In the world of business computing and data communications, this design process is often called ‘systems engineering’. It involves the identification and specification of the logical architectural elements of an overall system. This view models the business as a system, with system components that are themselves sub-systems. It shows the major architectural security elements in terms of logical security services, and describes the logical flow of control and the relationships between these logical elements. It is therefore also known as the logical security architecture.

The logical security architecture should reflect and represent all of the major security strategies in the conceptual security architecture. At this logical level, everything from the higher layers is transformed into a series of logical abstractions.

The logical security architecture is concerned with:

- What? Business information is a logical representation of the real business. It is this business information that needs to be secured;
- Why? Specifying the security policy requirements (high-level security policy, registration authority policy, certification authority policy, physical domain policies, logical domain policies, etc.) for securing business information;
- How? Specifying the logical security services (entity authentication, confidentiality protection, integrity protection, non-repudiation, system assurance, etc.) and how they fit together as common re-usable building blocks into a complex security system that meets the overall business requirements;
- Who? Specifying the entities (users, security administrators, auditors, etc.) and their inter-relationships, attributes, authorised roles and privilege profiles in the form of a schema;
- Where? Specifying the security domains and inter-domain relationships (logical security domains, physical security domains, security associations);
- When? Specifying the security processing cycle (registration, certification, login, session management, etc.);

#### The Builder’s View

*The builder constructs the physical system.*

The builder’s job is to choose and assemble the physical elements that will make the logical design come to life as a real construction. This view is therefore also referred to as the ‘physical security architecture’.

In the world of business information systems, the designer produces a set of logical abstractions that describe the system to be built. These need to be turned into a physical security architecture model that describes the actual technology model and specifies the functional requirements of the various system components.

In total, the physical security architecture is concerned with:

- What? Specifying the business data model and the security-related data structures (tables, messages, pointers, certificates, signatures, etc.);
- Why? Specifying rules that drive logical decision-making within the system (conditions, practices, procedures and actions);
- How? Specifying security mechanisms (encryption, access control, digital signatures, virus scanning, etc.) and the physical machines upon which these mechanisms will be hosted;
- Who? Specifying the people dependency in the form of the users, the applications that they use and the security user interface (screen formats and user interactions);
- Where? Specifying security technology infrastructure (physical layout of the hardware, software and communications lines);
- When? Specifying the time dependency in the form of execution control structures (sequences, events, lifetimes and time intervals).

#### The Tradesman’s View

*The construction process needs a range of different skills and component parts.*

When the builder plans the construction process, she or he needs to assemble a team of experts in each of the building trades that will be needed. Each one of these brings some very specific production skills and some very specific products to the overall construction process. It is the same in the construction of information systems. The builder needs to assemble a series of products from specialist vendors and a team with the integration skills to join these products together during an implementation of the design.

Each of the integrators is the equivalent of a tradesman, working with specialist products and system components that are the equivalent of building materials and components. Some of these ‘trades’ are hardware-related, some are software-related, and some are service oriented. Hence this layer of the architectural model is also called the ‘component security architecture’.

The component security architecture is concerned with:

- What? Data field specifications, address specifications and other detailed data structure specifications;
- Why? Security standards;
- How? Products and tools (both hardware and software);
- Who? User identities, privileges, functions, actions and access control lists (ACLs);
- Where? Computer processes, node addresses, and inter-process protocols;
- When? Security step timings and sequencing.

#### The Facilities Manager’s View

*The facilities manager runs the building in its operational lifetime.*

When the building is finished, those who architected, designed and constructed it move out, but someone has to run the building during its lifetime. Such a person is often called the facilities manager. The framework for dealing with the operation of the building and its various services, maintaining it in good working order, and monitoring how well it is performing in meeting the requirements is called the ‘operational security architecture’.

In the realm of business information systems the operational architecture is concerned with classical systems operations work. Here the focus of attention is only on the security-related parts of that work.

The operational security architecture is concerned with the following:

- What? Ensuring the operational continuity of the business systems and information processing, and maintaining the security of operational business data and information (confidentiality, integrity, availability, auditability and accountability);
- Why? To manage operational risks and hence to minimise operational failures and disruptions;
- How? Performing specialised security-related operations (user security administration, system security administration, data back-ups, security monitoring, emergency response procedures, etc.);
- Who? Providing operational support for the security-related needs of all users and their applications (business users, operators, administrators, etc.);
- Where? Maintaining the system integrity and security of all operational platforms and networks (by applying operational security standards and auditing the configuration against these standards);
- When? Scheduling and executing a timetable of security-related operations.

However, referring back to diagram in the [SABSA® Model section](#the-sabsa-model), there is another dimension to the operational security architecture – its vertical relationship with the other five layers of the model. Thus the operational security architecture needs to be interpreted in detail at each and every one of the other five layers.

Here are some examples of the type of operational activity that is implied with regard to each of the layers.

##### The Operational Security Architecture

| Layer | Operational activities |
| --- | --- |
| Contextual Layer | Business policymaking, business risk assessment process, business requirements collection and specification, organisational and cultural development, etc. |
| Conceptual Layer | Major programmes for training and awareness, business continuity management, audit and review, process development for registration, authorisation, administration and incident handling, development of standards and procedures, etc. |
| Logical Layer | Security policymaking, information classification, system classification, management of security services, security of service management, negotiation of inter-operable standards for security services, audit trail monitoring and invocation of actions, etc. |
| Physical Layer | Development and execution of security rules, practices and procedures, including: cryptographic key management, communication of security parameters between parties, synchronisation between parties; ACL maintenance and distribution of ACEs, backup management (storing, labelling, indexing, etc.), virus pattern search maintenance, event log file management and archiving, etc. |
| Component Layer | Products, technology, evaluation and selection of standards and tools, project management, implementation management, operation and administration of individual components, etc. |

#### The Inspector’s View

*Providing assurance through audit and inspection.*

The inspector’s view is concerned with providing assurance that the architecture is complete, consistent, robust and ‘fitfor-purpose’ in every way.

In the realm of information systems security this is the process of security auditing carried out by computer auditors or systems quality assurance personnel.

However, the SABSA® Model does not recognise this as a separate architectural view. The SABSA® approach to audit and assurance is that the architecture model as a whole supports these needs. The existence of such an architecture is one of the ways in which the auditors will establish that security is being applied in a systematic and appropriate way. The framework itself can provide a means by which to structure the audit process. In addition, security audit and review is addressed as one of the major strategic programmes within the operational security architecture associated with the conceptual layer.

#### The SABSA® Matrix

In the above sections, each of the six horizontal layers of abstraction of the architecture model has been examined. Each of the sections has also introduced a series of vertical cuts through each of these horizontal layers, answering the questions:

- What are you trying to do at this layer? – The assets to be protected by your security architecture;
- Why are you doing it? – The motivation for wanting to apply security, expressed in the terms of this layer;
- How are you trying to do it? – The functions needed to achieve security at this layer;
- Who is involved? – The people and organisational aspects of security at this layer;
- Where are you doing it? – The locations where you apply your security, relevant to this layer;
- When are you doing it? – The time-related aspects of security relevant to this layer.

These six vertical architectural elements are now summarised for all six horizontal layers. This gives a 6×6 matrix of cells, which represents the whole model for the enterprise security architecture called the SABSA® Matrix.

If you can address the issues raised by each and every one of these cells, then you will have covered the entire range of questions to be answered, and you can have a high level of confidence that your security architecture is complete. The process of developing an enterprise security architecture is a process of populating all of these 36 cells.

##### The 36-Cell SABSA® Matrix

| | Assets (What) | Motivation (Why) | Process (How) | People (Who) | Location (Where) | Time (When) |
| --- | --- | --- | --- | --- | --- | --- |
| Contextual | The Business | Business Risk Model | Business Process Model | Business Organisation and Relationships | Business Geography | Business Time Dependencies |
| Conceptual | Business Attributes Profile | Control Objectives | Security Strategies and Architectural Layering | Security Entity Model and Trust Framework | Security Domain Model | Security-Related Lifetimes and Deadlines |
| Logical | Business Information Model | Security Policies | Security Services | Entity Schema and Privilege Profiles | Security Domain Definitions and Associations | Security Processing Cycle |
| Physical | Business Data Model | Security Rules, Practices and Procedures | Security Mechanisms | Users, Applications and the User Interface | Platform and Network Infrastructure | Control Structure Execution |
| Component | Detailed Data Structures | Security Standards | Security Products and Tools | Identities, Functions, Actions and ACLs | Processes, Modes, Addresses and Protocols | Security Step Timing and Sequencing |
| Operational | Assurance of Operational Continuity | Operational Risk Management | Security Service Management and Support | Application and User Management Support | Security of Sites, networks and Platforms | Security Operations Schedule |

#### Detailed SABSA® Matrix for the Operational Layer

When one examines the lowest layer (operational security architecture) of the [36-Cell SABSA® Matrix](#the-36-cell-sabsa-matrix), and refers back to the [Operational Security Architecture](#the-operational-security-architecture), it becomes clear that this operational layer can be further broken out into a Matrix mapping to each of the five layers above. In other words, there are operational aspects associated with each of the contextual, conceptual, logical, physical and component layers. This more detailed insight into the operation security architecture is provided in the Operational Security Architecture Matrix.

##### The Operational Security Architecture Matrix

| | Assets (What) | Motivation (Why) | Process (How) | People (Who) | Location (Where) | Time (When) |
| --- | --- | --- | --- | --- | --- | --- |
| Contextual | Business Requirements Collection; Information Classification | Business Risk Assessment; Corporate Policy Making | Business-Driven Information Security Management Programme | Business Security Organisation Management | Business Field Operations Management | Business Calendar & Timetable Management |
| Conceptual | Business Continuity Management | Security Audit & Assurance Levels; Measurement, Metrics & Benchmarking | Incident Response; Disaster Recovery; Change Control Programme | Security Training; Awareness & Culture Development | Security Domain Management | Security Operation Schedule Management |
| Logical | Information Security; System Integrity | Detailed Security Policy Making; Policy Compliance; Monitoring; Intelligence Gathering | Intrusion Detection; Event Monitoring; Process Development; Security Service Management, System Development Controls; Configuration Management | Access Control & Privilege Profile Administration | Application Security Administration & Management | Managing Application Deadlines & Cut-off |
| Physical | Database Security Software Integrity | Vulnerability Assessment; Penetration Testing; Threat Assessment | Rule Definition; Key Management; ACL Maintenance; Backup Admin; Computer Forensics; Event Log Admin; Anti-Virus Admin | User Support & Help Desk | Network Security Management; Site Security Management | User Account Aging; Password Aging; Crypto Key Aging; Administering Time Windows for Access Control |
| Component | Product & Tool Security & Integrity | CERT Notifications; Research on Threats & Vulnerabilities | Product Procurement; Project Management, Operations Management | Personnel Vetting; User Administration | Platform, Workstation & Equipment Security Management | Time-out Configuration; Detailed Operation Sequence |

#### To Summarise: The Security Architecture Model

The SABSA® Model for Security Architecture Development used in this book has six layers:

- Contextual security architecture – the business view;
- Conceptual security architecture – the architect’s view;
- Logical security architecture – the designer’s view;
- Physical security architecture – the builder’s view;
- Component security architecture – the tradesman’s view;
- Operational security architecture – the facilities manager’s view.

The operational layer can be visualised as cutting across the other five layers, since there are operational aspects to each of these layers. Each of these six layers is further analysed by asking six basic questions:

- What?
- Why?
- How?
- Who?
- Where?
- When?

Combining the horizontal layered analysis with the vertical analysis of the six key questions produces a 36-cell table called the SABSA® Matrix.

There is another architectural view – the inspector’s view. For an enterprise security architecture this is the view taken by the security auditor. In the SABSA® Model this is treated this as an integral part of the operational security architecture and so it does not have a separate layer in the model.

The SABSA® Matrix provides a framework for developing and documenting your enterprise security architecture. Each cell must be addressed in turn (although not necessarily in strict sequential order). Thus a security architecture document might well be structured with a chapter for each row of the matrix and a section within the chapter for each cell in that row. By taking this approach you can have a high level of assurance that your security architecture is comprehensive.

---

### Chapter 4 : Case Study

---

### Chapter 5 : A Systems Approach

---

### Chapter 6 : Measuring Return on Investment in Security Architecture

---

### Chapter 7 : Using This Book as a Practical Guide

---

### Chapter 8 : Managing the Security Architecture Programme

---

## Part 2: Strategy and Planning

---

### Chapter 9 : Contextual Security Architecture

---

### Chapter 10 : Conceptual Security Architecture

---

## Part 3 : Design

---

### Chapter 11 : Logical Security Architecture

---

### Chapter 12 : Physical Security Architecture

---

### Chapter 13 : Component Security Architecture

---

## Part 4 : Operations

---

### Chapter 14 : Security Policy Management

---

### Chapter 15 : Operational Risk Management

The key driver for your enterprise security architecture is business risk. This chapter examines in detail the management of risk within your business operations. It discusses operational risk management in general terms but constantly focuses back onto the specific needs for managing operational risk in the context of business information security.

#### Introduction to Operational Risk Management

The aims of operational risk management are to :

- Understand the enterprise risk profile in detail;
- Make well-informed risk-mitigation and risk-taking decisions;
- Minimise the overall cost to the organisation, taking into account the widest possible view.

Definition of operational risk across different industry sector (p. 626 to 628):

| Operational Risk Areas | Description | Information or ICT Mapping |
| -------- | -------- | -------- |
| Health & Safety Risk | Threats to the personal health and safety of staff, customers and members of the public | Confidentiality of home addresses and travel schedules |
| Information Security Risk | Unauthorised disclosure or modification to information, or loss of availability of information, or inappropriate use of information | All aspects of information and ICT security |
| Corporate Governance Risk | Failure of directors to fulfil their personal statutory obligations in managing and controlling the company | Information security policy making, performance measurement and reporting |
| Reputation Risk | The negative effects of public opinion, customer opinion, market reputation and the damage caused to the brand by failure to manage public relations | Controlling the disclosure of confidential information; Also presenting a public image of a well-managed enterprise |
| Technology Risk | Failure to plan, manage and monitor the performance of technology-related projects, products, services, processes, staff and delivery channels | Failure of information and communications technology systems and the need for business continuity management |

#### Regulatory Drivers for Operational Risk Management

There is a continuing pressure from both governments and industry regulators to clean up the corporate act so as to improve the quality of corporate governance and the accountability of senior executives in the face of governance failures.

Drivers for increased regulation are :

- Major corporate frauds like Enron, WorldCom, Shell and Parmala
- Failures of corporate strategy such as at The Equitable Life
- Failures of operational control such as at Barings Bank
- Scandal of mis-selling of life assurance and pensions product

Some of the most important recent developments in the corporate governance regulation and compliance arena :

- Sarbanes-Oxley (USA) : *Financial reporting transparency*
- Patriot Act (USA) : *Government surveillance* and *Archiving of all corporate communications*
- Basel II (Banking Industry) : *Representing banking supervision in the G10 countries*, *Central banks and national regulators*, *Harmonisation of national regulations*, *The first Basel Accord addressed credit risk*, *Market risk was included under various amendments*, *The New Basel Accord includes operational risk* and *Supervision and public disclosure of risk management practices*
- Gramm-Leach-Bliley Act (USA) : *Privacy for banking customers*
- HIPAA (USA) : *Standards for electronic transmission of health care information* and *Information security is explicitly included*
- CAD3 (EU) : *EU harmonisation of Basel II implementations*
- Combined Code, Turnbull, Smith and Higgs (UK) : *Standards of governance for listed companies*, *Statement in the annual report of a listed company*, *Turnbull Report: guidance on the Combined Code* and *Improving transparency for shareholders*
- Integrated Prudential Sourcebook (UK) : *The FSA Integrated Prudential Sourcebook* and *FSA requirements for operational risk management*
- 21 CFR Part 11 (Pharmaceuticals Industry, USA) : *The use of electronic documents in the food and drug industries*, *Compliance mandated for all pharmaceutical firms*, *Creating trust and privacy in electronic documents*, *Computer security and sound business practice* and *Electronic recordkeeping is not mandated, but where it used, compliance with the
regulations is mandated*
- FAA, CAA and Others (Civil Aviation Industry) : *Regulation of the civil aviation industry*, *Licensing of people, organisations and aircraft* and *Licensing implies documentation, much of it in electronic format*
- Data Protection Legislation (EU) : *European protection of information held on private individuals*, *Define concepts*, *Eight principles* and *A major driver for information security*

#### The Complexity of Operational Risk Management

#### Approaches to Risk Assessment

#### Managing Operational Risk

#### Risk Mitigation

#### Risk-Based Security Reviews

#### Risk Financing

#### The Risk Management Dashboard

#### To Summarise

---

### Chapter 16 : Assurance Management

---

### Chapter 17 : Security Administration and Operations

---
