# pro-notes

# Exam Details: 

* 170 minutes, ~74 questions
 
* Multiple-choice (pick best answer) and multiple-answer (pick 3 of 8)

* No partial credit for questions

* You can mark questions and go back to them within the time limit

* No points for unanswered questions

* Score between 100 and 1000 with a minimum passing score of 750. Scaled scoring models are used

Blueprint:

| Domain                                           | % of Examination |
| ---                                              | ---              |
| 1. Design for Organizational Complexity          | 12.5%            |
| 2. Design for New Solutions                      | 31%              |
| 3. Migration Planning                            | 15%              |
| 4. Cost Control                                  | 12.5%            |
| 5. Continuous Improvement for Existing Solutions | 29%              |


* A new product, service, or feature must be generally available (GA) for six months before appearing on a certification exam.

* Special accomodations can be requested once an exam is scheduled - but it must be reviewed and might require supporting documentation.

* Select one or more responses that best complete the statement or answer the question. Distractors, or incorrect answers, are response options that an examinee with incomplete knowledge or skill would likely choose. However, they are generally plausible responses that fit in the content area defined by the test objective.

| Do This                                                     | Not That                                          |
| ---                                                         | ---                                               |
| Focus on service capabilities and what problems they solve. | Memorize service limits, prices, version numbers. |
| Study to become an Expert                                   | Study to pass the Exam                            |
| Make it your own                                            | Expect learning through osmosis.                  |
| Review the Supplemental Material                            | Ignore the Supplemental Material.                 |

# Data Stores

## Concepts

Types of data stores:
 
	*  Persistent Data Store - Data is durable and sticks around after reboots, restarts, or power cycles (Glacier, RDS)
 
	* Transient data Store - Data is just temporarily stored and passed along to another process or persistent store (SQS, SNS)

	* Ephemeral Data Store - Data is lost when stopped. (EC2 Instance Store, Memcached)

Input/Output Operations per Second (IOPS) - Measure of how fast we can read and write to a device.

Throughput - Measure of how much data can be moved at a time.

Consistentcy Models:

  * ACID
	
		* Atomic - Transactions are "all or nothing"

		* Consistent - Transactions must be valid.

		* Isolated - Transactions can't mess with one another.

		* Durable - Completed transaction must stick around. 
	
	* BASE

		* Basic Availability - values availability even if stale

		* Soft-state - might not be instantly consistent across stores

		* Eventual Consistency - will achieve consitency at some point

## S3

One of the first AWS service introduced back in 2006
 
S3 is an Object Store
 
Used in AWS services - directly and behind-the-scenes
 
Maximum object size is 5TB, largest object in a single PUT is 5GB.

Recommended to use muti-part uploads if larger than 100MB

Security

	* Resource-based (Object ACL, Bucket Policy)
 
	* User-based (IAM policies)
 
	* Optional Multi-factor authentication before Delete

Versioning

	* New version with each write
 
	* Enables "roll-back" and "un-delete" capabilities
 
	* Old versions count as billable size until they are permanently deleted.
 
	* Integrated with Lifecycle Management
 
Optionally require Multi-factor Authentication:

	* Safeguard against accidental deletion of an object

	* Change the versioning state of your bucket

Cross-Region Replication

	* Security
 
	* Compliance
 
	* Latency

S3 Storage Classes

	* Standard - frequently accessed data
 
	* Standard-IA - long-lived, infrequently accessed data
 
	* One-Zone-IA - long-lived, infrequently accessed, non-critical data
 
	* Reduced redundancy - Frequently accessed, non-critical data
 
	* Intelligent-Tiering - Long-lived data with changing or unknown access patterns
 
	* Glacier - Long-term data archiving with retrieval times ranging from minutes to hours
 
	* Glacier Deep Archive - Long-term data with retrieval times within 112 hours

S3 Intelligent Tiering Archive - Automatically moves data to Glacier or DeepGlacier

S3 Lifecycle Management:

	* Optimize storage costs
 
	* Adhere to data retention policies
 
	* Keep S3 volumes well-maintained
 
	* Rules based on prefixes, tags, or previous version numbers

