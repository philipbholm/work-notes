# SPUHiN

- Entiteter: Brukere av helsedata (inkl. forskning), helseregistre, sykre analyserom
- Data Governance Act: Regulation by the EU with a goal to facilitate data-sharing. 
- EHDS, article 50:
	1. The health data access bodies shall provide access to electronic health data only through a secure processing environment, with technical and organisational measures and security and interoperability requirements. In particular, they shall take the following security measures:
		(a) restrict access to the secure processing environment to authorised persons listed in the respective data permit;
		(b) minimise the risk of unauthorized reading, copying, modification or removal of electronic health data hosted in the secure processing environment through state-of-the-art technological means;
		(c) limit the input of electronic health data and inspection, modification or deletion of electronic health data hosted in the secure processing enviornment to a limited number of authorised identifiable indviduals;
		(d) ensure that data users have access only to the electronic health data covered by their data permit, by means of individual and unique user identities and confidential access modes only;
		(e) keep identifiable logs of access to the secure processing environment for the period of time necessary to verify and audit all processing operations in that environment;
		(f) ensure compliance and monitor the security measures referred to in this Article to mitigate poential security threats.
- Solution for article 50:
	1a. Require level 4 auth (BankID, Buypass, Commfides)
	1b. Permissions for adding, view, edit and delete per entry and variable. Unmasking of anonymised values.
	1c. Surveys with level 4 auth. Verified import. 
	1d. Permissions 
	1e. Audit logs of login, view, edit, deletion of data. Logs for all code that is ran
	1f. Monitoring of all outbound traffic
- Phases: 
	- Data collection:
		1. Collection
		2. Standardization 
		3. Publication
	- Users' journey:
		1. Discovery
		2. Permit application
		3. Use
		4. Project finalization
- 