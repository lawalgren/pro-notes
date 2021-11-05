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

### VPC Peering

What: AWS-provided network connectivity between two VPCs

When: Multiple VPCs need to communicate or access each others resources

Pros: Uses AWS backbone without touching the Internet

Cons: If A is connected to B and B is connected to C, A cannot talk to C via B. (transitive peering not supported)

How: VPC Peering is made; Accepter accepts request (either within Account or across Accounts); Both sides set up appropriate route

### AWS PrivateLink

What: AWS-provided network connectivity between two VPCs and/or AWS services using interface endpoints

When: Keep Private Subnets truly private by using the AWS backbone to reach other services rather than the public internet

Pros: Redundant - uses AWS backbone

Cons: ~VPC Endpoints only available within the region they are created~ As of October 2018, they can be accessed over inter-region VPC peering

How: Create Endpoint for needed AWS or Marketplace service in all needed subnets; access via the provided DNS hostname

VPC Endpoints

|               | Interface Endpoint                            | Gateway Endpoint                                         |
| ---           | ---                                           | ---                                                      |
| What          | Elastic Network Interface with a Private IP   | A gateway that is a target for a specific route          |
| How           | Uses DNS to redirect traffic                  | Uses prefix lists in the route table to redirect traffic |
| What Products | API Gateway, CloudFormation, CloudWatch, etc. | Amazon S3, DynamoDB                                      |
| Securing      | Security Groups                               | VPC Endpoint Policies                                    |

## Internet Gateways

### Internet Gateway

Horizontally scaled, redundant and highly available component that allows communication between your VPC and the internet

No availability risk or bandwidth constraints

If your subnet is associated with a route to the internet, then it is a public subnet

Supports IPv4 and IPv6

Purpose 1: Provide route table target for Internet-bound traffic

Purpose 2: Perform NAT for instances with <u>public</u> IP addresses

__<u>Does not perform NAT for instances with private IP's only.</u>__

### Egress-Only Internet Gateway

IPv6 addresses are globally unique and are therefore public by default

Provides outbound internet access for IPv6 addressed instances

Prevents inbound access to those IPv6 instances

Stateful - forwards traffic from instance to internet and then sends back the response

Must create a custom route for ::/0 to the Egress-Only Internet Gateway

Use Egress-Only Internet Gateway instead of NAT for IPv6

### NAT Instance

EC2 instance from a special AWS-provided AMI

Translate traffic from many private IP instances to a single public IP and back

Doesn't allow public Internet initiated connections into private instances

Not supported for IPv6 (use Egress-Only Gateway)

NAT Instance must live on a public subnet with route to Internet Gateway

Private instances in private subnet must have route to the NAT instance, usually the default route destination of 0.0.0.0/0

### NAT Gateway

Fully-managed NAT service that replaces need for NAT Instance on EC2

Must be created in a public subnet

Uses an Elastic IP for public IP for the life of the Gateway

Private instances in private subnet must have route to the NAT Gateway, usually the default route destination of 0.0.0.0/0

Created in specified AZ with redundancy in that zone

For multi-AZ redundancy, create NAT Gateways in each AZ with routes for private subnets to use the local Gateway

Up to 5Gbps bandwidth that can scale up to 45 Gbps

Can't use a NAT Gateway to access VPC peering, VPN or Direct Connect, so be sure to include specific routes to those in route table (remember: most specific route is selected first)

NAT Gateway vs NAT Instance

|                 | NAT Gateway                              | NAT Instance                               |
| ---             | ---                                      | ---                                        |
| Availability    | Highly available within AZ               | On your own                                |
| Bandwidth       | Up to 45 Gbps                            | Depends on bandwidth of instance type      |
| Maintenance     | Managed by AWS                           | On your own                                |
| Performance     | Optimized for NAT                        | Amazon Linux AMI configured to perform NAT |
| Public IP       | Elastic IP that __can not__ be detatched | Elastic IP that __can__ be detatched       |
| Security Groups | Cannot be associated with NAT gateway    | Can use Security Groups                    |
| Bastion Server  | Not Supported                            | Can be used as bastion server              |

## Routing

### Routing Tables

VPCS have an implicit router and main routing table

You can modify the main routing table or create new tables

Each route table must contain a local route for the CIDR block

Most specific route for an address wins

| Destination                                                                                                                                                                                                            | Target                              |
| ---                                                                                                                                                                                                                    | ---                                 |
| 10.0.0.0/16                                                                                                                                                                                                            | local                               |
| 192.168.0.0/24                                                                                                                                                                                                         | vpg-xxxxx (virtual private gateway) |
| 0.0.0.0/0                                                                                                                                                                                                              | nat-xxxxx (nat gateway)             |
| pl-xxxxx (prefix list, predefined mapping of ip addresses for a particular service, intercepts traffic that would go through the default route -- often the internet -- and routes it through vpc endpoint connection) | vpce-xxxxx (vpc endpoint)           |

| Address                   | Route      |
| ---                       | ---        |
| 10.0.45.34                | local      |
| 64.56.34.1                | nat-xxxxx  |
| 192.168.0.7               | vpg-xxxxx  |
| Resolved IP address of S3 | vpce-xxxxx |
| 10.0.255.255              | nowhere    |

### Border Gateway Protocol

Popular routing profile for the Internet

"Propogates" information about the network to allow for dynamic routing

Required for Direct Connect and optional for VPN

Alternative of not using BGP with AWS VPC is static routes

AWS supports BGP community tagging as a way to control traffic scope and route preference

Required TCP port 179 + ephemeral ports (remember these?)

Autonomous System Number (ASN) = Unique endpoint identifier

Weighting is local to the router and higher weight is preferred path for outbound traffic

## Enhanced Networking

Generally used for High Performance Computing use-cases

Uses single root I/O virtualization (SR-IOV) to deliver higher performance than traditional virtualized network interfaces

Might have to install driver if other than Amazon Linux HVM AMI

Intel 82599 VF Interface - 10 Gbps

Elastic Network Adapter - 25 Gbps

Placement Groups:

|      | Clustered                                                        | Spread                                                            | Partition                                                               |
| ---  | ---                                                              | ---                                                               | ---                                                                     |
| What | Instances are placed into a low-latency group within a single AZ | Instances spread across underlying hardware                       | Instances are grouped into partitions and spread across racks           |
| When | Need low network latency and/or high network throughput          | Reduces risk of simultaneous failure if underlying hardware fails | Reduce risk of correlated hardware failure for multi-instance workloads |
| Pros | Get the most outof Enhanced Networking Instances                 | Can span multiple AZ's                                            | Better for large distributed or replicated workloads than Spread        |
| Cons | Finite capacity: recommend launching all you might need up front | Max of 7 instances running per group per AZ                       | Not supported for Dedicated Hosts                                       |

## Route 53

Register domain names

Check the health of your domain resources

Route internet traffic for your domain

You should already know:

	* What is a DNS?
 
	* DNS record types (A, CNAME, MX, TXT, etc.)
 
	* Route 53 Concepts (alias, hosted zone, etc.)

	* Why is it called Route 53?

Route 53 Routing Policies:

| Policy            | Route 53 is thinking...                                                                                                                                                                                          |
| ---               | ---                                                                                                                                                                                                              |
| Simple            | Simple. Here's the destination for that name                                                                                                                                                                     |
| Failover          | Normally, I'd route you to <Primary>, but it appears down based on my Health Checks so I'll failover to <Backup>                                                                                                 |
| Geolocation       | Looks like you're in Europe, so I'm going to route you to a resource closer to you in that region                                                                                                                |
| Geoproximity      | You're closer to the US-EAST-1 region than US-WEST-2 so I'll route you to US-EAST-1 (can bias positive or negative, between +-99, which widens or shrinks the circle for routing traffic to a particular region) |
| Latency           | Let me see which resources has lower latency from you, then I'll direct you that way.                                                                                                                            |
| Multivalue Answer | I will return several IP addresses, as a sort of basic load balancer                                                                                                                                             |
| Weighted          | You can setup multiple resources and I'll route according to the percentage of weight you assign each. (Weights from 0 to 255, 0 disables the route)                                                             |

Percentage of traffic = Weight for specified record / Sum of all weights for all records

| Weight for Endpoint 1 | Weight for Endpoint 2 | Percentage to Endpoint 1 | Percentage to Endpoint 2 |
| ---                   | ---                   | ---                      | ---                      |
| 10                    | 10                    | 50%                      | 50%                      |
| 250                   | 101                   | ~71.2%                   | ~28.8%                   |
| 0                     | 1                     | 0%                       | 100%                     |
| 11                    | 10                    | ~52.3%                   | 47.6%                    |

