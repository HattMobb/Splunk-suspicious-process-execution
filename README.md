# Splunk-suspicious-process-execution
Using Splunk to  investigate host-centric logs to find suspicious process execution.

The brief for this investigation was as follows:

![Screenshot 2023-06-22 142835](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/1e33f64d-9dba-4842-b17c-2d38ea5b30f7)

Network users:

![Screenshot 2023-06-22 142854](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/d5c0b63b-95b6-4bc7-b3c6-604376f19ef2)

---

![Screenshot 2023-06-22 144731](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/c6428575-b461-401e-ae01-c309fd398abf)

Diving into the logs via UserName displays all listed users and corresponding number of logs. 

One definitely stands out as suspect:



![Screenshot 2023-06-22 144836](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/ad9b98ad-e54f-477d-84a6-3ded2acb3f01)

---

![image](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/12b42ea6-c234-4b56-9afd-7b1c9faec69e)

I queried via HR UserName and schtasks.exe (process that Windows uses for task scheduling) and it was immediately apparent that a user was running a .exe in the Temp folder (sus):

![Screenshot 2023-06-22 145342](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/eb38751b-d895-4faf-956a-0677b240476d)

---



![Screenshot 2023-06-22 145455](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/cc01b0ff-f1bc-435c-8d14-c6774269a487)

A LOBIN is a Living-off-the-Land Binariy or executables that are a part of an OS and can be exploited to support an attack.

I ran through the list at https://lolbas-project.github.io/# to note anything that might stand out and created a table of commands by UserName for easier viewing (the count() function allowed for easier grouping):

![Screenshot 2023-06-22 150618](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/246a0880-470a-418b-b8a8-f6ff1f6dce8b)

Cross referencing this result with the list mentioned above, `certutils` stood out like a sore thumb (especially as it was a HR user running it).

---

Drilling into this event revealed more info, including the date, third party site that was accessed and the .exe that was used:


![Screenshot 2023-06-22 150807](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/fa6b2bf6-3082-4fb5-8bdd-acbd414877e4)

Accessing the site shows what exactly the individual was looking at and what was downloaded:

![Screenshot 2023-06-22 151926](https://github.com/HattMobb/Splunk-suspicious-process-execution/assets/134090089/2a682c8d-7456-4169-a33c-e28fd6c14531)


