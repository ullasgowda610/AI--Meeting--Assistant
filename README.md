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
# 4. Proposed Architecture

![AI Meeting Assistant Architecture](architecture.png)

The proposed system uses multiple servers, a message queue, processing workers, secure communication, and real-time notifications.

### Architecture Flow

```text
                         USER
                          │
                        HTTPS
                          │
                          ▼
                   Load Balancer
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
            API Server 1      API Server 2
                 │                 │
                 └────────┬────────┘
                          ▼
                    Message Queue
                          │
                ┌─────────┼─────────┐
                ▼         ▼         ▼
             Worker 1  Worker 2  Worker 3
                │         │         │
                └─────────┼─────────┘
                          ▼
                     AI Processing
                          │
                          ▼
                       Database
                          │
                          ▼
                     Notification
                          │
                       WebSocket
                          │
                          ▼
                         USER
```

---

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

## 6. Key Networking Concepts









# 5. Component Responsibilities

## 5.1 User / Client

The employee uses the meeting-assistant application.

The client can:

* Send meeting-related requests.
* View meeting information.
* Request summaries.
* Receive notifications when processing is complete.

Communication with the backend uses HTTPS.

---

## 5.2 HTTPS and TLS

The client communicates with the backend using **HTTPS**.

```text
User
 │
 │ HTTPS + TLS
 ▼
Backend
```

HTTPS provides secure communication, while TLS encrypts the data transmitted between the client and server.

This is important because meeting transcripts and summaries may contain sensitive company information.

---

## 5.3 Load Balancer

The load balancer distributes incoming requests between multiple API servers.

```text
                Load Balancer
                 /         \
                ↓           ↓
           API Server 1  API Server 2
```

Without a load balancer, a single server could become overloaded.

With multiple servers, traffic can be distributed across available instances.

This improves:

* Availability
* Scalability
* Reliability

---

## 5.4 API Servers

API servers handle requests from users and communicate with internal services.

For example:

```text
User
 ↓
API Server
 ↓
Create processing job
 ↓
Message Queue
```

Multiple API servers can handle requests simultaneously.

If demand increases, additional API server instances can be added.

---

## 5.5 Message Queue

The message queue is used to temporarily store processing jobs.

For example:

```text
Meeting 1 ─┐
Meeting 2 ─┤
Meeting 3 ─┤
Meeting 4 ─┤
Meeting 5 ─┤
           ▼
       Message Queue
```

Workers then process the jobs from the queue.

The queue prevents a sudden increase in meeting activity from directly overwhelming the processing services.

### Example

If 1,000 meetings finish at approximately the same time:

```text
1000 Jobs
   ↓
Queue
   ↓
Workers process jobs gradually
```

This helps absorb traffic spikes.

---

## 5.6 Processing Workers

Workers perform the processing required for each meeting.

A worker can:

1. Receive a meeting-processing job.
2. Process the transcript.
3. Send the transcript to the AI service.
4. Generate the summary.
5. Extract important updates.
6. Store the result.

Multiple workers can operate in parallel:

```text
             Queue
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
    Worker 1 Worker 2 Worker 3
```

This increases throughput.

If the workload increases, more workers can be added.

---

## 5.7 AI Processing

The AI processing component receives meeting transcripts and generates useful information.

Example:

```text
Transcript
    ↓
AI Processing
    ↓
Summary
    ↓
Important Updates
    ↓
Action Items
```

The AI model itself is outside the main scope of this project. The project focuses on how the surrounding network and system architecture delivers work to and from the AI processing component reliably.

---

## 5.8 Database

The database stores information such as:

* Meeting details
* Transcripts
* Summaries
* Important updates
* Action items
* Processing status

The database allows users to retrieve meeting information later.

---

## 5.9 WebSocket

WebSocket can be used to provide real-time notifications.

Instead of the client repeatedly asking:

```text
"Is my summary ready?"
```

the server can notify the client when processing is complete.

```text
AI Processing Complete
          ↓
      WebSocket
          ↓
        User
          ↓
"Your meeting summary is ready."
```

This helps provide a responsive user experience.

---

# 6. Network Foundations Concepts Used

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