S3 Analytics

| Concept                         | AWS Services                          |
| ---                             | ---                                   |
| Data Lake Concept               | Athena, Redshift Spectrum, QuickSight |
| IoT Streaming Data Repository   | Kinesis Firehose                      |
| Machine Learning and AI Storage | Rekognition, Lex, MXNet               |
| Storage Class Analysis          | S3 Managent Analysis                  |

S3 Encryption at Rest

| Encryption Option | Meaning                                                                                              |
| ---               | ---                                                                                                  |
| SSE-S3            | Use S3's existing encryption for AES-256                                                             |
| SSE-C             | Upload your own AES-256 encryption key which S3 will use when it writes the objects                  |
| SSE-KMS           | Use a key generated and managed by AWS Key Management Service                                        |
| Client-Side       | Encrypt objects using your own local encryption process before uploading to S3 (i.e. PGP, GPG, etc.) |

More Nifty S3 Tricks:

	* Transfer Acceleration - Speeds up data uploads using CloudFront in reverse
 
	* Requester Pays - The requester rather than the bucket owner pays for requests and data transfer
 
	* Tags - Assign tags to objects for use in costing, billing, security, etc.
 
	* Events - Trigger notifications to SNS, SQS, or Lambda when certain events happen in your bucket
 
	* Static Web Hosting - Simple and massively scalable static website hosting
 
	* BitTorrent - Use the BitTorrent protocol to retrieve any publicly available object by automatically generating a .torrent file.

## Amazon Glacier

Cheap, slow to response, seldom accessed

"Cold Storage"

Used by AWS Storage Gateway Virtual Tape Library

Integrated with AWS S3 via Lifecycle Management

Faster retrieval speed if you pay more

Vault = bucket

Vault lock enforces rules. Immutable.

Archive - the data in glacier, max size 40TB, immutable

Access through IAM

Policy defines what rules the vault must obey by (No deletion, MFA)

Vault Lock process - create vault lock, initiate vault lock, 24 hours to abort or complete vault lock

## Amazon EBS

Think "virtual hard drives"

Can only be used with EC2

Tied to a single AZ

Variety of Optimized choices for IOPS, Throughput, and Cost

Snapshots are great!

Ebs vs instance stores

	* Instance stores - temporary, ideal for caches, buffers, work areas, data goes away when EC2 is stopped or terminated, locked to an EC2 instance, better performance (directly attached to the instance)
 
	* EBS - not locked to an instance, data persists, snapshots, worse performance (rides over the network)
	
EBS Snapshots

	* Cost-effective and easy backup strategy
 
	* Share data sets with other users or accounts
 
	* Migrate a system to a new AZ or Region
 
	* Convert unencrypted volume to an encrypted volume

Amazon Data Lifecycle Manager - Schedule snapshots for volumes or instances every X hours, Retention rules to remove stale snapshots

## Amazon Elastic File Service (EFS)

Implementation of NFS file share

Elastic storage capacity, and pay for only what you use (in contrast to EBS)

Multi-AZ metadata and data storage

Configure mount-points in on, or many, AZs

