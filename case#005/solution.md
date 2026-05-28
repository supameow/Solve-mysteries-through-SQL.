# Case #005: The Silicon Sabotage 🐱‍💻
QuantumTech, Miami’s leading technology corporation, was about to unveil its groundbreaking microprocessor called “QuantaX.” Just hours before the reveal, the prototype was destroyed, and all research data was erased. Detectives suspect corporate espionage.

### 1. Start from incident_reports: 
Search for location 'QuantumTech' > got the id and date of the incident

```sql
select * from incident_reports
where 
   location like '%QuantumTech%';
```
> id: 74 | date: 19890421

### 2. Search the incident_id in witness_statements
Plug the incident_id in the witness_statements table.

```sql
select * from witness_statements
where 
   incident_id= 74;
```
2 clues are found
> I heard someone mention a server in Helsinki
> I saw someone holding a keycard marked QX- succeeded by a two-digit odd number.
 
> And dont forget the first clue access_date = 19890421
Let's check these clues to find a list of suspect id!

### 3. Chasing the clues
Let's join everything together

```sql
select
   distinct k.employee_id,
   k.keycard_code,
   e.email_subject,
   e.email_content
from keycard_access_logs k
left join computer_access_logs c
on k.employee_id = c.employee_id
left join email_logs e
on k.employee_id = e.recipient_employee_id
left join facility_access_logs f
on k.employee_id = f.employee_id
where
   k.access_date= 19890421
   and
   c.access_date= 19890421
   and
   f.access_date= 19890421
   and
   SUBSTRING(k.keycard_code, 6,1) IN ('1','3','5','7','9')
   and 
   SUBSTRING(k.keycard_code, 5,1) IN ('1','3','5','7','9')
   and
   SUBSTRING(k.keycard_code, 4,1) = '0'
   and
   c.server_location like '%Helsinki%'
```
> Found a suspect! employee_id: 99
- Tried to submit but it was not her. However, when joining the email content of her as the recipient, this girl received an email relating to the case:
> 'I noticed something strange with the alarm system. There might be a potential malfunction near the chip. Thought you should check it out to be safe.'
- Since 99 is not the saboteur, let's continue to check whoever sent her this email

### 4. Who sent the suspicious email?
Let's check if there's any suspicious content about the sender.
First, find the one who sent the email to employee_id: 99
```sql
select
   sender_employee_id 
from email_logs
where recipient_employee_id=99 
```
>sender_employee_id: 263

Now chase all the content about this one for more clues
- In the witness_statements: checked, nothing.
- In the email_logs: hmm, found something here.

```sql
select *
from email_logs
where recipient_employee_id=263
```
- This group of saboteurs are pro! They hid the sender email (as NULL). But we can find something in the content:

> email 1: L’s schedule puts her close enough, but we need her inside F18 before 9. Trigger a minor alert or routine checkup to send her in by 8:30. Make sure she logs the visit. That part matters.

> email 2: Unlock 18 quietly by 9. He’ll use his own credentials to access it shortly after L leaves. No questions. Just ensure the timing lines up. The trail will lead exactly where it needs to.

- They are talking about F18, this sounds like a floor/area where the QuantaX was placed. Their plan is: try to alarm a colleague to check before they act so this one would ignore whatever they did after that (so smart here!!). Then after this colleague left, 'he' would conduct his crime to destroy the QuantaX.

### 5. Got the saboteur!
So, let's take a look at the ones who logged their access in F18. Since the emails are saying something about timing, make sure to include the access_time.
Use the last table facility_access_logs

```sql
select 
   f.employee_id,
   e.employee_name,
   f.access_time
from facility_access_logs f
left join employee_records e
on f.employee_id = e.id
where facility_name like '%18%'
```
3 names are found: 
> Katie Watkins | id: 290 | access_time: 12:56
> Hristo Bogoev | id: 297 | access_time: 09:01
> Elizabeth Gordon | id: 99 | access_time: 08:55 

Elizabeth Gordon seems to match the email content: an innocent colleague who received the fake alarm and logged her access before 9.
Now there're only 2 left: Katie Watkins & Hristo Bogoev
> We can try to submit both to see who is the saboteur but since the email said 'Unlock 18 quietly by 9. He’ll use his own credentials to access it shortly after L leaves', the crime must be right after 9, which makes Hristo Bogoev more of the correct answer!

Link to the case: https://www.sqlnoir.com/cases/005-The-Silicon-Sabotage 
Supa thank to the game creator!
