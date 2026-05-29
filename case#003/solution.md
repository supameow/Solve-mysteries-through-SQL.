# Case #003: The Miami Marina Murder
A body was found floating near the docks of Coral Bay Marina in the early hours of August 14, 1986. Your job, detective, is to find the murderer and bring them to justice. This case might require the use of JOINs, wildcard searches, and logical deduction. 
Get to work, detective.

### 1. Start from the crime_scene
```sql
select * from crime_scene
where 
   date= 19860814
   and
   location= 'Coral Bay Marina';
```
> The body of an unidentified man was found near the docks. Two people were seen nearby: one who lives on 300ish "Ocean Drive" and another whose first name ends with "ul" and his last name ends with "ez".

### 2. Looking for these 2 witnesses
```sql
select * from person
where
   address like '3__ %Ocean Drive'
   or 
   name like '%ul %ez';
```
> Found 2 witnesses: 101(Carlos Mendez) and 102(Raul Gutierrez)
Let's check their interviews

### 3. The witnesses interviews
```sql
select * from interviews
where person_id in (101,102);
```
> I saw someone check into a hotel on August 13. The guy looked nervous.
> I heard someone checked into a hotel with "Sunset" in the name.

### 4. Join the clues for a shortlist
```sql
select 
   s.person_id,
   s.suspicious_activity,
   c.confession
from hotel_checkins h
left join surveillance_records s
on s.person_id = h.person_id
left join confessions c
on s.person_id = c.person_id
where 
   h.check_in_date= 19860813
   and
   hotel_name like '%Sunset%'
   and 
   s.suspicious_activity is not null;
```
> Got a really long list, but there're some suspicious activity we can notice: Spotted entering late at night, Seen arguing with an unknown person, Left suddenly at 3 AM.
Hmm, not sure if I missed any clue. It was a quite long list to check though.

Because the crime was conducted in the early hours of August 14 then the guy who left suddenly at 3 AM is likely to be the murderer. Check his confession to make sure then submit him to the police.
