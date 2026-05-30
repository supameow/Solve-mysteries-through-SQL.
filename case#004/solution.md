# Case #004: The Midnight Masquerade Murder
On October 31, 1987, at a Coconut Grove mansion masked ball, Leonard Pierce was found dead in the garden. Can you piece together all the clues to expose the true murderer?

### 1. First clues from crime_scene
```sql
select * from crime_scene
where 
   date=  19871031
   and
   location like '%Coconut Grove%';
```
> During a masked ball, a body was found in the garden. Witnesses mentioned a hotel booking and suspicious phone activity.

> crime_scene_id= 75
```sql
select * from witness_statements
where crime_scene_id= 75;
```
> I overheard a booking at The Grand Regency.
> I noticed someone at the front desk discussing Room 707 for a reservation made yesterday.

### 2. What happened in the hotel room
```sql
select
h.person_id,
s.note
from hotel_checkins h
left join surveillance_records s
on h.id = s.hotel_checkin_id
where 
   hotel_name= 'The Grand Regency'
   and
   room_number= '707'
   and 
   check_in_date= 19871030
   and 
   s.note is not null;
```
> This person might hear something about the case: person_id 11 - Subject was overheard yelling on a phone: "Did you kill him?"
Since this guy was on a phonecall, let's see who made the call
```sql
select * from phone_records
where caller_id= 11;
```
> 11 was the caller, and he was talking to 58 about 'Why did you kill him, bro? You should have left the carpenter do it himself!'
Since 58 is a bit too ez for this level, let's see if he received any other call
```sql
select * from phone_records
where recipient_id= 58;
```
> 163 called him 'I will do it. Only if you give me that nice Lambo of yours.'
okay... killing for a Lambo

### 3. Let's find the Lambo dude
Since the last clue we got is a Lambo car, let's chase all the Lambo owners and their scripts
```sql
select 
   i.person_id,
   p.name,
   i.confession,
   v.car_make
from final_interviews i
left join vehicle_registry v
on i.person_id = v.person_id
left join person p
on i.person_id = p.id 
where car_make like '%Lambo%'
```
> Gotcha 'I ordered the hit. It was me. You caught me.' - 97 (Marco Santos)

Not really into the storyline of this case... 
Anyway, supa thanks to the bro who created this: https://www.sqlnoir.com/cases/004-The-Midnight-Masquerade-Murder
