# Google cloud notes

* best for Big data services

## Major Concepts

### What is a project? <a href="#what_is_a_project" id="what_is_a_project"></a>

* A project organises all your Google Cloud resources.&#x20;
* A project consists of a set of users; a set of APIs; and billing, authentication, and monitoring settings for those APIs. So, for example, all of your Cloud Storage buckets and objects, along with user permissions for accessing them, reside in a project.&#x20;
* You can have one project, or you can create multiple projects and use them to organize your Google Cloud resources, including your Cloud Storage data, into logical groups.

### Service accounts <a href="#service-accounts" id="service-accounts"></a>

[Service accounts](https://cloud.google.com/iam/docs/service-accounts) allow applications to authenticate and access Google Cloud resources and services. For example, you can create a service account that your Compute Engine instances use to access objects stored in Cloud Storage buckets. Service accounts are created within a project and have a unique email address that identifies them.

The following are examples of actions related to Cloud Storage that are often taken by service accounts that [you create and manage](https://cloud.google.com/iam/docs/creating-managing-service-accounts):

* Performing [Storage Transfer Service](https://cloud.google.com/storage-transfer/docs/overview) transfers.
* [Moving data to/from Cloud SQL instances](https://cloud.google.com/sql/docs/mysql/import-export).
* Creating [signed URLs](https://cloud.google.com/storage/docs/access-control/signed-urls).

#### Service agents <a href="#service-agents" id="service-agents"></a>

A [service agent](https://cloud.google.com/iam/docs/service-agents) is a special type of service account that acts on behalf of a Google Cloud service. Cloud Storage uses a service agent for the following features:

* [Pub/Sub Notifications for Cloud Storage](https://cloud.google.com/storage/docs/pubsub-notifications).
* [Customer-Managed Encryption Keys](https://cloud.google.com/storage/docs/encryption/customer-managed-keys).

The service agent for Cloud Storage has an email address with the following format:

```
service-PROJECT_NUMBER@gs-project-accounts.iam.gserviceaccount.com
```

Where `PROJECT_NUMBER` is the [project number](https://cloud.google.com/resource-manager/docs/creating-managing-projects#identifying\_projects) of the project that owns the service account.

## Core services

* compute engine
* storage engine
* network engine

### Storage:

#### Google cloud storage

* world-wide storage and retrieval of any amount of data at any time

> Comparable service in AWS: S3

**command line util: gsutil**

to create bucket in cloud storage

```
gsutil mb -b on -l us-east1 gs://my-awesome-bucket/
```

to copy files to cloud storage

```
gsutil cp Desktop/kitten.png gs://my-awesome-bucket
```

to copy files from cloud storage

```
gsutil cp gs://my-awesome-bucket/kitten.png Desktop/kitten2.png
```

list files

```
gsutil ls gs://my-awesome-bucket
```

remove file from cloud storage

```
gsutil rm gs://my-awesome-bucket/kitten.png
```

\
Python code sample : (similar to boto3 of aws):

```
from google.cloud import storage


def create_bucket(bucket_name):
    """Creates a new bucket."""
    bucket_name = "your-new-bucket-name"
    storage_client = storage.Client()
    bucket = storage_client.create_bucket(bucket_name)
    print("Bucket {} created".format(bucket.name))
```

**Buckets**

* Buckets are the basic containers that hold your data.
* Everything that you store in Cloud Storage must be contained in a bucket.

**Objects**

* Objects are the individual pieces of data that you store in Cloud Storage.&#x20;
* There is no limit on the number of objects that you can create in a bucket.

**No folders**

* Cloud Storage operates with a flat namespace, which means that folders don't actually exist within Cloud Storage.
* If you create an object named `folder1/file.txt` in the bucket `your-bucket`, the path to the object is `your-bucket/folder1/file.txt`, but there is no folder named `folder1`; instead, the string `folder1` is part of the object's name.
* _`However, the Cloud Console and gsutil provide the illusion of a hierarchical file tree`_
* > If you create an empty folder using the Cloud Console, Cloud Storage creates a zero-byte object as a placeholder.

#### **Strongly consistent operations** <a href="#strongly_consistent_operations" id="strongly_consistent_operations"></a>

Cloud Storage provides strong global consistency for the following operations, including both data and metadata:

* Read-after-write
* Read-after-metadata-update
* Read-after-delete
* Bucket listing
* Object listing

#### **Eventually consistent operations** <a href="#eventually_consistent_operations" id="eventually_consistent_operations"></a>

The following operations are eventually consistent:

* Granting access to or revoking access from resources.

### Available storage classes <a href="#available_storage_classes" id="available_storage_classes"></a>

The following table summarizes the primary storage classes offered by Cloud Storage. See [class descriptions](https://cloud.google.com/storage/docs/storage-classes#descriptions) for a complete discussion.

| Storage Class    | Name for APIs and gsutil | [Minimum storage duration](https://cloud.google.com/storage/pricing#archival-pricing) | Typical monthly availability1                                                         |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Standard Storage | `STANDARD`               | None                                                                                  | <ul><li>>99.99% in multi-regions and dual-regions</li><li>99.99% in regions</li></ul> |
| Nearline Storage | `NEARLINE`               | 30 days                                                                               | <ul><li>99.95% in multi-regions and dual-regions</li><li>99.9% in regions</li></ul>   |
| Coldline Storage | `COLDLINE`               | 90 days                                                                               | <ul><li>99.95% in multi-regions and dual-regions</li><li>99.9% in regions</li></ul>   |
| Archive Storage  | `ARCHIVE`                | 365 days                                                                              | <ul><li>99.95% in multi-regions and dual-regions</li><li>99.9% in regions</li></ul>   |

**Encryption:**

Cloud Storage always encrypts your data on the server side, before it is written to disk, at no additional charge. Besides this [standard, Google-managed behavior](https://cloud.google.com/storage/docs/encryption/default-keys), there are additional ways to encrypt your data when using Cloud Storage. Below is a summary of the encryption options available to you:

* _Server-side encryption_: encryption that occurs after Cloud Storage receives your data, but before the data is written to disk and stored.
  * [_Customer-managed encryption keys_](https://cloud.google.com/storage/docs/encryption/customer-managed-keys): You can create and manage your encryption keys through [Cloud Key Management Service](https://cloud.google.com/security-key-management). Customer-managed encryption keys can be stored as software keys, in an [HSM cluster](https://cloud.google.com/kms/docs/hsm), or [externally](https://cloud.google.com/kms/docs/ekm).
  * [_Customer-supplied encryption keys_](https://cloud.google.com/storage/docs/encryption/customer-supplied-keys): You can create and manage your own encryption keys. These keys act as an additional encryption layer on top of the standard Cloud Storage encryption.
* [_Client-side encryption_](https://cloud.google.com/storage/docs/encryption/client-side-keys): encryption that occurs before data is sent to Cloud Storage. Such data arrives at Cloud Storage already encrypted but also undergoes server-side encryption.

**Similarities and differences with AWS:**

* object versioning,lifeclycle management features are same.
* object hold (object cannot be deleted)
* Static website hosting is generally done using HTTPS loadbalancer. Instead in AWS cloudfront is preferred approach.
* Theres a concept of  Composite objects. Not sure if such thing exists in S3&#x20;

```
create from existing objects without transferring additional object data. 
Composite objects are useful for making appends to an existing object, 
as well as for recreating objects that you uploaded as multiple components in parallel.
```

## General gotchas for people coming from AWS

* `Labels` in GCP are like tags from AWS. These are used to label all the resources and pair them later for granting permissions or stuff
*
