#### 1. Performance at Different Concurrency Levels

##### 1.1 Commercial platform

| Concurrency Level  | Average Response Time (ms) | **Min (ms)** | **Max (ms)** | **Throughput (req/sec)** | **Error %** |
| ------------------ | -------------------------- | ------------ | ------------ | ------------------------ | ----------- |
| 10 Users * 10times | 465                        | 158          | 1142         | 15.1                     | 0.00%       |
| 100 Users*10times  | 2932                       | 353          | 4952         | 32.1                     | 0.00%       |

##### 1.2 openwhisk

| **Concurrency Level** | **Average Response Time (ms)** | **Min (ms)** | **Max (ms)** | **Throughput (req/sec)** | **Error %** |
| --------------------- | ------------------------------ | ------------ | ------------ | ------------------------ | ----------- |
| 10 Users*10times      | 650                            | 200          | 1800         | 12.8                     | 0.00%       |
| 100 Users*10times     | 4100                           | 500          | 9200         | 25.4                     | 2.50%       |

#### **2. Cold Start vs. Warm Execution**

##### 2.1 azure

Cold start tests were conducted by stopping the function app for 10 minutes before invoking it again.

| **Execution Type** | **First Request Time (ms)** | **Subsequent Requests (ms, Avg.)** |
| ------------------ | --------------------------- | ---------------------------------- |
| Cold Start         | 2200                        | 465                                |
| Warm Start         | 158                         | 465                                |

##### 2.2 openwhisk

| **Execution Type** | **First Request Time (ms)** | **Subsequent Requests (ms, Avg.)** |
| ------------------ | --------------------------- | ---------------------------------- |
| Cold Start         | 2302                        | 556                                |
| Warm Start         | 211                         | 556                                |

#### 3. relation with the papers

![image-20250320051703803](/Users/czl/Library/Application Support/typora-user-images/image-20250320051703803.png)