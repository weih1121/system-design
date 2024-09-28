# Durability in System Design

**Durability** means that once data is successfully submitted to the system, it is not lost—even if faults occur in the system. For example, when we upload a photo to Instagram and receive confirmation of a successful upload, we can be confident that the photo will not disappear from the system.

## Achieving Durability

Systems achieve durability by creating and maintaining multiple copies of data. There are several ways to do this:

1. **Backup**
2. **RAID (Redundant Array of Independent Disks)**
3. **Replication**

### Backup

The first option is to periodically copy data from non-volatile storage (e.g., disks). Such a copy is called a **backup**. There are three common strategies to create backups:

#### Full Backup

- Always create a complete copy of the data.
- In case of primary data storage failure, we restore data from the most recent full backup.
- **Advantages**: Quick data restoration.
- **Disadvantages**: Time-consuming to create, especially with large data volumes.

#### Differential Backup

- Save the difference in data since the last full backup.
- For example, perform a full backup once a week, and on other days, back up only the changes since the last full backup.
- **Advantages**: Smaller backup size, faster to create than full backups.
- **Disadvantages**: To restore data, need the last full backup and the last differential backup.

#### Incremental Backup

- Save only the data that has changed since the preceding backup.
- For example, after a full backup on Sunday, Monday's backup contains changes since Sunday, Tuesday's backup contains changes since Monday, etc.
- **Advantages**: Even smaller backup size, faster to create.
- **Disadvantages**: Restoration is more complex and time-consuming, as it requires the last full backup and all subsequent incremental backups.

### Limitations of Backup

- **Latency**: Cannot create backups immediately after data changes; there's always a delay.
- **Restoration Time**: Restoring data takes time, during which data may not be available to users.

### RAID (Redundant Array of Independent Disks)

To overcome backup limitations, we can use multiple disks and copy data between them as it is written.

- **RAID** combines multiple physical disks into one logical unit.
- From the application's perspective, it appears as a single virtual device.
- Data is copied to all disks internally.
- **Advantages**:
  - If one disk fails, data remains available on other disks.
  - Increased read throughput due to parallelism.

#### RAID Levels

- **RAID 1 (Mirrored Volume)**:
  - Data is mirrored across disks.
  - Provides redundancy and increases durability.
- **RAID 0 (Striping)**:
  - Data is striped across disks without redundancy.
  - **Does not** increase durability; in fact, it decreases it.
  - **Advantages**: Improved performance due to concurrent reads/writes.
  - **Disadvantages**: Any disk failure results in data loss.

### Replication

Instead of copying data at the disk level, we can copy it at the application level.

- **Replication** involves multiple machines (possibly geographically distributed), each with its own storage.
- When data is written to one machine, it is quickly copied to others.
- **Advantages**:
  - Increases durability.
  - Improves availability and read throughput.

## Combining Backup, RAID, and Replication

These mechanisms are not mutually exclusive. In highly durable systems, you may find two or all three implemented.

- **Backup** is essential for high durability.
- **Replication** is widely used, especially with cloud storage.
- **RAID** may be used for additional redundancy, though its necessity may vary.

## Other Mechanisms to Increase Durability

- **Versioning**: Keeping multiple versions of specific objects.
- **Safeguards against Accidental Deletion**: Protecting data from being accidentally deleted.

## Ensuring Data Integrity

High durability also requires ensuring that data copies are not corrupted.

- **Checksums**:
  - Calculate and store a checksum when data is written.
  - Verify data integrity by comparing stored and calculated checksums upon reading.
- **Data Repair**:
  - If corruption is detected, repair by recreating the data from healthy copies.
- **Example**:
  - Hadoop Distributed File System (HDFS) regularly scans data to identify and remove corrupted data.

## Durability vs. Availability

Durability is often confused with availability, but they are different concepts.

- **Availability**: Whether you can access your data right now.
- **Durability**: Whether your data will still be there in the future.

## Achieving High Durability

While 100% durability cannot be guaranteed due to factors like user error, hardware failures, natural disasters, etc., we can aim for very high durability.

- **Example**:
  - AWS S3 promises **11 nines** (99.999999999%) durability over a given year.
  - This means losing a single object once every 10,000 years if storing 10 million objects.
