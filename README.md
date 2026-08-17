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

The system therefore needs an architecture that can handle increased workload without becoming slow or unreliable.

### Main problems

- Meeting summaries may be delayed.
- Important updates may appear late.
- Processing becomes slower during busy periods.
- A single server could become a bottleneck.
- Failure of one component could interrupt processing.
- Meeting information needs secure communication.
- The system must support increasing usage.
