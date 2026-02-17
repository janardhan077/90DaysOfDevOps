**Day 05 – Linux File Read/Write Practice**

Today I practiced basic file handling using simple Linux commands.

1. Created a file
   `touch notes.txt`

2. Wrote text using overwrite
   `echo "hi this is recall hands on" > notes.txt`

3. Appended new lines
   `echo "practice makes commands permanent" >> notes.txt`
   `echo "recall is important in devops" >> notes.txt`

4. Write + display together
   `echo "learning by doing" | tee -a notes.txt`

5. Read full file
   `cat notes.txt`

6. Read parts of file
   `head -2 notes.txt`
   `tail -1 notes.txt`

7. Filter specific word
   `grep recall notes.txt`

**Key Learning:**
Redirection ( > , >> ) controls overwrite vs append, and tee helps write + view simultaneously.
