# 系统设计中的持久性

**持久性**意味着一旦数据成功提交到系统，即使系统发生故障，数据也不会丢失。例如，当我们上传一张照片到 Instagram，并收到上传成功的确认后，我们可以确信这张照片不会从系统中消失。

## 实现持久性

系统通过创建和维护数据的多个副本来实现持久性。有几种方法可以做到这一点：

1. **备份**
2. **RAID（独立磁盘冗余阵列）**
3. **复制**

### 备份

第一种方法是定期从非易失性存储（如磁盘）复制数据。这样的副本称为**备份**。创建备份有三种常见策略：

#### 完全备份

- 始终创建数据的完整副本。
- 如果主数据存储失败，我们从最近的完整备份中恢复数据。
- **优点**：数据恢复速度快。
- **缺点**：创建备份耗时，尤其是数据量很大时。

#### 差异备份

- 保存自上次完整备份以来的数据差异。
- 例如，每周进行一次完整备份，而在其他天备份自上次完整备份以来的更改。
- **优点**：备份文件较小，创建速度比完全备份快。
- **缺点**：恢复数据需要最后一次完整备份和最后一次差异备份。

#### 增量备份

- 仅保存自前一次备份以来更改的数据。
- 例如，在周日进行完整备份，周一的备份包含自周日以来的更改，周二的备份包含自周一以来的更改，依此类推。
- **优点**：备份文件更小，创建速度更快。
- **缺点**：恢复过程更复杂，耗时更长，需要最后一次完整备份和所有后续的增量备份。

### 备份的局限性

- **延迟**：无法在数据更改后立即创建备份，总会有延迟。
- **恢复时间**：恢复数据需要时间，在此期间用户可能无法访问数据。

### RAID（独立磁盘冗余阵列）

为了解决备份的局限性，我们可以使用多个磁盘，在写入数据时将其复制到多个磁盘上。

- **RAID** 将多个物理磁盘组合成一个逻辑单元。
- 对于应用程序而言，它看起来像一个单一的虚拟设备。
- 数据在内部被复制到所有磁盘。
- **优点**：
  - 如果一个磁盘故障，数据仍然可从其他磁盘获取。
  - 由于并行性，提高了读取吞吐量。

#### RAID 级别

- **RAID 1（镜像卷）**：
  - 数据在磁盘间镜像复制。
  - 提供冗余性，增加持久性。
- **RAID 0（条带化）**：
  - 数据在磁盘间交替写入，无冗余。
  - **不**增加持久性，实际上降低了持久性。
  - **优点**：由于并发读写，性能提高。
  - **缺点**：任何磁盘故障都会导致数据丢失。

### 复制

与其在磁盘级别复制数据，不如在应用程序级别复制。

- **复制**涉及多台机器（可能地理分散），每台都有自己的存储。
- 当数据写入一台机器时，会快速复制到其他机器。
- **优点**：
  - 增加持久性。
  - 提高可用性和读取吞吐量。

## 组合使用备份、RAID 和复制

这些机制并不是互斥的。在高度持久的系统中，您可能会发现其中的两种或全部三种机制。

- **备份**是实现高持久性的必要条件。
- **复制**在当今也被广泛使用，特别是在云存储中。
- **RAID** 的必要性可能不那么明显，但在实践中仍被使用。例如，Cassandra 数据库的文档指出，尽管复制能够充分防止数据丢失，如果需要额外的冗余性，可以对提交日志磁盘使用 RAID 1。

## 增加持久性的其他机制

- **版本控制**：保留特定对象的多个版本。
- **防止意外删除的保护措施**：防止数据被意外删除。

## 确保数据完整性

高持久性还需要确保数据副本未被损坏。

- **校验和**：
  - 在数据写入时计算并存储校验和。
  - 在读取时，通过比较存储的校验和和计算的校验和来验证数据完整性。
- **数据修复**：
  - 如果检测到损坏，通过从健康的副本中重新创建数据来修复。
- **示例**：
  - Hadoop 分布式文件系统（HDFS）定期扫描数据，识别并删除损坏的数据。

## 持久性与可用性

持久性常常与可用性混淆，但它们是不同的概念。

- **可用性**：您现在能否访问您的数据。
- **持久性**：您的数据将来是否仍然存在。

## 实现高持久性

尽管由于用户错误、硬件故障、自然灾害等因素，无法保证 100% 的持久性，但我们可以追求非常高的持久性。

- **示例**：
  - AWS S3 承诺在一年内提供 **11 个 9**（99.999999999%）的持久性。
  - 这意味着，如果我们在 Amazon S3 上存储 1000 万个对象，平均每 1 万年可能会丢失一个对象。
