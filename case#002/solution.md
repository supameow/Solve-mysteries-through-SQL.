# Case #002: The Stolen Sound 💿
In the neon glow of 1980s Los Angeles, the West Hollywood Records store was rocked by a daring theft. A prized vinyl record, worth over $10,000, vanished during a busy evening, leaving the store owner desperate for answers. Vaguely recalling the details, you know the incident occurred on July 15, 1983, at this famous store. Your task is to track down the thief and bring them to justice.
### 1. Retrieve the crime scene report for the record theft using the known date and location.
```sql
select * from crime_scene
where 
   date= 19830715
   and 
   location= 'West Hollywood Records';
```
> crime_scene_id= 65

### 2. Retrieve witness records linked to that crime scene to obtain their clues.
```sql
select * from witnesses
where crime_scene_id= 65
```
> I saw a man wearing a red bandana rushing out of the store.
> The main thing I remember is that he had a distinctive gold watch on his wrist.

Let's search for red bandana and distinctive gold watch on his wrist in the suspects table.

### 3. Use the clues from the witnesses to find the suspect in the suspects table.
```sql
select * from suspects
where
   bandana_color= 'red'
   and
   accessory= 'gold watch';
```
> Got 3 suspects: 35(Tony Ramirez), 44(Mickey Rivera), 97(Rico Delgado)

### 4. Retrieve the suspect's interview transcript to confirm the confession.
```sql
select * from interviews
where suspect_id in (35,44,97)
```
> 97: I couldn't help it. I snapped and took the record.

Supa thanks to bro who created this: https://www.sqlnoir.com/cases/002-The-Stolen-Sound
