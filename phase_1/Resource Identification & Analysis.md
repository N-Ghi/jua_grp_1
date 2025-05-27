# Resource Identification & Analysis
This section outlines both the foundational and extended resources required to support the functionality of the JuaJobs platform, analyzing their business relevance, ownership, and lifecycle.
##Initial Core Resources

### Users

Description: Represents both clients and workers; includes role, credentials, and profile data.


Justification: Fundamental to all interactions; acts as the core identity object.


Ownership: Individual user.


Lifecycle: Created at registration; updated periodically; deletable by user.


###Job Postings


Description: Jobs posted by clients with specific requirements and deadlines.


Justification: Central to client-worker matching process.


Ownership: Client.


Lifecycle: From creation to job fulfillment, withdrawal, or expiry.


### Worker Profiles


Description: Displays a worker’s expertise, location, ratings, and availability.


Justification: Allows clients to assess and hire based on qualifications.


Ownership: Worker.


Lifecycle: Continuously updated as skills or availability change.


### Applications


Description: A worker’s submission indicating interest in a job posting.


Justification: Enables job matching and client response workflows.


Ownership: Worker (created); client/system (updated).


Lifecycle: Exists from submission to job assignment, withdrawal, or rejection.


### Reviews


Description: Feedback from clients and workers after job completion.


Justification: Establishes accountability and platform trust.


Ownership: Reviewer (either party).


Lifecycle: Created once per job; immutable thereafter.


## Extended Resources

### Payments


Description: Digital records of payments between users.


Justification: Ensures financial integrity and platform credibility.


Ownership: Client (payer); recorded by system.


Lifecycle: Created upon payment; immutable for audit purposes.


### Notifications


Description: System-generated alerts for jobs, applications, reviews, etc.


Justification: Keeps users informed and engaged in real time.


Ownership: System-generated per user.


Lifecycle: Created on event trigger; user can mark as read/delete.


### Categories


Description: Classifies job postings by industry (e.g., plumbing, tailoring).


Justification: Enhances filtering, matching, and discoverability.


Ownership: Managed by platform administrators.


Lifecycle: Mostly static; can be expanded as the platform grows.



## Summary Table: Resource Ownership & Lifecycle
| Resource	| Owner |	Created By | Lifecycle
|----------|----------|----------|----------|
| Users |	Individual User |	User |	Persistent; deletable/deactivatable |
| Job Postings |	Client |	Client |	Valid until withdrawn, expired, or fulfilled |
Worker Profiles	Worker	Worker	Continuously updated
Applications	Worker	Worker	Exists from application to resolution
Reviews	Client/Worker	Reviewer	Immutable post-creation
Payments	System/Client	System/Client	Immutable; auditable
Notifications	System → User	System	Ephemeral; user-managed read/delete state
Categories	Platform Admins	Admin	Semi-static; curated centrally

