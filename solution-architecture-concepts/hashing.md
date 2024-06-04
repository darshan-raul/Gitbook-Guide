# Hashing

The Git commit hash is a 40-character hexadecimal string that uniquely identifies a specific commit in a Git repository. It's created by hashing the following information:

1. **Tree object hash:** Represents the state of the repository at the time of the commit (the files and their contents).
2. **Parent commit hash(es):** If the commit is not the first in the repository, it includes the hash of its parent commit(s) to establish the commit history.
3. **Author information:** The name and email of the person who made the commit, as well as the timestamp of when the commit was authored.
4. **Committer information:** Similar to author information, but for the person who finalized the commit (sometimes different from the author).
5. **Commit message:** The message written to describe the changes made in the commit.

The hashing algorithm used in Git is currently SHA-1. The data mentioned above is combined into a single string and then hashed using SHA-1 to produce the commit hash.

Due to the nature of SHA-1, even a minor change in the commit's content (e.g., a single character in the commit message) will result in a completely different hash. This ensures the integrity of the commit history and allows for efficient comparison between different versions of the repository.

It's important to note that Git hashes are designed to be unique within a single repository. However, there's an extremely small chance of collision (two different commits having the same hash) across different repositories.

***

Docker uses several types of hashes for images and containers, each with its own purpose and construction:

**Image Hashes:**

* **Image ID (also known as a Short ID):** This is a 12-character hexadecimal string that is a truncated version of the full image digest. It is not globally unique, but unique enough within a single Docker host.
* **Image Digest (Content Addressable Hash):** This is a longer hash, usually prefixed with `sha256:`, representing the SHA-256 hash of the image's manifest (which includes information about layers, architecture, and configuration). This hash is globally unique and ensures the integrity of the image. It is used to pull specific images from registries.

Docker calculates the image digest by hashing the following information:

1. **Manifest Configuration:** Includes information about the image's architecture, OS, and other settings.
2. **Layer Digests:** Each layer in the image is a compressed filesystem change. Docker calculates a SHA-256 hash for each layer's content.
3. **Layer Order:** The order of layers is important for rebuilding the image, so it's included in the digest calculation.

**Container Hashes:**

* **Container ID (also known as a Short ID):** Similar to the Image ID, it is a 12-character hexadecimal string that is unique to a container on a single Docker host. It is a truncated version of the full container ID.
* **Full Container ID:** This is a longer, unique hash that identifies a specific container instance globally. It is typically not displayed directly but can be retrieved using `docker inspect`.

The container ID is not a hash of specific content like the image digest. Instead, it is generated randomly when the container is created to provide a unique identifier.

**Key Points:**

* **Uniqueness:** Image digests are designed to be globally unique, while image IDs and container IDs are only unique on a single host.
* **Integrity:** Image digests ensure the integrity of the image content and can be used to verify that an image has not been tampered with.
* **Mutability:** Container IDs are mutable and will change if a container is recreated, while image digests remain constant as long as the image content doesn't change.

You can view image digests using `docker images --digests` and retrieve container IDs using `docker ps` or `docker inspect`.



***

Hashes, regardless of the data's size, are designed to be fixed-length outputs. This is a fundamental property of cryptographic hash functions. Here's why a hash can be small even for large files:

**1. Hash Functions are Mathematical Algorithms:**

Hash functions are complex mathematical operations. They take an input of any size and produce a fixed-size output through a series of transformations and calculations. The original data isn't stored directly within the hash.

**2. Pigeonhole Principle:**

The pigeonhole principle states that if you have more items than containers, at least one container must have more than one item. Since there are a limited number of possible hash values (e.g., 256 possibilities for a SHA-256 hash), and an infinite number of possible input files, there must be many files that share the same hash. This is known as a collision.

**3. The Avalanche Effect:**

Good cryptographic hash functions exhibit the avalanche effect, which means even a tiny change in the input data will result in a drastically different hash. This ensures that even two very similar files will have distinct hashes.

**4. Security vs. Compression:**

It's important to distinguish between hashing and compression. Compression aims to reduce file size while retaining the original data. Hashing, on the other hand, aims to create a unique, fixed-size representation of the data, not to reduce its size. Recovering the original data from a hash is not possible.

**Example:**

Consider a SHA-256 hash, which produces a 256-bit (32-byte) output. There are far more possible 1GB files (2^(8 \* 1024 \* 1024 \* 1024)) than there are possible SHA-256 hashes (2^256). This means multiple 1GB files will inevitably share the same hash, but the probability of finding such a collision is incredibly low for a well-designed hash function.

**Use Case of Hashing:**

The small size of hashes makes them ideal for:

* **Data Integrity:** Verifying that a file hasn't been altered by comparing its hash to a previously recorded one.
* **Quick Comparisons:** Quickly checking if two files are identical by comparing their hashes instead of the entire file contents.
* **Security:** Storing passwords as hashes instead of plaintext, making it harder for attackers to recover the original passwords.

