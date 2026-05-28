# Case #001: The Vanishing Briefcase 💼
Set in the gritty 1980s, a valuable briefcase has disappeared from the Blue Note Lounge. A witness reported that a man in a trench coat was seen fleeing the scene. Investigate the crime scene, review the list of suspects, and examine interview transcripts to reveal the culprit.

### 1. Retrieve the correct crime scene details to gather the key clue.
```sql
   select * from crime_scene
   where type= 'theft' and location='Blue Note Lounge'; 
```
Now we got the info of the crime scene: A briefcase containing sensitive documents vanished. 
A witness reported a man in a trench coat with a scar on his left cheek fleeing the scene
> Focus on trench coat and scar position (left cheek)

### 2. Identify the suspect whose profile matches the witness description.
```sql
   select * from suspects
   where attire= 'trench coat' and scar= 'left cheek';
```
> Suspects: Frankie Lombardi (id:3), 	Vincent Malone (id:183)

### 3. Verify the suspect using their interview transcript.
```sql
   select * from interviews
   where suspect_id in (3,183);
```
> 3: NULL

> 183: I wasn’t going to steal it, but I did. > Vincent Malone, case solved

Supa thanks to bro who created this: https://www.sqlnoir.com/cases/001-The-Vanishing-Briefcase