Can be mounted from on-premises systems (Caution here..., unencrypted in transit by default so use tunnel if you don't want sent through the raw internet, need fast internet connection)

Alternatively, use Amazon DataSync

EFS is 3x more expensive than EBS, and 20x more expensive than S3

Some NFSv4 features not supported, so check the documentation

## Amazon Storage Gateway

Virtual machine that you run on-premises with VMWare or HyperV OR via a specially configured Dell hardware appliance

Provides local storage resources backed by AWS S3 and Glacier

Often used in disaster recovery preparedness to sync to AWS

Useful in cloud migrations

Modes:

| New Name                   | Old Name                     | Interface | Function                                                                         |
| ---                        | ---                          | ---       | ---                                                                              |
| File Gateway               | None                         | NFS, SMB  | Allow on-prem or EC2 instances to store objects in S3 via NFS or SMB mount point |
| Volume Gateway Stored Mode | Gateway-stored Volumes       | iSCSI     | Async replication of on-prem data to S3                                          |
| Volume Gateway Cached Mode | Gateway-cached Volumes       | iSCSI     | Primary data stored in S3 with frequently accessed data cached locally on-prem   |
| Tape Gateway               | Gateway-Virtual Tape Library | iSCSI     | Virtual media changer and tape library for use with existing backup software     |

Contains bandwidth throttling, so good solution if you don't want to take down the internet connection in e.g. remote offices


## Amazon WorkDocs

Amazon's DropBox or Google Drive

Secure fully managed file collaboration service

Can integrate with AD for SSO

Web, mobile and native clients (no Linux client)

HIPAA, PCI DSS, and ISO compliance requirements

Available SDK for creating complementary apps

## Database on EC2

Run any database with full control and ultimate flexibility

Must manage everything like backups, redundancy, patching, scale

Good option if you require a database not yet supported by RDS, such as IBM DB2 or SAP HANA

Good option if it is not feasible to migrate to AWS-managed database

## RDS

Managed database option for MySQL, Maria, PostgreSQL, Microsoft SQL Server, Oracle and MySQL compatible Aurora

Best for structured, relational data store needs

Aims to be drop-in replacement for existing on-prem instances of same databases

Automated backups and patching in customer-defined maintenance windows

Push-button scaling, replication and redundancy

Anti-Patterns:

| If you need...                                    | Don't use RDS, instead use |
| ---                                               | ---                        |
| Lots of large binary objects (BLOBs)              | S3                         |
| Automated scalability                             | DynamoDB                   |
| Name/Value Data Structure                         | DynamoDB                   |
| Data is not well structured or unpredictable      | DynamoDB                   |
| Other database platforms like IBM DB2 or SAP HANA | EC2                        |
| Complete control over the database                | EC2                        |

Multi-AZ RDS

Read-Replicas service regional users

Note for MySQL: non-transactional storage engines like MyISAM don't support replication; you must use InnoDB (or XtraDB on Maria)

MariaDB is an open-source fork of MySQL. MySQL behaves similar

Types of replication: sync (instantly replicated to standbys after transaction is committed) and async (lagging a minute or less with regards to the writes)

In multi-az deployment:
	if one AZ fails, stand-by in another AZ assumes the role of Master, read replicas keep on keeping on
	
	if whole region failed, read replica promoted to Stand-Alone (single-AZ), single AZ reconfigured to multi-az
	
	
## Amazon DynamoDB

Managed, multi-AZ NoSQL data store with Cross-Region Replication option

Defaults to eventual consitency but can request strongly consitent read via SDK parameter

Priced on throughput, rather than compute

Provision read and write capacity in anticipation of need

Autoscale capacity adjusts per configured min/max levels

On-Demand Capacity for flexible capacity at a small premium cost

Achieve ACID compliance with DynamoDB Transactions

Secondary Indexes

| Index Type             | Description                                                         | How to Remember                                                                                       |
| ---                    | ---                                                                 | ---                                                                                                   |
| Global Secondary Index | Partition key and sort key can be different from those on the table | I'm not restricted to just the paritioning set forth by the partition key. I'm GLOBAL BABY!           |
| Local Secondary Index  | Same partition key as the table but different sort key              | I have to stay local and respect the table's partition key, but I can choose whatever sort key I want |

There is a limit to the number of indexes and attributes per index

Indexes take up storage space

| Index Type             | When to Use                                                                                                                       | Example                                                                                                     |
| ---                    | ---                                                                                                                               | ---                                                                                                         |
| Global Secondary Index | When you want a fast query of attributes outside the primary key without having to do a table scan (read everything sequentially) | "I'd like to query Sales Orders by customer number rather than Sales Order Number."                         |
| Local Secondary Index  | When you already know the partition key and wnt to quickly query on some other attribute                                          | "I have the Sales Order Number, but I'd like to retrieve only those records with a certain Materal Number." |

| If you need to..                                      | Consider...                                                                      | Cost                                        | Benefit                                               |
| ---                                                   | ---                                                                              | ---                                         | ---                                                   |
| access just a few attributes the fastest way possible | Projecting just those few attributes in a global secondary index                 | Minimal                                     | Lowest possible latency access for non-key items      |
| frequently access some non-key attributes             | Projecting those attributes in a global secondary index                          | Moderate; aim to offset cost of table scans | Lowest possible latency access for non-key items      |
| frequently access most non-key attributes             | Projecting those attributes or even the entire table in a global secondary index | Up to Double                                | Maximum flexibility                                   |
| Rarely query but write or update frequently           | Projecting keys only for the global secondary index                              | Minimal                                     | Very fast write or updates for non-parition-key items |

## Amazon Redshift

Fully managed, clustered peta-byte scale data warehouse

Extremely cost-effective as compared to some other on-premises data warehouse platforms

PostgreSQL compatible with JDBC and ODBC drivers available; compatible with most BI tools out of the box

Features parallel processing and columnar data stores which are optimized for complex queries

Option to query directly from data files on S3 via Redshift Spectrum

Data Lake:

	* Query raw data without extensive pre-processing
 
	* Lessen time from data collection to data value
 
	* Identify correlations between disparate data sets

## Neptune

Fully-managed graph database

Supports open graph APIs for both Gremlin and SPARQL

## Amazon Elasticache

Fully managed implementations of two popular in-memory data stores - Redis and Memcached

Push-button scalability for memory, writes and reads

In Memory key/value store - not persistent in the traditional sense...

Billed by node size and hours of use

Use cases:

| Use                       | Benefit                                                                                                                                                                 |
| ---                       | ---                                                                                                                                                                     |
| Web Session Store         | In cases with load-balanced web servers, store web session information in Redis so if a server is lost, the session info is not lost and another web server can pick-up |
| Database caching          | Use Memcached in front of AWS RDS to cache popular queries to offload work from RDS and return results faster to users                                                  |
| Leaderboards              | Use Redis ot provide a live leaderboard for millions of users of your mobile app                                                                                        |
| Streaming Data Dashboards | Provide a landing spot for streaming sensor data on the factory floor, providing live real-time dashboard displays                                                      |


Memcached

	* Simple, no frills, straight forward
 
	* You need to scale out and in as demand changes
 
	* You need to run multiple CPU cores and threads
 
	* You need to cache objects (i.e. like database queries)

Redis

	* You need encryption
 
	* You need HIPAA compliance
 
	* Support for clustering
 
	* You need complex data types
 
	* You need high-availability (replication)
 
	* Pub/Sub capability
 
	* Geospacial Indexing
 
	* Backup and Restore

## Other Datastore Options

### Amazon Athena

SQL Engine overlaid on S# based on Presto

Query raw data objects as they sit in an S3 bucket

Use or convert your data to Parquet format if possible for a big performance jump

Similar in concept to Redshift Spectrum but

| Use Amazon Athena                                                                 | Use Redshift Spectrum                                                       |
| ---                                                                               | ---                                                                         |
| Data lives mostly on S3 without the need to perform joins with other data sources | Want to join S3 data with existing RedShift tables or create union products |

### Amazon Quantum Ledger Database (QLDB)

Based on blockchain concepts

Provides an immutable and transparent journal as a service without having to setup and maintain an entire blockchain framework

Centralized design (as opposed to decentralize consensus-based design for common blockchain frameworks) allows for higher performance and scalability

Append-only concept where each record contributes to the integrity of the chain

### Amazon Managed Blockchain

Fully managed blockchain framework supporting open source frameworks of Hyperledger Fabric and Ethereum

Distributed consensus-based concept consisting of a network, members (other AWS accounts), nodes (instances) and potentially applications.

Uses the Amazon QLDB ordering service to maintain complete history of all transactions

### Amazon Timestream Database

Fully managed database service specifically built for storing and analyzing time-series data

Alternative to DynamoDB or RedShift and includes some built-in analytics like interpolation and smoothing

### Amazon DocumentDB

"with MongoDB compatibility"

AWS's invention that emulates the MongoDB API so it acts like MongoDB to existing clients and drivers

Fully managed with all the good stuff (multi-AZ HA, scalable, integrated with KMS, backed up to S3)

An option if you currently use MongoDB and want to get out of the server management business

### Amazon ElasticSearch (OpenSearch)

Not to be confused with ElastiCache

Mostly a search engine but also a document store (caution here)

Amazon ElasticSearch Service components are sometimes referred to as an ELK stack

Search and storage - ElasticSearch

Intake - LogStash, CloudWatch, Firehose, IoT

Analytics - Kibana

Search engine that is large, scalable, and has a mature development community, ES

## Database Comparisons

Database on EC2

	* Ultimate control over database
 
	* Preferred DB not available under RDS

Amazon RDS

	* Need traditional relational database for OLTP (Online Transaction Processing)
 
	* Your data is well-formed and structured

Amazon DynamoDB

	* Name/value pair data or unpredictable data structure
 
	* In-memory performance with persistence

Amazon Redshift

	* Massive amounts of data
 
	* Primarily OLAP (Online Analytical Processing) workloads

Amazon Neptune

	* Relationships between objects a major portion of data value

Amazon Elasticache

	* Fast temporary storage for small amounts of data
 
	* Highly volatile data

## Exam Tips

Read the AWS Storage Options white paper and note anti-patterns

Know when to use various data stores

RDS:

	* Traditional relational data models
 
	* Existing apps requiring RDBMS
 
	* OLTP, ACID-compliant

DynamoDB:

	* High I/O needs
 
	* Scale dynamically

S3:

	* BLOBs

EC2:

	* Database not supported under RDS
 
	* Need complete control

Redshift:

	* OLAP

Read the Whitepapers!

https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf

https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf 

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

https://d1.awsstatic.com/whitepapers/cost-optimization-storage-optimization.pdf

### Pro Tips - Storage

Archiving and Backup often a great "pilot" to build AWS business case

Make use of the S3 endpoints within your VPC

Learn how to properly secure your S3 bucket

Encrypt, Encrypt, Encrypt

### Pro Tips - Databases

Consider Aurora for your production MySQL/Maria or PostgreSQL needs

Consider NoSQL if you don't need relational database features

Databases on EC2 cost less on the surface than RDS, but remember to factor in management (backups, patching, OS-level hardening)

There can be a performance hit when RDS backups run if you have only a single AZ instance.

# Networking

## Concepts

You should already know:

	* Physical layout of AZs and Regions

	* VPC concept and how to create

	* Create private and public subnets

	* What a NAT is and what "Disable Source/Destination Checks" means
	
	* Route table and routing terminology (default routes, local routes)

	* IPv4 Addressing and Subnet Mask Notation (/16, /24, etc)

	* Intermediate Networking Terminology (MAC address, port, gateway vs. router)

OSI Model:

| Layer | Name         | Example                                         | Mneumonic |
| ---   | ---          | ---                                             | ---       |
| 7     | Application  | Web Browser                                     | Away      |
| 6     | Presentation | TLS/SSL Compression                             | Pizza     |
| 5     | Session      | Setup, Negotiation, Teardown                    | Sausage   |
| 4     | Transport    | TCP                                             | Throw     |
| 3     | Network      | IR, ARP                                         | Not       |
| 2     | Data Link    | MAC                                             | Do        |
| 1     | Physical     | CAT5, fiber optic cable, 5GHz carrier frequency | Please    |

AWS handles 1 & 2, Customer handles the rest


Unicast - Directed communication (a phone call)

Multicast - Communicating with multiple devices on the network at once (shouting on the street)

Multicast is not usually possible on VPC (some workarounds exist)

| Protocol                                               | Characteristics                                            | Plain Speak                                                                         | Uses                      |
| ---                                                    | ---                                                        | ---                                                                                 | ---                       |
| TCP (Layer 4)                                          | Connection-based, stateful, acknowledges receipt           | After everything I say, I want you to confirm that you received it.                 | Web, Email, File Transfer |
| UPD (Layer 4)                                          | Connectionless, stateless, simple, no retransmission delay | I'm going to start talking, it's ok if you miss some words                          | Streaming media, DNS      |
| ICMP (officially, it's Layer 3 but people debate this) | Used by network devices to exchange info                   | We routers can keep in touch about the health of the network using our own language | traceroute, ping          |


### Ephemeral Ports

Short-lived transport protocol ports used in IP communications

Above the "well-known" IP ports (above 1024)

"Dynamic Ports"

"Suggested range is 49152 to 65535 but:

	* Linux kernels generally use 32568 to 61000
 
	* Windows platforms default from 1025

NACL and Security Group implications

### Reserved IP Addresses

AWS uses certain IP addresses in each VPC as reserved -- you can't use them

5 IPs are reserved in every VPC subnet (example 10.0.0.0/24):
	
	* 10.0.0.0: Network Address
 
	* 10.0.0.1: Reserved by AWS for the VPC router

	* 10.0.0.2: Reserved by AWS for Amazon DNS

	* 10.0.0.3: Reserved by AWS for future use

	* 10.0.0.255: VPCs don't support broadcast so AWS reserves this address

Example: 192.168.8.16/28

	* 192.168.8.16: Network
 
	* 192.168.8.17: Router
 
	* 192.168.8.18: DNS
 
	* 192.168.8.19: Future use 
 
	* 192.168.8.20-30: Available

	* 192.168.8.31: Broadcast

### AWS Availability Zones

The Physical to Logical assignment of AZ's is done at the Account level (Your us-west-2a might be a different AZ then someone else's us-west-2a)

## Network to AWS Connectivity

### AWS Managed VPN

What: AWS managed IPsec VPN connection over your existing internet

When: Quick and usually simple way to establish a secure tunneled connection to a VPC; Redundant link for Direct Connect or other VPC VPN

Pros: Supports static routes or BGP peering and routing

Cons: Dependent on your Internet connection

How: 

	1. Designate an appliance to act as your customer gateway (usually your on-prem router)
 
	2. Create the VPN connection in AWS and download the configuration file for your customer gateway 

	3. Configure your customer gateway using the information from the configuration file
	
	4. Generate traffic from your side of the VPN connection to bring up the VPN tunnel
	
	5. Configure BGP routing (if needed)

### AWS Direct Connect

What: Dedicated network connection over private lines straight into the AWS backbone

When: Require a "big pipeline" into AWS, lots of resources and services being provided on AWS to your corporate users

Pros: More predictable network performance, potential bandwidth cost reduction; up to 10 Gbps provisioned connections; Supports BGP peering ad routing

Cons: May require additional telecom and hosting provider relationships and/or new network circuits

How: Work with your existing Data Networking Provider; Create Virtual Interfaces (VIF) to connect to VPCs (private VIF) or other AWS service like S3 or Glacier (public VIF)


### AWS Direct Connect Plus VPN

What: IPsec VPN connection over private lines

When: Want added security of encrypted tunnel over Direct Connect

Pros: More secure (in theory) than Direct Connect alone

Cons: More complexity introduced by VPN layer

How: Work with your existing Data Networking Provider

### AWS VPN CloudHub

What: Connect locations in a Hub and Spoke manner usin AWS's Virtual Private Gateway

When: Link remote offices for backup or primary WAN access to AWS resources and each other

Pros: Reuses existing internet connection; Supports BGP routes to direct traffic (for example, use MPLS first then CloudHub VPN as backup)

Cons: Dependent on internet connection; No inherent redundancy

How: Assign multiple Customer Gateways to a Virtual Private Gateway, each with their own BGP ASN and unique IP ranges

### Software VPN

What: You provide your own VPN endpoint and software

When: You must manage both ends of the VPN connection for compliance reasons or you want to use a VPN option not supported by AWS

Pros: Ultimate flexibility and manageability

Cons: You must design for any needed redundancy across the whole chaino

How: Install VPN software via Marketplace appliance or on an EC2 instance

### Transit VPC

What: Common strategy for connecting geographically disperse VPCs and locations in order to create a global network transit center

When: Locations and VPC deployed assets across multiple regions that need to communicate wth one another

Pros: Ultimate flexibility and manageability but also AWS-managed VPN hub-and-spoke between VPCs

Cons: You must design for any needed redundancy across the whole chain

How: Providers like Cisco, Juniper Networks and Riverbed have offerings which work with their equipment and AWS VPC

## VPC to VPC Connectivity
