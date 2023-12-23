---
layout: default
title: SOP - assess project
parent: SOPs
---

# Assess project

{: .highlight }
This is just for the infra part of the business.

## Software development

1. Can you please define your current SDLC (software development life cycle) if you have one?
2. Do you deploy automatically?
3. Do you track releases?
4. Has your code been audited in the past by a third party and if so, can you share those results?
5. Was security part of the audit?

## Dealing with problems

1. What is your BCP (Business Continuity Plan)? And have there been any instances where it was used?
2. What is your DRP (Disaster Recovery Plan)? And have there been any instances where it was used?

## Infrastructure

1. Please share an overview of your current infrastructure including whether they are self–hosted, self-managed or outsourced.
2. How do you handle sensitive information such as API keys and passwords?
3. In case you use VMs or bare metal, please provide the host OS and specific version.
4. In case your application is containerized please provide:
5. Which orchestrator is used
6. Which base images are used
7. Please list all database engines used whether relational or otherwise along with their version.
8. Identify whether the database engine is self managed and/or self hosted.
9. What is your backup schedule for each database?
10. What is your backup retention plan for each database?

## Software architecture

1. Please share an overview of your current software architecture clearly identifying external interfaces and data sources.
2. Please describe how the code is structured. Identify any relevant repositories, third party libraries and frameworks.
3. If possible, please include third party library versions.
4. What, if any, versioning system are you currently using?
5. What development methodology are you currently using (agile, kanban, etc.)?
6. What ticketing system are you using (jira, gitlab issue, github issue, etc.)?

## CI/CD

1. Have you implemented CI/CD pipelines?
2. What tool do you use?
3. you collect metrics after deployment?

## Monitoring

1. What monitoring tools do you use?
2. What is your current retention policy for the data collected through the tools?
3. How do you leverage the information from these tools?

## Project management

1. What project management tool do you use?
2. What internal communication tool(s) do you currently use and what is the retention policy?
3. Can you please share your 6 month roadmap?
4. Can you please share your 12 month roadmap?

## Senstive data

1. Can you please describe the access hierarchy to your sensitive information (password etc)?
2. Can you please describe the access hierarchy to your code base (i.e. who can commit to master, deploy etc)?
