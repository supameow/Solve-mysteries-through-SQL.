#Case #005: The Silicon Sabotage
QuantumTech, Miami’s leading technology corporation, was about to unveil its groundbreaking microprocessor called “QuantaX.” Just hours before the reveal, the prototype was destroyed, and all research data was erased. Detectives suspect corporate espionage.

Start from incident_reports: search for location 'QuantumTech' > got the id and date of the incident
```sql
select * from incident_reports
where 
   location like '%QuantumTech%';
```
id: 74
date: 19890421