## CloudFront

Distributed content delivery service for simple static asset caching up to 4k live and on-demand video streaming

You should already know how to create a CloudFront distribution and understand edge location concept

Integrated with Amazon Certificate Manager and supports SNI

If you want to use with custom domain, need to use custom SSL cert, either from Certificate Manager or a 3rd party issuer uploaded to IAM

SNI - allows the client to specify which host it wants to connect to, and the server can present multiple certificates on the same IP. Some old browsers don't support it, but people rarely worry about those.

Security Policy - can adjust which SSL/TLS versions the CloudFront distribution supports

All security policies support TLSv1.2, and browsers tend to use the most modern protocol available and fallback as needed. (TLS_2018 only supports TLSv1.2)

## Elastic Load Balancer

Distributes inbound connections to one or many backend endpoints

Three different options:

	* Application Load Balancer (Layer 7)
 
	* Network Load Balancer (Layer 4)

	* Classic Load Balancer (Layer 4 or Layer 7)

Can be used for public or private workloads

Consume IP addresses within a VPC subnet

Similarities:

|                                | Application LB | Network LB | Classic LB         |
| ---                            | ---            | ---        | ---                |
| Zonal Failover                 | Yes            | Yes        | Yes                |
| Platform                       | VPC Only       | VPC Only   | EC2-Classic or VPC |
| Health Checks                  | Yes            | Yes        | Yes                |
| Cross-Zone Load Balancing      | Yes            | Yes        | Yes                |
| CloudWatch Metrics             | Yes            | Yes        | Yes                |
| SSL Offloading                 | Yes            | Yes        | Yes                |
| Resource-based IAM Permissions | Yes            | Yes        | Yes                |

Differences:

|                              | Application LB                      | Network LB        | Classic LB            |
| ---                          | ---                                 | ---               | ---                   |
| Protocols                    | HTTPS, HTTP                         | TCP, UDP, TLS     | TCP, SSL, HTTP, HTTPS |
| Path or Host-based Routing   | Yes                                 | No                | No                    |
| WebSockets                   | Yes                                 | No                | No                    |
| Server Name Indication (SNI) | Yes                                 | Yes               | No                    |
| Sticky Sessions              | Yes                                 | Yes, as of 9/2019 | No                    |
| Sticky IP, Elastic IP        | Only through AWS Global Accelerator | Yes               | No                    |
| User Authentication          | Yes                                 | No                | No                    |

### Routing

Network Load Balancers:
	
	* Port number

	* TCP connections to backend are persisted for the duration of the connection
	
	* Route based on the destination port

	* Excells at speed

Application Load Balancers:
	
	* Host-based routing
 
	* Path-based routing

	* HTTP header-based routing

	* HTTP method-based routing

	* Query string parameter-based routing

	* Source IP address CIDR-based routing

Sticky sessions - uniquely identify the client based on session id and ensure that traffic is routed to the same instance while session is alive

## Exam Tips

### VPCS in General

Know the pros and cons of each On-prem to AWS connection mode

Know the functions of the different VPC components (Customer Gateway, Virtual Private Gateway)

Know that Direct Connect is not inherently redundant, so know how to architect a network that *is* (VPN, secondary direct connect)

Multicast and Broadcast aren't supported in VPCs

Know what is meant by "stateless", "stateful", "connectionless", and "connection-based" in terms of IP protocols

Know what ephemeral ports are and why they might need to be in NACLs or SGs

### Routing

Understand BGP and how to use weighting to shift network traffic

Know how routes in a route table are prioritized (most specific first)

What other routing protocols does AWS support (none... only BGP)

### VPC Peering

CIDR ranges cannot overlap

After VPC owner accepts a peering request, routes must be added to respective route tables

Transitive peering is not supported, but mesh or hub-and-spoke architectures are... with proper NACLs and routes

A Transit VPC is supported

### Internet Gateways

Difference between a NAT instance and NAT Gateway

Internet Gateway is horizontally scaled, redundant, with no bandwidth constraints

NATs do have bandwidth constraints, but...

VPCs can have multiple NATs across AZs and subnets for scale -- so long as routes are defined properly

Use Egress-Only Gateway for IPv6

### Route 53

Understand different types of routing policies and use cases

Know the Weighted Routing formula

Route 53 is a global service

### CloudFront

Understand what must happen to use a custom domain with CloudFront

Understand what SNI enables and the necessary alternative

### Elastic Load Balancer

Know the different types of Load Balancers and at which OSI Layer they work

Understand which major features each deliver (protocol,SNI, Sticky Sessions)

Know what sticky sessions are and when they come into play

Whitepapers:

* https://d0.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf

* https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf

* https://docs.aws.amazon.com/vpc/latest/userguide/security.html

## Pro Tips

Direct Connect may be a more complex and costlier option to setup, but it could save big on bandwidth costs

Explicitly deny as much traffic as you can with NACLs and SG -- Principle of Least Privilege

