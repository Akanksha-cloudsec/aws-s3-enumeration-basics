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








