Hadoop_Report

Performance Report for Hadoop MapReduce Job

1. Job Overview
   - Job ID: job_local201838363_0001
   - Input: 4 files from /user/ubuntu/gutenberg/*
   - Output: /user/ubuntu/gutenberg-output
   - Execution Date: October 25, 2024

2. Execution Time
   - Total Job Runtime: 8.208 seconds (real time)
   - CPU Time: 11.039 seconds (user time)
   - System Time: 0.799 seconds

3. Resource Utilization
   - Number of Map Tasks: 4
   - Number of Reduce Tasks: 1
   - Total Committed Heap Usage: 1,885,863,936 bytes (≈1.76 GB)

4. Data Processing Statistics
   - Input Records Processed: 91,936
   - Map Output Records: 755,958
   - Reduce Input Records: 755,958
   - Reduce Output Records: 60,383
   - Input Data Read: 4,521,984 bytes (≈4.31 MB)
   - Output Data Written: 658,051 bytes (≈642 KB)

5. Performance Metrics
   - Map Input Processing Rate: 11,203 records/second
   - Map Output Generation Rate: 92,101 records/second
   - Reduce Processing Rate: 92,101 input records/second
   - Reduce Output Generation Rate: 7,354 records/second
   - Overall Data Throughput: 0.52 MB/s (input) to 0.08 MB/s (output)

6. Resource Allocation (based on capacity-scheduler.xml)
   - Maximum Applications: 10,000
   - Maximum AM Resource Percent: 10%
   - Default Queue Capacity: 100%
   - Default Queue Maximum Capacity: 100%

7. Job Phases Analysis
   - Map Phase:
     * Fastest Map Task: ≈0.3 seconds
     * Slowest Map Task: ≈0.7 seconds
   - Shuffle and Sort Phase:
     * Total Time: ≈0.1 seconds
   - Reduce Phase:
     * Total Time: ≈1.5 seconds
