# Suspicious-Email-From-External-Domain
The alert below was sent from another location. A suspicious email was received from an external sender with an unusual top level domain. Note from SOC Lead: This detection rule still needs fine-tuning.

**ALERT** <br>
datasource: email <br>
timestamp: 03/05/2026 09:47:31.440 <br>
subject: Amazing Hat Enhancement Pills Grow Your Hat Collection Instantly <br>
sender: keane@modernmillinerygroup.online <br>
recipient: michael.ascot@tryhatme.com <br>
attachment: None <br>
content: Want a bigger more impressive hat collection Our revolutionary hat growth formula guarantees results in just days Try now before the FDA finds out <br>
direction: inbound

**STEP 1: CONFIRM IF THE EMAIL EVENT EXIST RUN:** <br>
index=* "modernmillinerygroup.online"

<img width="941" height="360" alt="image" src="https://github.com/user-attachments/assets/a3c4de8b-ebab-413d-a769-bb2a54cdd80b" />

You already confirmed the email event exists. From the log and image above: <br>
* Sender: keane@modernmillinerygroup.online <br>
* Recipient: michael.ascot@tryhatme.com <br>
*Subject: Amazing Hat Enhancement Pills Grow Your Hat Collection Instantly <br>
*Direction: inbound<br>
*Attachment: None <br>
*Datasource: email <br>
This confirms the alert is legitimate and not a false trigger.

**Step 2 — Check If Multiple Emails Came From This Sender run:** <br>
index=* "modernmillinerygroup.online" <br>
| spath <br>
| stats count by sender recipient

<img width="941" height="308" alt="image" src="https://github.com/user-attachments/assets/2c5fc641-34c0-4675-947e-a795e0fec84a" />

**What the screenshot above Means** <br>
The count = 4 means that four events in the logs contain the same sender and recipient pair. <br>
It usually means: The same email event was logged multiple times <br>
Different log processing stages recorded the same event <br>
Splunk extracted multiple entries from the JSON structure <br>
This is common when logs come from event collectors or structured JSON logs.

**Step 3 — Verify the Actual Number of Email Events Run this to see the individual events:** <br>
index=* "modernmillinerygroup.online" <br>
| spath <br>
| table _time sender recipient subject

<img width="941" height="302" alt="image" src="https://github.com/user-attachments/assets/d713b6f3-c141-4397-94db-ec572600855a" />

From the log image screenshot i discovered that: <br>
* 1 event returned <br>
* Timestamp: 2026-03-05 09:47:18.440 <br>
* Sender: keane@modernmillinerygroup.online <br>
* Recipient: michael.ascot@tryhatme.com <br>
*Subject: Amazing Hat Enhancement Pills Grow Your Hat Collection Instantly <br>
You see each value twice because the JSON event contains duplicate field extractions, but Splunk still shows only one event.

So the important takeaway is: <br>
Only one email event exists in the logs.

**Final Investigation Conclusion Evidence Found** <br>
**Field	                   Value** <br>
* Time	                   2026-03-05 09:47:18 <br>
* Sender	                 keane@modernmillinerygroup.online <br>
* Recipient	               michael.ascot@tryhatme.com <br>
* Direction	              inbound <br>
* Attachment	            None <br>
* Subject	                Spam-like marketing message

**Log Analysis** <br>
* Only 1 event in Splunk <br>
* Only 1 recipient targeted <br>
* No attachments <br>
* Content indicates spam/scam advertising.

 **Threat Assessment** <br>
* Risk level: Low
  
 **Reason:** <br>
* No phishing link observed <br>
* No malware attachment <br>
* No mass email campaign <br>
* Only one user targeted.

  **Recommended SOC Response** <br>

* Mark the email as Spam <br>
* Block the sender domain <br>
* Monitor logs for similar domains or emails <br>
* No escalation required unless: <br>
more recipients appear <br>
malicious links are discovered.

 **Alert Verdict:** True Positive <br>

**Summary:** <br>
Investigation revealed an email originating from keane@modernmillinerygroup.online with subject "Amazing Hat Enhancement Pills Grow Your Hat Collection Instantly", which exhibits characteristics of spam/phishing campaigns. The sender domain appears suspicious and uses a non-standard .online TLD. No evidence suggests legitimate business communication.

Action Taken: <br>
Email identified as spam. Recommended blocking sender domain and investigating additional recipients.

**ESCALATION OR NOT:**
No escalation required. Monitor for additional emails from the same domain and block if necessary.

CLOSE.
