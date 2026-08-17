# AI Meeting Assistant
## 1. Project Overview

The AI Meeting Assistant is designed to capture important information from virtual meetings,
generate summaries and action items, and make the results available shortly after the meeting.

As the number of meetings increases, the system may experience delayed updates, slow summaries and inconsistent performance.

The proposed architecture focuses on making the system:

- Responsive
- Reliable
- Secure
- Available
- Scalable

## 2. Problem Statement

When the number of meetings increases, the system has to process more data and more requests at the same time.

A simple architecture with a single server could become overloaded:

```text
More Meetings
      ↓
More Processing Requests
      ↓
Server Overload
      ↓
Higher Latency
      ↓
Delayed Summaries
      ↓
Poor User Experience
```

The system therefore needs an architecture that can handle increased workload without becoming slow or unreliable.

### Main problems

- Meeting summaries may be delayed.
- Important updates may appear late.
- Processing becomes slower during busy periods.
- A single server could become a bottleneck.
- Failure of one component could interrupt processing.
- Meeting information needs secure communication.
- The system must support increasing usage.

## 3. Objectives

The proposed architecture aims to:

1. Keep the application responsive.
2. Handle increasing numbers of meetings.
3. Process multiple meeting jobs simultaneously.
4. Handle sudden increases in workload.
5. Continue processing when individual workers fail.
6. Protect meeting data during communication.
7. Maintain service availability.
8. Provide real-time or near-real-time notifications.
9. Demonstrate how Network Foundations concepts can solve a real-world system problem.

## 4. Architecture
The proposed system uses multiple servers, a message queue, processing workers, secure communication, and real-time notifications.

### Architecture Flow

```text
                  AI MEETING ASSISTANT
                           │
                           ▼
                  ┌─────────────────┐
                  │     USERS       │
                  │   Employees     │
                  └────────┬────────┘
                           │
                       HTTPS / TLS
                           │
                           ▼
                  ┌─────────────────┐
                  │  LOAD BALANCER  │
                  └────────┬────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
       ┌──────────────┐          ┌──────────────┐
       │ API SERVER 1 │          │ API SERVER 2 │
       └──────┬───────┘          └──────┬───────┘
              └────────────┬────────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   API GATEWAY   │
                  │ Auth + Rate     │
                  │ Limiting        │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │  MESSAGE QUEUE  │
                  │ Kafka / RabbitMQ │
                  │ / AWS SQS        │
                  └────────┬────────┘
                           │
                ┌──────────┴──────────┐
                ▼                     ▼
       ┌────────────────┐    ┌─────────────────┐
       │ TRANSCRIPTION  │    │ AI SUMMARIZATION│
       │    SERVICE     │───►│     SERVICE     │
       └────────────────┘    └────────┬────────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │   DATABASE    │
                              └───────┬───────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │ NOTIFICATION  │
                              │ Email/Teams   │
                              └───────┬───────┘
                                      │
                                      ▼
                               User receives
                              summary + tasks
```


## 5. System Workflow

1. Employee participates in a virtual meeting.
2. Meeting information is transmitted securely using HTTPS/TLS.
3. The Load Balancer distributes incoming requests across available servers.
4. The API Gateway performs authentication, validation and rate limiting.
5. Processing requests are placed into a Message Queue.
6. Transcription workers convert speech into text.
7. The AI Summarization service generates summaries and action items.
8. Results are stored in the database.
9. The Notification Service informs the user when the summary is ready.


## 6. Network Foundations Concepts Used

| Concept                 | Application in this project                                                    |
| ----------------------- | ------------------------------------------------------------------------------ |
| **TCP/IP**              | Provides the basic communication foundation between clients and services.      |
| **HTTP/HTTPS**          | Used for client-server API communication.                                      |
| **TLS**                 | Encrypts communication containing meeting information.                         |
| **WebSocket**           | Provides real-time notifications to users.                                     |
| **gRPC**                | Can be used for efficient communication between internal services if required. |
| **Latency**             | Represents the delay between a request and the availability of a result.       |
| **Throughput**          | Represents how many meeting-processing jobs the system can handle over time.   |
| **Message Queue**       | Buffers jobs during periods of high demand.                                    |
| **Distributed Systems** | Multiple servers and workers cooperate to provide the service.                 |
| **Horizontal Scaling**  | Additional API servers or workers can be added as usage increases.             |
| **Fault Tolerance**     | Multiple workers and queued jobs reduce the impact of individual failures.     |

---

## 7. Reliability
- Multiple instances
- load-balancer health checks
- durable queues
- retries
- database backups/replication and monitoring help prevent a single failure from stopping the service.

## 8. Security
- Use HTTPS/TLS
- authentication
- authorization/RBAC
- encryption at rest
- secure secret storage and audit logging.

## 9. Scalability

The architecture supports increasing usage by adding more application and processing instances.

For example:

```text
Low traffic
     |
Server 1 + Server 2

High traffic
     |
Server 1 + Server 2 + Server 3 + Server 4 + Server 5
```

Workers can also be increased based on queue length and system load.

---

## 10. Demonstration Plan

A simple demonstration can be used to show how the architecture behaves under different workloads.

### Test 1 : Normal Load

Send a small number of meeting-processing jobs.

Expected result:

```text
Jobs → Queue → Workers → Completed
```

Processing should complete normally with low waiting time.

### Test 2 : Increased Load

Send a much larger number of jobs.

Expected result:

```text
Many Jobs
    ↓
Queue
    ↓
Workers process jobs
    ↓
All jobs eventually complete
```

The queue prevents all requests from directly overwhelming the workers.

### Test 3 : Scaling

Run the same workload with different numbers of workers.

For example:

```text
3 Workers → Processing time A

6 Workers → Processing time B
```

The expected result is increased processing throughput when more workers are available.

### Test 4 : Worker Failure


If one application server fails:

```text
             Load Balancer
              /          \
             ↓            ↓
        Server 1       Server 2
                          X
                       FAILED
```

The Load Balancer detects the unhealthy server and sends new requests to healthy instances.

---
## 11. Handling Increasing Usage

The architecture is designed to scale horizontally.

### Normal workload

```text
100 Meetings
     ↓
Queue
     ↓
3 Workers
```

### Increased workload

```text
1,000 Meetings
      ↓
    Queue
      ↓
6 Workers
```

### Very high workload

```text
10,000 Meetings
       ↓
     Queue
       ↓
More Workers
```

Instead of replacing one server with an increasingly powerful server, additional instances can be added.

This allows the system to increase processing capacity as demand grows.

---
## 12. Assumptions
 
- Users are authenticated employees.
- Meetings are conducted using approved platforms.
- Meeting information may contain confidential data.
- Summary generation is expected within a few minutes after the meeting.
- Cloud infrastructure is available for deployment.

## 13. Conclusion

The proposed AI Meeting Assistant architecture uses several Network Foundations concepts to create a system that can support increasing usage.
HTTPS and TLS provide secure communication. A load balancer distributes requests across multiple API servers. A message queue absorbs sudden increases in processing requests, while multiple workers process jobs in parallel. The system can scale horizontally by adding more servers and workers as demand increases.
WebSocket can provide real-time notifications when meeting summaries are ready. Multiple workers and queued jobs also improve reliability by reducing the impact of individual component failures.Overall, the architecture is designed to keep the AI Meeting Assistant responsive, reliable, secure, available, and scalable as the number of meetings increases.



