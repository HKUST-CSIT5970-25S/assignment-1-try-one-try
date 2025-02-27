[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: CHANG, Rui He
### Student Id: 21109304
### Email: rchangab@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    >  **The benchmarking tool I used is the same as tutorial: Phoronix Test Suite, iPerf and Ping.**

    >  **For N.Virginia, my server and client is at us-east-1c**
    
    >  **For Oregon, my server and client is at us-west-2a**

    >For **CPU performance**, I used the pts/compress-7zip test. I chose this test because it gives a clear indication of how the CPU performs under heavy load. **For the value of my measurement result, I choose to use average compression rate.**

    >For **memory performance**, I used the pts/ramspeed test. I selected this test to assess how efficiently the system handles memory access and data throughput. **For the value of my measurement result, I choose to use average speed of copying Integer.** 

    >For **TCP bandwith performance**, I used the iPerf test with 256k window size. I selected this test to measure the network throughput between end hosts. **For the value of my measurement result, I choose Mbps (Gbits/sec times 1000).** 

    >For **TCP RTT performance**, I used the Ping test directly. I selected this test to measure the measure the packet round-trip time (RTT) between end hosts. **For the value of my measurement result, I choose average round trip time in ms.** 

The tool automatically reports the performance scores in terms of MIPS for CPU and MB/s for memory, providing a clear comparison of system performance.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |  avg compression rate: 3441 MIPS               |      avg copy Integer: 10346.39 MB/S              |
    | `t2.medium`  |  avg compression rate: 9743 MIPS              |     avg copy Integer: 19372.39 MB/S               |
    | `c5d.large` |   avg compression rate: 7753 MIPS              |     avg copy Integer: 13887.44 MB/S               |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.


   >    Yes, t2.micro with 1 vCPUs and 1 GiB memory, t2.medium with 2 vCPUs and 4 GiB memory, c5d.large with 2 vCPUs and 4 GiB memory (same with t2.medium), obviously, the performance of t2.micro is less than t2.medium and c5d.large, so the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource. **However, t2.medium and c5d.large with same 2 vCPUs and 4 GiB memory, but it is strange that c5d.large's performance is less than t2.medium.**

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |    3020 Mbps            |   avg 0.411 ms       |
    | `m5.large` - `m5.large`   |    4710 Mbps            |   avg 0.262 ms       |
    | `c5n.large` - `c5n.large` |    4880 Mbps            |   avg 0.172 ms       |
    | `t3.medium` - `c5n.large` |    3910 Mbps            |   avg 0.302 ms       |
    | `m5.large` - `c5n.large`  |    8970 Mbps            |   avg 0.110 ms       |
    | `m5.large` - `t3.medium`  |    3940 Mbps            |   avg 0.312 ms       |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    > First of all, I observe that with the more bandwidth, the less RTT. Then, the network performance for instances of the same type is generally better than that of different types. However, there are two exceptions: 

    >`t3.medium` - `t3.medium`'s performance is worse than `t3.medium` - `c5n.large` and `m5.large` - `t3.medium`. 

    >`m5.large` - `c5n.large`'s performance is distinctly outstanding than all  instances of the same type.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |   32.2 Mbps             |  avg 64.009 ms        |
    | N. Virginia - N. Virginia |   4410 Mbps             |  avg 0.298 ms        |
    | Oregon - Oregon           |   4580 Mbps             |  avg 0.183 ms        |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    > For the network performance for instances deployed in different regions, can see that the bandwith is much less than instances in the same region, and the RTT for instances in different regions is much higher than instances in the same region.