Think through your VPC layout (see 2017 re:Invent video "Networking Many VPCs: Transit and Shared Architecture.)

You can use Route 53 for your domain even if AWS isn't your registrar

ELBs provide a useful layer of abstraction (as does Route 53 too!)

# Security

## Concepts

Principle of Least Privilege - Give users (or services) nothing more than those privileges required to perform their intended fuction (and only when they need them)

Security Facets:

| Facet          | Description                                        | AWS Example                                                          |
| ---            | ---                                                | ---                                                                  |
| Identity       | Who are you?                                       | Root Account User, IAM User, Temporary Security Credentials          |
| Authentication | Prove that you're who you say                      | Multi-factor Authentication, Client-side SSL Certificate             |
| Authorization  | Are you allowed to do this?                        | IAM Policies                                                         |
| Trust          | Do other entities that I trust say they trust you? | Cross-Account Access, SAML-based Federation, Web Identity Federation |

Components:

	* Identities - People, Objects, Other computers

	* Identity Provider - Contains identity store / brokers

	* Identity Store - Stores identities & metadata about identites

	* Identity Broker - Takes requests from identites / applications and run them against the identity store

	* Federation - The identity broker reaches out to other identity providers (facebook, google, cognito), the other provider returns a token/key, which lets the identity access the service


SAML 2.0:

	* Can handle both **authorization** and **authentication**
 
	* XML-based protocol
 
	*  Can contain user, group membership and other useful information

	*  Assertions in the XML for authentication, attributes or authorization

	* Best suited for Single Sign-on for enterprise users

OAuth 2.0:

	* Allows sharing of protected assets without having to send login credentials

	* Handles **authorization** only, not authentication
 
	* Issues token to client
 
	* Application validates token with Authorization Server

	* Delegate access, allowing the client applications to acccess infromation on behalf of user

	* Best suited for API authorization between apps

OpenID:

	* Identity layer built on top of OAuth 2.0, adding **authentication**

	* Uses REST/JSON message flows

	* Supports web clients, mobile clients, Javascript clients

	* Extensible

	* Best suited for Single Sign-on for consumer

Compliance:

	AWS has implemented certain processes, practices, and standards as prescribed by many national and international standards bodies - can see in AWS Artifact

## Multi-Account Management

Most large organizations will have multiple AWS accounts

Segregation of duties, cost allocation, and increase agility

Need methods to properly manage and maintain them

When should you use multiple accounts?

	* Do you require administrative isolation between workloads?

	* Do you require limited visibility and discoverability of workloads?

	* Do you require strong isolation to minimize "blast radius"?

	* Do you require strong isolation of recovery and/or auditing data?

AWS Tools for Account Management:

	* AWS Organizations

	* Service Control Policies
	
	* Tagging

	* Resource Groups

	* Consolidated Billing

Identity Account Structure:

	* Manage all user accounts in one location

	* Users trust relations from IAM roles in sub-accounts to Identity Account to grant temporary access

	* Variations include by Business Unit, Deployment Environment, Geography

Logging Account Structure:
	
	* Centralized logging repository

	* Can be secured so as to be immutable
	
	* Can use Service Control Policies (SCP) to prevent sub-accounts from changing logging settings

Publishing Account Structure:
	
	* Common repository for AMI's, Containers, Code

	* Permits sub-accounts to use pre-approved standardized services or assets

Information Security Account Structure:
	
	* Hybrid of consolidated security and logging

	* Allows ond poin of control and audit

	* Logs cannot be tampered with by sub-account users

Central IT Account Structure:
	
	* IT can manage IAM users and groups while assigning to sub-account roles

	* IT can provide shared services and standardized assets (AMI's, databases, EBS, etc.) that adhere to corporate policy

Service Control Policies cascade down the tree - i.e. define scp for parent account and it is also applied to all its children

## Network Controls and Security Groups

### Security Groups

Virtual firewalls for individual assets (EC2, RDS, AWS Workspaces, etc)

Controls inbound and outbound traffic for TCP, UDP, ICMP, or custom protocols

Port or port ranges

Inbound rules are by Source IP, Subnet, or other Security Group

Outbound rules are by Destination, IP, Subnet, or other Security Group

### Network Access Control Lists

Additional layer of security for VPC that acts as a firewall

Apply to entire subnets rather than individual assets

Default NACL allows all inbound and outbound traffic

NACLs are stateless -- meaning outbound traffic simple obeys outbound rules -- no connection is maintained

Can duplicate or further restrict access along with Security Groups

Remember ephemeral ports for Outbound if you need them

### Why use SG's and NACL's

NACLs provide a backup method of security if you accidentally change your SG to be too permissive

Covers the entire subnet so users to create instances and fail to assign a proper SG are still protected

Part of a multi-layer Least Privilege concept to explicitly allow and deny

## AWS Directory Services

| Directory Service Option | Description | Best for... |
| --- | --- | --- | --- |
| AWS Cloud Directory | Cloud-native directory to share and control access to hierarchical data between applications | Cloud applications that need hierarchical data with complex relationships |
| Amazon Cognito | Sign-up and sign-in functionality that scales to millions of users and federated to public social media services | Develop consumer apps or SaaS |
| AWS Directory Service for Microsoft Active Directory | AWS-managed full Microsoft AD (standard or enterprise) running on Windows Server 2012 R2 | Enterprises that want hosted Microsoft AD or you need LDAP for Linux apps |
| AD Connector | Allows on-premises users to log into AWS services with their existing AD credentials. Also allows EC2 instances to join AD domain | Single sign-on for on-prem employees and for adding EC2 instances to the domain |
| Simple AD | Low scale, low cost AD implementation based on Samba | Simple user directory, or you need LDAP compatibility |

### AD Connector vs Simple AD

AD Connector:

	* Must have existing AD

	* Existing AD users can access AWS assets via IAM roles

	* Supports MFA via existing RADIUS-based MFA infrastructure

Simple AD:

	* Stand-alone AD based on Samba

	* Supports user accounts, groups, group policies, and domains

	* Kerberos-based SSO

	* MFA not supported

	* No Trust Relationships

## Credentials and Access Management

Know what IAM is and components

Users, Groups, Roles, Policies

Resource-based Policies vs Identity-based Policies

Know how to read and write policies in JSON

Services->Actions->Resources

AWS Security Token Service (STS) - allows us to temporarily grant credential access to applications or users, sourced from IAM or one of the federated options

Amazon Cognito - designed to be used in mobile applications, cognito sdk already has a lot of the security handling built-in

Token Vending Machine Concept:

	* Common way to issue credentials for mobile app development

	* Anonymous TVM (token vending machine) - used as a way to provide access to AWS services only, does not store user identity

	* Identity TVM - used for registration and login, and authorizations

	* AWS now recommends that mobile developers use Cognito and the related SDK

AWS Secrets Manager:

	* Store passwords, encryption keys, API keys, SSH keys, PGP keys, etc.

	* Alternative to storing passwords or keys in a "vault" (software or physical)

	* Can access secrets via API with fine-grained access control provided by IAM

	* Automatically rotate RDS database credentials for MySQL, PostgreSQL and Aurora

	* Better than hard-coding credentials in scripts or application

## Encryption

Encryption at Rest - Data is encrypted where it is stored such as on EBS, on S3, in an RDS database, or in an SQS queue waiting to be processed

Encryption in Transit - Data is encrypted as it flows through a network or process, such as SSL/TLS for HTTPS, or with IPSec for VPN connections

Key Management Service (KMS):

	* Key storage, management, and auditing

	* Tightly integrated into MANY AWS services like Lambda, S3, EBS, EFS, DynamoDB, SQS, etc.

	* You can import your own keys or have KMS generate them

	* Control who manages and accesses keys via IAM users and roles

	* Audit use of keys via CloudTrail

	* Differs from Secret Manager as it's purpose-built for encryption key management

	* Validated by many compliance schemes (PCI DSS Level 1, FIPS 140-2 Level 2)

CloudHSM

	* Dedicated hardware device, Single Tenanted

	* Must be within a VPC and can access via VPC Peering
	
	* Does not natively integrate with many AWS services like KMS, but rather requires custom application scripting

	* Offload SSL from web servers, act as an issuing CA, enable TDE for Oracle databases

| | "Classic" Cloud HSM | Current Cloud HSM |
| --- | --- | --- |
| Device | safeNet Luna SA | Proprietary |
| Pricing | Upfront cost required ($5000) | No upfront cost, pay per hour |
| High Availability | Have to buy a second device | Clustered |
| FIPS 140-2 | Level 2 | Level 3 |

CloudHSM vs KMS:

| | CloudHSM | AWS KMS |
| --- | --- | --- |
| Tenancy | Single-Tenant HSM | Multi-Tenant AWS Service |
| Availability | Customer-managed durability and available | Highly available and durable key storage and management |
| Root of Trust | Customer managed root of trust | AWS managed root of trust |
| FIPS 140-2 | Level 3 | Level 2 / Level 3 in some areas |
| 3rd Party Support | Broad 3rd Party Support | Broad AWS Service Support |

AWS Certificate Manager:
	
	* Managed service that lets you provision, manage, and deploy public or private SSL/TLS certificates

	* Directly integrated into many AWS services like CloudFront, ELB, and API Gateway
	
	* Free public certificates to use with AWS services; no need to register via a 3rd party certificate authority

	* ...but you can import 3rd party certificates for use on AWS

	* Supports wildcard domains (\*.domain.com) to cover all your subdomains

	* Managed certificate renewal (no embarassing "certificate expired" messages for customers)

	* Can create a managed Private Certificate Authority as well for internal or proprietary apps, services, or devices

## Distributed Denial of Service Attacks

Mitigation:

| Best Practice | AWS Service |
| --- | --- |
| Minimize attack surface | NACLs, SGs, VPC Design |
| Scale to absorb attack | Auto Scaling Groups, AWS CloudFront, Static Web Content via S3 |
| Safeguard exposed resources | Route 53, AWS WAF, AWS Shield |
| Learn normal behavior | AWS GuardDuty, CloudWatch |
| Have a plan | All You! |

## IDS/IPS

**Intrusion Detection System** (IDS) watches the network and systems for suspicious activity that might indicate someone trying to compromise a system

**Intrusion Prevention System** (IPS) tries to prevent exploits by sitting behind firewalls and scanning and analyzing suspicious content fro threats

Normally comprised of a **Collection/Monitoring** system and **monitoring agents on each system**

Logs collected or analyzed in CloudWatch, S3 or third-party tools (Splunk, SumoLogic, etc.) sometimes called a **Security Information and Event Management (SIEM) system**

CloudWatch vs. CloudTrail:

| CloudWatch | CloudTrail |
| --- | --- |
| Log events across AWS services. Think operations | Log API activity across AWS services; Think activities |
| Higher-level comprehensive Monitoring and Eventing | More low-level granular |
| Log from multiple accounts | Log from multiple accounts |
| Logs stored indefinitely | Logs stored to S3 or CloudWatch indefinitely |
| Alarms history for 14 days | No native alarming; Can use CloudWatch alarms |

## Service Catalog

Framework allowing administators to create pre-defined products and landscapes for their users

Granular control over which users have access to which offerings

Makes use of adopted IAM roles so users don't need underlying service access

Allows end users to be self-sufficient while upholding enterprise standards for deployments

Based on CloudFormation templates

Administrators can version and remove products. Existing running product versions will not be shutdown

### AWS Service Catalog Constraints

| Type | What | Why |
| --- | --- | --- |
| Launch Constraint | IAM role that Service Catalog assumes when an end user launches a product | Without a launch constraint, the end-user must have all permissions needed within their own IAM credentials |
| Notification Constraint | Specifiesthe Amazon SNS topic to receive notifications about stack events | Can get notifications when products are launched or have problems |
| Template Constraint | One or more rules that narrow allowable values an end-user can select | Adjust product attributes based on choices a user makes. (Example: Only allow certain instances types for DEV environment) |

If we share a catalog, the launch role i  pulled from the original account, meaning resources are provisioned in the master account. Can override, creating a new launch constraint associated with a new launch role.

Launch constraints are associated with a product in the portfolio, not at the portfolio level, so can have individual roles defined for each and every product.

## Exam Tips

### Multi-Account Management

Know the different models an best practices for cross-account management of security

Know how roles and trusts are used to create cross-account relationships and authorizations

### Network Controls and Security

Know the differences and capabilities of NACLs and SGs

NACLs are stateless

Get some hands-on with NACLs and SGs to reinforce your knowledge

Remember the ephemerals

### AWS Directory Services

Understand the types of Directory Services offered by AWS -- especially AD Connector and Simple AD

Understand use-cases for each type of Directory Service

Be familiar with how on-prem Active Directory implementation might connect to AWS and what functions that might enable

### Credential and Access Management

Know IAM and its components

Know how to read and write IAM policies in JSON

Understand Identity Brokers, Federation, and SSO

Know options and steps for temporary authorization

### Encryption

Know differences between AWS KMS and CloudHSM and use cases

The test will likely be restricted to the "classic" CloudHSM

Understand AWS Certificate Manager and how it integrates with other AWS services

### DDoS Attacks

Understand what they are and some best practices to limit your exposure

Know some options to mitigate them using AWS services

### IDS/IPS

Understand the difference between IDS and IPS

Know what AWS services can help with each

Understand the differences between CloudWatch and CloudTrail

### Service Catalog

Know that it allows users to deploy assets through inheriting rights

Understand how Service Catalog can work in a multi-account scenario

### Whitepapers

* https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html

* https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf

* https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf

## Pro Tips

Know that security will be front-of-mind for every client evaluating the cloud... but rarely are there sound processes in place

Acknowledge concerns and be ready with a process (Cloud Adoption Framework is a good start.)

Leverage assessments and checklists as illustrations of care and best-practice

Migrating to the cloud is often more secure than on-prem due to increased transparency and visibility

Speak in terms of risk as a continuum rather than an absolute

Consider AWS Certified Security -- Specialty or other security-minded certification like CISSP

# Migrations

## Migration Strategies

| Migration Strategy | Description | Example |
| --- | --- | --- | --- |
| Re-Host | "Lift and Shift", simply move assets with no changes | Move on-prem MySQL database to EC2 Instance |
| Re-Platform | "Lift and Reshape", Move assets but change underlying platforms | Migrate on-prem MySQL database to RDS MySQL |
| Re-Purchase | "Drop and Shop", Abandon existing and purchase new. | Migrate legacy on-prem CRM system to salesforce.com |
| Rearchitect | Redesign application in a cloud-native manner | Create a server-less version of a legacy application |
| Retire | Get rid of applications which are not needed | End-of-life the label printing app because no one uses it anymore |
| Retain | "Do nothing option"; Decide to reevaluate at a future date | "Good new Servers, you'll live to see another day" |

| Migration Strategy | Effort (Time and Cost) | Opportunity to Optimize |
| --- | --- | --- |
| Re-Host | \*\* | \* |
| Re-Platform | \*\*\*\* | \*\*\* |
| Re-Purchase | \*\*\* | \* |
| Rearchitect | \*\*\*\*\* | \*\*\*\*\* |
| Retire | | |
| Retain | \* | |

## Cloud Adoption Framework

TOGAF:
	
	* The Open Group Architectural Framework

	* Approach for designing, planning, implementing and governing enterprise IT architectures

	* Started development in 1995

	* De factor standard in EA practice

	* Favored enterprise architecture for most Fortune 500 companies

	* Enterprise Architecture is not TOGAF...

	* But TOGAF often fills a vacuum

	* Often misunderstood and victim of unreasonable expectations

	*	Not all practitioners are great practitioners

	* TOGAF is not a cookbook

	* People take it too literally

	* "Architects gonna architect"

What is a Framework?

	* IS some information to help you get your mind around a problem

	* IS open fr localization and interpretation

	* IS something you should adapt to your organizational culture

	* IS NOT a perfect step-by-step recipe to success

	* IS NOT something to hide behind with big words

### Typical Cloud Adoption Phases

Project - In the project phase, you are running projects to get familiar and experience benefits from the cloud

Foundation - After experiencing the benefits of cloud, you then build the foundation to scale your cloud adoption. This includes creating a landing zone (a pre-configured, secure, multi-account AWS environment), Cloud Center of Excellence (CCoE), operations model, as well as assuring security and compliance methods

Migration - In this stage, you migrate existing applications including mission-critical applications or entire data centers to the cloud as you scale your adoption across a growing portion of your IT portfolio

Reinvention - Now that your operations are in the cloud, you can focus on reinvention by taking advantage of the flexibility and capabilities of AWS to transform your business by speeding time to market and increasing the attention on innovation

### Cloud Adoption Framework

There's more to cloud adoption than technology.

To fully unlock the potential benefits of a cloud migration, a holistic approach must be considered. 

Business:

	* Creation of a strong business case for cloud adoption

	* Business goals are congruent with cloud objectives

	* Ability to measure benefits (TCO, ROI)

People:

	* Evaluate organizational roles and structures, new skills and process needs to identify gaps

	* Incentives and Career Management aligned with evolving roles

	* Training options appropriate for learning styles

Governance:

	* Portfolio Management geared for determining cloud eligibility and priority

	* Program and Project management more agile projects

	* Align KPI's with newly enabled business capabilities

Platform:

	* Resource provisioning can happen with standardization

	* Architecture patterns adjusted to leverage cloud-native

	* New application development skill and processes enable more agility

Security:

	* Identity and Access Management modes change

	* Logging and audit capabilities will evolve

	* Shared Responsibility Model removes some and adds some facets

Operations:

	* Service monitoring has potential to be highly automated

	* Performance management can scale as needed

	* Business continuity and disaster recovery takes on new methods in the cloud

## Hybrid Architecture

Hybrid Architectures make use of cloud resources along with on-premises resources

Very common first step as a pilot for cloud migrations

Infrastructure can augment or simply be extensions of on-prem platforms -- VMWare, for example

Ideally, integrations are loosely coupled -- meaning each end can exist without extensive knowledge of the other side

Examples:
	
	* Storage Gateway creates a bridge between on-prem and AWS

		* Seamless to end-users

		* Common first step into tthe cloud due to low risk and appealing economics

	* Middleware often a great way to leverage cloud services

		* Loosely-coupled, canonical-based

	* VMWare vCenter Plug-in allows transparent migration of VMs to and from AWS

		* VMWare Cloud furthers this concept with more public cloud-native features

## Migration Tools

Storage Migration

	* AWS Storage Gateway

	* AWS Snowball

Server Migration Service

	* Automates migration of on-premises VMware vSphere or Microsoft Hyper-V/SCVMM virtual machines to AWS

	* Replicates VMs to AWS, syncing volumes and creating periodic AMIs

	* Minimizes cutover downtime by syncing VMs incrementally

	* Supports Windows and Linux VMs only (just like AWS)

	* The Server Migration Connector is downloaded as a virtual appliance into your on-prem vSphere or Hyper-V setup

Database Migration Service

	* Database Migration Service (DMS) along with the Schema Conversion Tool (SCT) helps customers migrate databases to AWS RDS or EC2-based databases

	* Schema Conversion Tool can copy database schemas for homogenous migration (same database) and convert schemas for heterogeneous migrations (different database)

	* DMS is used for smaller, simpler conversions and also supports MongoDB and DynamoDB

	* SCT is used for larger, more complex datasets like data warehouses

	* DMS has replication function for on-prem to AWS or to Snowball or S3

Application Discovery Service

	* Gathers information about on-premises data centers to help in cloud migration planning

	* Often customers don't know the full inventory or status of all their data center assets, so this tool helps with that inventory

	* Collects config, usage and behavior data from your servers to help in estimating TCO of running on AWS

	* Can run as agent-less (VMware environment) or agent-based (non-VMware environment)

	* Only supports those OSes that AWS supports (Linux and Windows)

View all these in AWS Migration Hub

## Network Migrations and Cutovers

### CIDR Reservations

Ensure your IP addresses will not overlap between VPC and on-prem

VPCs support IPv4 netmasks from /16 to /28

	* /16 = 255.255.0.0 = 65536 addresses

	* /28 = 255.255.255.240 = 16 addresses

Remember: 5 IPs are reserved in every VPC subnet that will take up addresses

	* /28 = 255.255.255.240

		* 16 addresses

		* 14 "usable" minus 3 reserved addresses in a VPC = 11 available in VPC

### Network Migrations

Most organizations start with a VPN connection to AWS

As usage grows, they might choose Direct Connect but keep the VPN as a backup

Transition from VPN to Direct Connect can be relatively seamless using BGP

Once Direct Connect is set-up, configure both VPN and Direct Connect within the same BGP prefix

From the AWS side, the Direct Connect path is always preferred...

... but you need to be sure the Direct Connect path is the preferred route from your network to AWS and not VPN (through BGP weighting or static routes)

### Snow Family

Evolution of AWS Import/Export process

Move massive amounts of data to and from AWS

Data transfer as fast or as slow as you're willing to pay a common carrier (UPS, DHL, FedEx, etc.)

Encrypted at rest

And also in transit!

AWS Import/Export - Ship an external hard drive to AWS. Someone at AWS plugs it in and copies your data to S3

AWS Snowball - Ruggedized NAS in a box AWS ships to you. You copy over up to 80TB of your data and ship it back to AWS. They copy the data over to S3

AWS Snowball Edge - Same as Snowball, but with onboard Lambda and clustering (portable AWS, capture and process data in a very remote location)

AWS Snowmobile - A literal shipping container full of storage (up to 100PB) and a truck to transport it

## Exam Tips

### Migration Strategies

Understand the different strategies that companies might undertake when deciding if the cloud is right for them

Understand the typical trade-offs and relative benefits for each strategy

### Cloud Adoption Framework

Know what a "framework" is and the realistic expectations that should accompany it

Understand the high-level components of the Cloud Adoption Framework

Most importantly, know that cloud adoption is only partially a technology effort

### Hybrid Architectures

Be able to speak to some typical hybrid architectures that leverage both on-prem and cloud assets

Know that VMware has some nifty tools for abstracting workloads across on-prem and clud, such as the Import plug-in

### Migrations

Understand the different services and tools available for migrating servers, storage and databases

Tool usage specifics won't likely be on the exam, except maybe Storage Gateway

### Network Migration and Cutovero

Know various hybrid networking architectures (See Networking chapter)

Understand that smooth transitions from and to VPN and Direct Connect can be done using BGP routing; abrupt route changes risk downtime

### Whitepapers

* https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf

* https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf

* https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf

* https://d1.awsstatic.com/whitepapers/cloud-driven-enterprise-transformation-on-aws.pdf?did=wp_card&trk=wp_card

## Pro Tips

Technology is often a minor part of a cloud migration project

Project management discipline is a must -- don't underestimate this

Adapt the Cloud Adoption Framework for your own organization's culture

Leverage the CAF to get buy-in by acknowledging the enterprise nature of cloud migrations

Be a Boundary Spanner!

# Architecting to Scale

## Concepts

### Loosely Coupled Architecture 

"Components can stand independently and require little or no knowledge of the inner workings of the other components"

Layers of abstraction

Permits more flexibility

Interchangeable components

**More atomic functional units**

**Can scale components independently**

| Horizontal Scaling | Vertical Scaling |
| --- | --- |
| Add more instances as demand increases | Add more CPU and/or RAM to existing instance as demand increases |
| No downtime required to scale up or down | Requires restart to scale up or down |
| Automatic using Auto-scaling Groups | Would require scripting to automate |
| (Theoretically) Unlimited | Limited by instance sizes |

Scale out - add an instance

Scale up - add resources to an existing instance

Scale in - remove an instance

Scale down - remove CPU and RAM

## Auto-Scaling

Types of Auto-Scaling:

| Name | What | Why |
| --- | --- | --- |
| Amazon EC2 Auto Scaling | Like the name says... focused on EC2 and nothing else | Setup scaling groups for EC2 instances; health checks to remove unhealthy instaces |
| Application Auto Scaling | API used to control scaling for resources other than EC2 like Dynamo, ECS, EMR | Provides a common way to interact with the scalability of other services |
| AWS Auto Scaling | Provides centralized way to manage scalability for whole stacks; Predictive scaling feature | Console that can manage both of the above from a unified standpoint |

### EC2 Auto-Scaling Groups

Automatically provides horizontal scaling (scale-out) for your landscape

Triggered by an event or scaling action to either launch or terminate instances

Availability, Cost and System Metrics can factor into scaling.

Four scaling options:
	
	* Maintain - Keep a specific or minimum number of instances running

	* Manual - Use maximum, minimum, or specific number of instances

	* Schedule - Increase or decrease instances based on schedule

	* Dynamic - Scale based on real-time metrics of the systems

Need to set up an auto scaling group with a launch configuration

Launch Configurations:

	* Specify VPC and subnets for scaled instances

	* Attach to a ELB

	* Define a Health Check Grace Period - time period that the scaling policy will allow the system to spin up and become healthy before it starts alerting on certain problems on that instance

	* Define size of group to stay at initial size

	* Or use scaling policy which can be based from metrics

Scaling Types:

| Scaling Type | What | When |
| --- | --- | --- |
| Maintain | Hands-off way to maintain X number of instances | "I need 3 instances always." |
| Manual | Manually change desired capacity via console or CLI | "My needs change so rarely that I can just manually add and remove." |
| Scheduled | Adjust min/max instances based on specific times | "Every monday morning, we get a rush on our website." |
| Dynamic | Scale in response to behavior of elements in the environment | "When CPU utilization gets to 70% on current instances, scale up." |

EC2 Auto Scaling Policies (Dynamic):

| Scaling | What | When |
| --- | --- | --- |
| Target Tracking Policy | Scale based on a predefined or custom metric in relation to a target value | "When CPU utilization gets to 70% on current instances, scale up." |
| Simple Scaling Policy | Waits until health check and cool down period before evaluating new need | "Let's add new instances slow and steady." |
| Step Scaling Policy | Responds to scaling needs with more sophistication and logic | "AGG! Add ALL the instances!" |

Scaling Cooldown Concept for EC2:

	* Cooldown period - Configurable duration that gives your scaling a chance to "come up to speed" and absorb load

	* Default cooldown period is 300 seconds

	* Automatically applies to **dynamic scaling** and optionally to manual scaling but **not supported for scheduled scaling**

	* Can override default cooldown via scaling specific cool down - might choose to do so in a scale-down policy

### AWS Application Auto Scaling

| Scaling | What | When |
| --- | --- | --- |
| Target Tracking Policy | Initiates scaling events to try to track as closely as possible a given target metric | "I want my ECS hosts to stay at or below 70% CPU utilization." |
| Step Scaling Policy | Based on a metric, adjusts capacity given certain defined thresholds | "I want to increase my EC2 Spot Fleet by 20% every time I add another 10,000 connections on my ELB." |
| Scheduled Scaling Policy | Initiates scaling events based on a predefined time, day or date | "Every Monday at 0800, I want to increase the Read Capacity Units of my DynamoDB Table to 20,000" |

### AWS Auto Scaling

Can manage the EC2 Auto Scaling and Application Auto Scaling through 1 common console.

AWS Predictive Scaling:

	* Predictive scaling can dynamically scale based on learning your load and calculating expected capacity

	* Without dynamic scaling, you can just use the data to adjust your own scaling policies

	* You can also opt-out of this is you don't want AWS collecting this data.

## Kinesis

Collection of services for processing streams of various data

Data is processed in "shards" -- with each shard able to ingest 1000 records per second

A default limit of 500 shards, but you can request an increase to unlimited shards

Record consists of Partition Key, Sequence Number and Data Blob (up to 1 MB)

Transient Data Store -- Default retention of 24 hours, but can be configured for up to 7 days

Kinesis Video Streams - process video data

Kinesis Data Streams:

	* Input - Capture and send data to Kinesis Data Streams

	* Kinesis data Streams - Ingests and stores data streams for processing

	* Processing tools - Build custom, real-time applications using Kinesis Data Analytics, stream processing frameworks like Apache Spark, or your code running on Amazon EC2 or AWS Lambda

	* Output - Analyze streaming data using your favorite BI tools

Kinesis Firehose:

	* Input - Capture and send data to Kinesis Data Firehose

	* Kinesis Data Firehose - Prepares and loads the data continuously to the destinations you choose

	* Data stores - durably store the data for analytics

	* Output - Analyze streaming data using analytics tools

Kinesis Data Analytics:

	* Input - Capture streaming data with Kinesis Data Firehose or Kinesis Data Streams

	* Kinesis Data Analytics - Run standard SQL queries against data streams

	* Output - Kinesis Data Analytics can send processed data to analytics tools so you can create alerts and respond in real-time

## DynamoDB Scaling

Throughput - Read and write capacity units

Size - Max item size is 400KB

DynamoDB Terminology:

| Term | What |
| --- | --- |
| Partition | A (10GB) physical space where DynamoDB data is stored. |
| Partition Key | A unique identifier for each record; sometimes called a **Hash Key** |
| Sort Key | In combination with a partition key, optional second part of a composite key that defines storage order; Sometimes called a **Range Key** |

DynamoDB Under the Hood:

	* Scales out by adding 10 GB partitions

| | Partition Calculations |
| --- | --- |
| By Capacity | (Total RCU / 3000) + (Total WCU / 1000) |
| By Size | Total Size / 10 GB |
| Total Partitions | Round Up for the MAX (By Capacity, by Size) |

10 GB Table, 2000 RCU and WCU:

	* By Capacity = (2000 RCU / 3000) + (2000 WCU / 1000) = **2.66**

	* By Size =  10 GB / 10 GB = **1**

	* Total Partitions = MAX(2.66, 1) = 2.66 Round Up = **3 Partitions**

Read and write capacity is equally allocated across partitions. 

Some burst capacity is allowed - can exceed capacity in a single partition if not using in other partitions, should design partition keys around trying to avoid whenever possible

Which partition determined by Partition Key hash value. Should use a partition key that has a degree of variability in it

If a lot of items have the same partition key, causes one partition to be overloaded with reads / writes (hot key/ partition issue)

Auto Scaling for DynamoDB:

	* Can set a target utilization and scale up when reached

	* Using Target Tracking method to try to stay close to target utilization

	* Currently does not scale down if table's consumption drops to zero

	* Workaround 1: Send requests to the table until it auto scales down

	* Workaround 2: Manually reduce the max capacity to be the same as the minimum capacity

	* Also supports Global secondary indexes -- think of them like a copy of the table

On-Demand Scaling for DynamoDB:

	* Alternative to Auto-Scaling

	* Useful if you can't deal with scaling lag or truly have no idea of the anticipated capacity requirements

	* Instantly allocates capacity as needed with no concept of provisioned capacity

	* Costs more than traditional provisioning and auto-scaling

DynamoDB Accelerator (DAX):

	* In-Memory cache that sits in front of a DynamoDB table and provides caching compatible with the DynamoDB API

	* Good uses of DAX:
	
		* Require fastest possible reads such as live auctions or securities trading

		* Read-intense scenarios where you want to offload the reads from DynamoDB

		* Repeated reads agains a large set of DynamoDB data
		
	* Bad Uses of DAX

		* Write-intensive applications that don't have many reads

		* Applications where you use client caching methods

## CloudFront Part 2

Can delever content to your users faster by caching **static** and **dynamic** content at edge locations

Dynamic content delivery is achieved using HTTP cookies forwarded from your origin

Supports Adobe Flash Media Server's RTMP protocol but you have to choose RTMP delivery method

Web distributions **also support media streaming and live streaming** but use HTTP or HTTPS

Origins can be S3, EC2, ELB or another web server

Multiple origins can be configured

Use Behaviors to configure serving up origin content based on URL paths

### Invalidation Requests

1. Simply delete the file from the origin and wait for the TTL to expire

2. Use the AWS Console to request invalidation for all content or a specific path such as /images/\* 

3. Use the CloudFront API to submit an invalidation request

4. Use third-party tools to perform CloudFront invalidation (CloudBerry, Ylastic, CDN Planet, CloudFront Purge Tool)

### Zone Apex Support

Your domain name with www or subdomain - forwards to cloudfront distribution

### Geo-Restrictions

Might want to do if you have regulatory restrictions / guidelines where you're not allowed to show or expose content to certain geographies or countries

## SNS

Enables a Publish/Subscribe design pattern

Topics = A channel for publishing a notification

Subscription = Configuring an endpoint to receive messages published on the topic

Endpoint protocols include HTTP(S), Email, SMS, SQS, Amazon Device Messaging (push notifications), and Lambda

Fan Out Architecture - Send events to multiple endpoints at once to perform different actions

## SQS

Reliable, highly-scalable, hosted message queuing service

Available integration with KMS for encrypted messaging

Transient storage -- default 4 days, max 14 days

Optionally supports First-In First-Out queue ordering

Maximum message size of 256KB but using a special Java SQS SDK, you can message as large as 2GB

Standard Queue - no assurances that messages will enter or leave the queue based on when they arrived

First-In First-Out (FIFO) Queue - Maintains the order in which the messages were received. Enforcing this means that if a message gets stuck, it will hold up all messages behind it, which can introduce latency or delay in clearing queues.

### Amazon MQ

Managed implementation of Apache ActiveMQ (message broker, takes in messages, manages them, and lets other applications access them via API)

Fully managed and highly available within a region

ActiveMQ API and support for JMS, NMS, MQTT, WebSocket

Designed as a drop-in replacement for on-premise message brokers

Use SQS if you are creating a new application from scratch

Use MQ if you want an easy low-hassle path to migrate from existing message brokers to AWS

## AWS Lambda, Serverless Application Manager, and EventBridge

### AWS Lambda

Allows you to run code on-demand without the need for infrastructure

Supports Node.js, Python, Java, Go, and C#

Extremely useful option for creating serverless architectures

Code is stateless and executed on an event basis (SNS, SQS, S3, DynamoDB Streams, etc.)

No fundamental limits to scaling a function since AWS dynamically allocates capacity in relation to events

Fan-out architecture - A lambda invokes multiple other lambda functions to perform multiple operations in parallel

### AWS Serverless Application Model

Open source framework for building serverless apps on AWS

Uses YAML as the configuration language

Includes AWS CLI-like functionality to create, deploy and update serverless apps usin AWS services such as Lambda, DynamoDB and API Gateway

Enables local testing and debugging of apps using a Lambda-like emulator via Docker

Extension of CloudFormation so you can use everything CloudFormation can provide by way of resources and functions

AWS provides Serverless Application Repository to catalog various premade serverless solutions

AWS SAM:

	* Uses YAML for templates

	* Purpose-built to help make developing and deploying serverless apps as efficient as possible

	* Generates CloudFormation scripts

Serverless Framework:

	* Uses YAML for templates

	* Purpose-built to help make developing and deploying serverless apps as efficient as possible

	* Generates CloudFormation scripts

	* **Supports many other cloud providers such as Azure, GCP, IBM, CloudFlare, etc.**

### Amazon EventBridge

Designed to link variety of AWS and 3rd party apps to rules logic for launching other event-based actions

## Simple Workflow Service (AWS SWF)

Status tracking system

Create distributed asynchronous systems as workflows

Supports both sequential and parallel processing

Tracks the state of your workflow which you interact with and update via API

Best suited for human-enabled workflows like order fulfillment or procedural requests

AWS recommends new applications -- look at Step Functions over SWF

Components:

	* Activity Worker - A program that interacts with the AWS SWF service to get tasks, process tasks, and return results

	* Decider - A program that controls coordination of tasks such as their concurrency and scheduling

Workers using SWF have to continuously "long poll" SWF to determine if there is any activity that they need to do

## Step Functions and Batch

### AWS Step Functions

Managed workflow and orchestration platform

Scalable and highly available

Define your app as a state machine

Create tasks, sequential steps, parallel steps, branching paths or timers

Amazon State Language declarative JSON

Apps can interact and update the stream via Step Function API

Visual interface describes flow and realtime status

Detailed logs of each step execution

### AWS Batch

Management tool for creating, managing and executing batch-oriented tasks using EC2 instances

1. Create a Compute Environment. Managed or Unmanaged, Spot or On-Demand, vCPUs

2. Create a Job Queue with priority and assigned to a Compute Environment

3. Create Job Definition: Script or JSON, environment variables, mount points, IAM role, container image, etc.

4. Schedule the Job

### Comparisons

| | When | Use Case |
| --- | --- | --- |
| Step Functions | Out-of-the-box coordination of AWS service components | Order Processing Flow |
| Simple Workflow Service | Need to support external processes or specialized execution logic | Loan Application Process with Manual Review Steps |
| Simple Queue Service | Messaging Queue; Store and forward patterns | Image Resize Process |
| AWS Batch | Scheduled or reoccurring tasks that do not require heavy logic | Rotate Logs Daily on Firewall Appliance |

## Elastic MapReduce (EMR) 

A collection of open-source projects

Hadoop HDFS - File system that the data is stored in, conducive to data analytics / manipulation

Hadoop MapReduce - Actual framework used to do the processing of the data

ZooKeeper - Resource Coordination - coordinates all the services to ensure they work together properly

Oozie - Workflow framework

Pig - Scripting Framework

Hive - SQL interface into hadoop landscape

Mahout - Machine Learning component

HBase - Columnar Database

Flume - Log Collection

Sqoop - Data Transfer - facilitates the import of data to hadoop from other databases / data sources

Ambari - Management and Monitoring console

### AWS Elastic MapReduce

Managed Hadoop framework for processing huge amounts of data

Also supports Apache Spark, HBase, Presto and Flink

Most commonly used for log analysis, financial analysis, or extract, transform and loading (ETL) activities

A Step is a programmatic task for performing some process on the data (i.e. count words)

A Cluster is a collection of EC2 instances provisioned by EMR to run your Steps

Components of AWS EMR:

	* Master Node

	* Core Nodes - have HDFS storage to persistently store data

	* Task Nodes - worker nodes, storage is ephemeral, there to work on the steps

## Exam Tips

### Auto Scaling Groups

Know the different scaling options and policies

Understand the difference and limitations between horizontal and vertical scaling

Know what a cool down period (vs a health check grace period) is and how it might impact your responsiveness to demand

### Kinesis

Exam is likely to be restricted to the Data Stream use cases for Kinesis such as Data Streams and Firehose

Understand shard concept and how partition keys and sequences enabled shards to manage data flow

### DynamoDB Auto Scaling

Know the new and old terminology and concept of a partition, partition key, and sort key in the context of DynamoDB

Understand how DynamoDB calculates total partitions and allocates RCU and WCU across available partitions

Conceptually know how data is stored across partitions

### CloudFront

Know that both static and dynamic content is supported

Understand possible origins and how multiple origins can be used together with behaviors

Know invalidation methods, zone apex and geo-restriction options

### SNS

Understand a loosely coupled architecture and the benefits it brings

Know the different types of subscription endpoints supported

### SQS

Know the difference between Standard and FIFO queues

Know difference between a Pub/Sub (SNS) and Message Queuing (SQS) architecture

### Lambda

Know what "Serverless" is in concept and how Lambda can facilitate such an architecture

Know the languages supported by lambda

### SWF

Understand the difference an functions of a Worker and a Decider

Best suited for human-enabled workflows like order fulfillment or procedural requests

### Elastic MapReduce

Understand the parts of a Hadoop landscape at a high level

Know what a Cluster is and what Steps are

Understand the roles of a Master Node, Core Nodes, and Task Nodes

### Step Functions

Managed workflow and orchestration platform considered preferred for modern development ove AWS Simple Workflow Service

Supports tasks, sequential steps, parallel steps, branching, timers

### AWS Batch

Ideal for use cases where a routine activity must be performed at a specific interval or time of day

Behind the scenes, EC2 instances are provisioned as workers to perform the batch activities then terminated when done

### Serverless Application Model

Open-source framework created to make serverless application development and deployment more efficient

SAM CLI translates specific SAM YAML into CloudFormation scripts which is then used to deploy on AWS

Serverless Application Repository contains lots of apps you can use as examples or elements of your own apps

AWS SAM and the "Serverless Framework" are different things

### Auto Scaling

Be familiar with the positioning and different purposes of EC2 Auto Scaling vs Application Auto Scaling vs AWS Auto Scaling

Know the different scaling options and policies

Understand the difference and limitations between horizontal and vertical scaling

Know what a cool down period is and how it might impact your responsiveness to demand

### Whitepapers

* https://d1.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf

* https://d0.awsstatic.com/whitepapers/aws-scalable-gaming-patterns.pdf

* https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

* https://d1.awsstatic.com/whitepapers/cost-optimization-automating-elasticity.pdf

* https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

* https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf

## Pro Tips

Elasticity will drive most benefit from the cloud

Think Cloud-First designs if you're in a Green Field scenario even if you're deploying on-prem

If you're in a "Brown Field" situation, create roadmaps for cloud-first enables like distributed applications, federated data and SOA

Be careful to not let elasticity cover for poor development methods

Microservice concepts help achieve scalability via decoupling, simplification and seperation of concerns

# Business Continuity

## Concepts

Business Continuity (BC) - seeks to minimize business activity disruption when something unexpected happens

Disaster Recovery (DR) - Act of responding to an event that threatens business continuity

High Availability (HA) - Designing in **redundancies** to reduce the chance of impacting service levels

Fault Tolerance - Designing in the ability to **absorb problems** without impacting service levels

Service Level Agreement (SLA) - An agreed **goal or target** for a given service on its performance or availability

Recovery Time Objective (RTO) - Time that it takes after a disruption to restore business processes to their service levels (**T** is for Time)

Recovery Point Objective (RPO) - Acceptable amount of data loss measured in time (**P** is for data that goes "poof")

Business Continuity Plan - defines RPO, RTO

RTO, RPO - supports Business Continuity Plan, justifies High Availability Investment, defines Disaster Recovery

High Availability Investment - migigates Disaster Recovery

Disaster Recovery - delivers RTO, RPO

### Disaster Categories

| Category | Example |
| --- | --- |
| Hardware Failure | Network switch power supply fails and brings down LAN |
| Deployment Failure | Deploying a patch that breaks a key ERP business process |
| Load Induced | Distributed Denial of Service attack on your website |
| Data Induced | Ariane 5 rocket explosion on June 4, 1996 |
| Credential Expiration | An SSL/TLS certificate expires on your eCommerce site |
| Dependency | S3 subsystem failure cause numerous other AWS service failures |
| Infrastructure | A construction crew accidentally cuts a fiber optic data line |
| Identifier Exhaustion | "We currently do not have sufficient capacity in the AZ you requested" |

Human Error:

	* "I thought I was in the QA system!"

	* sudo rm -rf /

	* truncate table 'invoices';

## AWS Continuum of HA

### Backup and Restore

Pros:

	* Very common entry point into AWS

	* Minimal effort to configure

Cons:

	* Least flexibility

	* Analogous to off-site backup storage

### Pilot Light

Pros:

	* Cost effective way to maintain a "hot site" concept

	* Suitable for a variety of landscapes and applications

Cons:

	* Usually requires manual intervention for failover

	* Spinning up cloud environments will take minutes or hours

	* Must keep AMIs up-to-date with on-prem counterparts

### Warm Standby

Pros:

	* All services are up and ready to accept a failover faster within minutes or seconds

	* Can be used as a "shadow environment" for testing or production staging

Cons:

	* Resources would need to be scaled to accept production load

	* Still requires some environment adjustments but could be scripted

### Multi-Site

Pros:

	* Ready all the time to take full production load -- effectively a mirrored data center

	* Fails over in seconds or less

	* No or little intervention required to fail over

Cons:

	* Most expensive DR option

	* Can be perceived as wasteful as you have resources just standing around waiting for the primary to fail

## Storage HA Options

### EBS Volumes

Annual Failure Rate of less than 0.2% compared to commodity hard drive at 4% (Given 1000 EBS volumes, expect around 2 to fail per year.)

Availability target or 99.999%

Replicated automatically **within a single AZ**

**Vulnerable to AZ failure. Plan accordingly**

Easy to snapshot, which is stored on S3 and multi-AZ durable

You can copy snapshots to other regions as well

Supports RAID configurations

#### RAID Configurations

| | RAID0 | RAID1 | RAID5 | RAID6 |
| --- | --- | --- | --- | --- |
| Redundancy | None | 1 drive can fail | 1 drive can fail | 2 drives can fail |
| Reads | \*\*\*\* | \*\*\* | \*\*\*\* | \*\*\*\* |
| Writes | \*\*\*\* | \*\*\* | \*\* | \* |
| Capacity | 100 % |  50% | (n-1)/n | (n-2)/2 |

AWS only recommends RAID0, RAID1

#### RAID IOPS and Throughput

| | Volume Size | Provisioned IOPS | Total Volume IOPS | Usable Space | Throughput |
| No RAID | (1) 1000 GB | 4000 | 1000 GB | 500 MB/s |
| RAID0 | (2) 500 GB | 4000 | **8000** | **1000 GB** | **1000 MB/s** |
| RAID1 | (2) 500 GB | 4000 | 4000 | 500 GB | 500 MB/s |

### S3 Storage

Standard Storage Class (99.99% availability = 52.6 minutes / year)

Standard Infrequent Access (99.9% Availability = 8.76 hours per year)

One-zone Infrequent Access (99.5% availability = 1.83 days / year)

Eleven 9s of durability (99.999999999%)

Standard & Standard-IA have multi-AZ durability; One-zone only has single AZ durability

Backing service for EBS snapshots and many other AWS services

### Amazon EFS

Implementation of NFS file system

True file system as opposed to block storage (EBS) or object storage (S3)

File locking, strong consistency, concurrently accessible

Each file object and metadata is stored across multiple AZs

Can be accessed from all AZs concurrently

Mount targets are highly available

### Other Options

Amazon Storage Gateway:

	* Good way to migrate on-prem data to AWS for offsite backup

	* Best for continuous sync needs

Snowball:

	* Various options for migrating data to AWS based on volume

	* Only for batch transfers of data

Glacier:

	* Safe offsite archive storage

	* Long-term storage with rare retrieval needs

## Compute HA Options

Up-to-Date AMIs are critical for rapid fail-over

AMIs can be copied to other regions for safety or DR staging

Horizontally scalable architectures are preferred because risk can be spread across multiple smaller machines vs one large machine

Reserved Instances is the only way to **guarantee** that resources will be available when needed

Auto Scaling and Elastic Load Balancing work together to provide automated recovery by maintaining minimum instances

Route 53 Health Checks also provide "self-healing" redirection of traffic

## Database HA Options

If possible, **choose DynamoDB over RDS** because of inherent fault tolerance

If DynamoDB can't be used, chooes Aurora because of redundancy and automatic recovery features

If Aurora can't be used, choose Multi-AZ RDS

Frequent RDS snapshots can protect against data corruption or failure -- and they won't impact performance of multi-AZ deployment

Regional replication is also an option, but will not be strongly consistent

If Database on EC2, you'll have to design HA yourself

### HA Notes for Redshift

Currently, Redshift does not support multi-AZ deployments

Best HA option is to use a multi-node cluster which supports data replication and node recovery

A single node Redshift cluster does not support data replication, and you'll have to restore from a snapshot on S3 if a drive fails

### HA Notes for ElastiCache

Memcached:

	* Because Memcached does not support replication, a node failure will result in data loss

	* Use multiple nodes in each shard to minimize data loss on node failure

	* Launch multiple nodes across available AZs to minimize data loss on AZ failure

Redis:

	* Use multiple nodes in each shard and distribute the nodes across multiple AZs

	* Enable multi-AZ on the replication group to permit automatic failover if the primary node fails

	* Schedule regular backups of your Redis cluster

## Network HA Options

By creating subnets in the available AZs, you create multi-AZ presence for your VPC

Best practice is to create at least two VPN tunnels into your Virtual Private Gateway

Direct Connect is not HA by default, so you need to establish a secondary connection via another Direct Connect (ideally with another provider) or use a VPN

Route 53's Health Checks provide basic levels of redirecting DNS resolutions

Elastic IPs allow you flexibility to change out backing assets without impacting name resolution

For multi-AZ redundancy of NAT Gateways, create gateways in each AZ with routes for private subnets to use the local Gateway

## Exam Tips

### General Concepts

Know the difference between Business Continuity, Disaster Recovery, and Service Levels

Know the difference between High Availability and Fault Tolerance

Understand the inter-relationships between High Availability and Fault Tolerance

Understand the inter-relationships and how AWS uses the terms

Know the difference between RTO and RPO

Know the four general types of DR architectures and trade-offs of each

### Storage Options

Understand the HA capabilities and limitations of AWS storage options

Know when to use each storage option to achieve the required level of recovery capability

Understand RAID and the potential benefits and limitations

### Compute Options

Understand why horizontal scaling is preferred from an HA perspective

Know that compute resources are finite in an AZ and know how to guarantee their availability

Understand how Auto Scaling and ELB can contribute to HA

### Database Options

Know the HA attributes of the various Database services

Understand the different HA approaches and risks for Memcached and Redis

Know which RDS options require manual failover and which are automatic

### Network Options

Know which networking components are not redundant across AZs and how to architect for them to be redundant

Understand the capabilities of Route 53 and Elastic IP in the context of HA

### Whitepapers

* https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/backup-recovery.pdf

* https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf

* https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/wellarchitected-reliability-pillar.pdf

## Pro Tips

Failure Mode and Effects Analysis (FMEA):

	* A systematic process to examine:

		1. What could go wrong

		2. What impact it might have

		3. What is the likelihood of it occurring

		4. What is our ability to detect and react

Severity * Probability * Detection = Risk Priority Number

Step 1 - Round up Possible Failures

Step 2 - Assign Scores

Step 3 - Prioritize on Risk

# Deployment and Operations Management

## Types of Deployment

"Big Bang" - everything goes live at the same time

Phased Rollout - more gradual over time

Parallel Adoption - Implement the new while still using the old

### Phased Rollout Types

Rolling Deployment:

	1. Create new Launch Configuration with the updated AMI

	2. Start terminating old EC2 instances

A/B Testing:

	- test new solution with small amount of people and compare results with original solution

Canary Release:

	- Deploy for small amount, confirm no errors / disasters, then deploy for rest

Blue-Green Deployment:

	- Switch over to the new version (Green), if we have issues, failover to the original (Blue)

	- The goal of blue/green deployments is to achieve immutable infrastructure, where you don't have to make changes to your application after it's deployed, but redeploy altogether

Blue Green Methods:

	* Update DNS with Route 53 to point to a new ELB or instance

	* Swap Auto-scaling Group already primed with new version instances behind the ELB

	* Change Auto-Scaling Group Launch Configuration to use new AMI version and terminate old instances

	* Swap environment URL of Elastic Beanstalk

	* Clone stack in AWS OpsWorks and update DNS

Blue Green Contraindication:

	* Data store schema is too tightly coupled to the code changes

	* The upgrade requires special upgrade routines to run during deployment

	* Off-the-shelf products might not be blue-green friendly

## Continuous Integration and Deployment

Continuous Integration - Merge code changes back to main branch as frequently as possible with automated testing as you go.

Continuous Delivery - You have automated your release process to the point you can deploy at the click of a button

Continuous Deployment - Each code change that passes all stages of the release process is released to production with no human intervention required

### CI/CD Considerations

Objective is to create smaller, incremental compartmentalized improvements and features

Lowers deployment risk and tries to limit negative impact

Test Automation game must be STRONG

Feature toggle patterns useful for dealing with in-progress features not ready for release (versus more traditional branching strategies)

Microservice architectures lend themselves well to CI/CD practices

### AWS Deployment Lifecycle Tools

AWS CodeCommit - hosted git repository

AWS CodeBuild - helps us compile code, run tests, create deployment packages

AWS CodeDeploy - deploy deployment packages on EC2, Lambda, Elastic Beanstalk, ECS, even on-prem

AWS CodePipeline - Orchestration mechanism that helps do all these things together

AWS X-Ray - Helps with debugging in a distributed / serverless architecture

AWS CodeStar - Leverages other services and creates templates that are more turnkey

## Elastic Beanstalk

Orchestration service to make it push-button easy to deploy scalable web landscapes

Wide range of supported platforms -- from Docker to PHP to Java to Node.js

Multiple Environments within Application (DEV, QA, PRD, etc.)

Great for ease of deployment, but not great if you need lots of control or flexibility

Layers:

	* Application Layer (Management Layer) - can have multiple environments

	* Environments - Instances, Web server, Engines, Load Balancers, Monitoring, Scaling

	* Application Versions

### Elastic Beanstalk Deployment Options

| Deployment Option | What | Deployment Time | Downtime? | Rollback Process |
| --- | --- | --- | --- | --- |
| All At Once | New application version is deployedto existing instances all at once, potentially resulting in downtime | \* | Yes | Manual |
| Rolling | One by one, new application version is deployed to existing instances in batches | \*\* | No | Manual |
| Rolling with Additional Batch | Launch new version instances prior to taking any old version instances out of service | \*\*\* | No | Manual |
| Immutable | Launch a full set of new version instances in seperate auto-scaling group and only cuts over when health check is passed | \*\*\*\* | No | Terminate New Instances |
| Traffic Splitting | Percent of client traffic is routed to new instances for purposes of "canary testing" | \*\*\*\* | No | Reroute DNS and Term New Instances |
| Blue/Green | CNAME DNS entry changed when new version is fully up, leaving old version in place until new is fully verified | \*\*\*\* | No | Swap URL |

## CloudFormation

Infrastructure as Code!

Using JSON or YAML, you can model and provision entire landscapes

Repeatable, automatic deployments and rollbacks

Nest common components for reusability

Supports over 300 Resource Types (components of AWS Services)

Want more? Supports custom resources via SNS or Lambda

### CloudFormation Concepts

Templates - The JSON or YAML text file that contains the instructions for building out the AWS environment

Stacks - The entire environment described by the template and created, updated, and deleted as a single unit

Change Sets - A summary of proposed changes to your stack that will allow you to see how those changes might impact your existing resources before implementing them

Stack Policies:

	* Protect specific resources within your stack from being unintentionally deleted or updated

	* Add a Stack Policy via the console or CLI when creating a stack

	* Adding a Stack Policy to an existing stack can only be done via CLI

	* Once applied, a Stack Policy cannot be removed - but it can be modified via CLI

	* By default, they protect everything, prohibiting changes to anything

### Cloudformation Best Practices

AWS provides Python "helper scripts" which can help you install software and start services on your EC2 instances

Use CloudFormation to make changes to your landscape rather than going directly into the resources

Make use of Change Sets to identify potential trouble spots in your updates

Use Stack Policies to explicitly protect sensitive portions of your stack

Use a version control system such as CodeCommit or GitHub to track changes to templates
