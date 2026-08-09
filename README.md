# 🛡️ AWS S3 Enumeration Basics

## 📋 Table of Contents
- <a href="#scenario">Scenario</a>
- <a href="#real-world-context">Real-world context</a>
- <a href="#entry-point">Entry Point</a>
- <a href="#attack-path-visualization">Attack Path Visualization</a>
- <a href="#attack-phase">Attack Phase</a>
- <a href="#learning-outcomes">Learning outcomes</a>
- <a href="#resources">Resources</a>

<h2><a class="anchor" id="scenario"></a> 🎯 Scenario</h2>

It's your first day on the red team, and you've been tasked with examining a website that was found in a phished employee's bookmarks. Check it out and see where it leads! In scope is the company's infrastructure, including cloud services.

<h2><a class="anchor" id="real-world-context"></a> 🔍 Real-world context</h2>

Amazon S3 (Simple Storage Service) is a very popular (and the second oldest!) AWS service that is used to store files and backups, and can even be used to serve websites. This multi-use functionality has led some to argue that this service would be more secure if it were split into separate public web hosting and private file storage services. In recent years AWS have introduced more visual warnings when customers are making buckets world-readable, but still, if this setting is available, people will set it! Misconfigurations and overly permissive settings in S3 have resulted in many data breaches over the years.

<h2><a class="anchor" id="entry-point"></a> 🔑 Entry Point</h2>

```bash

Website Name :- http://dev.huge-logistics.com

```

<h2><a class="anchor" id="attack-path-visualization"></a> 🗺️ Attack Path Visualization</h2>

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/c273fab4e53d82c519087bd38c4b64850a15eea2/Attack%20Path%20Visualization.png)

<h2><a class="anchor" id="attack-phase"></a> ⚔️ Attack Phase</h2>

Let's check out the website!

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/c273fab4e53d82c519087bd38c4b64850a15eea2/Images/img%201.png)

---

 Let's check the website source code.

 ![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/c273fab4e53d82c519087bd38c4b64850a15eea2/Images/img%202.png)
 ![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/c273fab4e53d82c519087bd38c4b64850a15eea2/Images/img%203.png)

 S3 bucket - **dev.huge-logistics.com**, it store static files which stores images, CSS and JavaScript files.

---

Lets check these bucket by attempting these requests in the browser.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/c273fab4e53d82c519087bd38c4b64850a15eea2/Images/img%204.png)
![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%205.png)

Neither of these requests are successful as both result in access denied messages.

---

Now try listing its contents, with the help of **--no-sign-request**, it would attempt to use any locally configured AWS credentials.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%206.png)

This reveals that the S3 bucket is open for the entire internet to access. Attempting to recursively list all directories. Results in an access denied error. It seems there are some directories which are not publicly accessible.

--- 

Let's check each directories. 

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%207.png)

It seems that both the **admin** and **migration-files** directories don't allow public access. The **static** and **shared** directory is available. **Shared** directory contains that archive **hl_migration_project.zip**.

---

Download and unzipping the archive **hl_migration_project.zip**.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%208.png)

There nothing inside the __MACOSX directory.

--- 

Let's check **migrate_secrets.ps1** files.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%209.png)

The script contains hardcoded AWS keys and region. Secrets Manager is a service that helps securely store, manage, and retrieve secrets like API keys and database credentials.

---

Let's set the keys.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%2010.png)

User name :- **pam-test**.

---

Now let's check **admin** and **migration-files** directories again.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/6842f027fc51667af4d8017c47f77918f59be201/Images/img%2012.png)
![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/5d8aafc37d9d111ce7d411262ebf44fcd7a1fd7b/Images/img%2013.png)

This reveals a **website transactions export file** and also the flag. Unfortunately we are unable to **download** either using our current credentials. In **migration-files** directory we found four files.

---

let's explore **migrate_secrets.ps1 file**.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/5d8aafc37d9d111ce7d411262ebf44fcd7a1fd7b/Images/img%2014.png)

We found **aws configuration**.

--- 

Let's check those configuration.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/5d8aafc37d9d111ce7d411262ebf44fcd7a1fd7b/Images/img%2015.png)

But its shows **Invalid client token**.

---

Now let's check the test-export.xml file.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/5d8aafc37d9d111ce7d411262ebf44fcd7a1fd7b/Images/img%2016.png)

**BINGO!** we found the aws **IT Admin credentials**.

---

Let's check those credentials.

![image alt](https://github.com/Akanksha-cloudsec/aws-s3-enumeration-basics/blob/5d8aafc37d9d111ce7d411262ebf44fcd7a1fd7b/Images/img%2017.png)

---








