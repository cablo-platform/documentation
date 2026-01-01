---
title: 'Queues API'
---

# Queues API Documentation

This document provides an overview and usage examples for the provided APIs. Each API endpoint includes a description,
request parameters, and Python code examples to help you integrate these APIs into your applications.

## Table of Contents

- [Authentication](#authentication)
- [Endpoints](#endpoints)
    - [Queues Enqueue](#queues-enqueue)
    - [Queues Dequeue](#queues-dequeue)
    - [Queues Fetch](#queues-fetch)
    - [Queues Clear](#queues-clear)

## Authentication

All endpoints require two URL parameters for authentication:

- `magic_id`: A unique identifier for the magic resource.
- `access_key`: An access key associated with the magic resource.

## Endpoints

### Queues Enqueue

Adds a job to a specific queue.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/queues/enqueue
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "queue": "queue_name",
    "body": { /* job body */ },
    "headers": { /* job headers */ },
    "ref": "unique_reference" // Optional
  }
  ```

- **Response**

  ```json
  {
    "job": "job_key"
  }
  ```

- **Example Python Usage**

  ```python
  import requests

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/queues/enqueue"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "queue": "default_queue",
      "body": {"task": "process_data"},
      "headers": {"priority": "high"},
      "ref": "unique_queue_ref_456"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      data = response.json()
      print(f"Job enqueued with key: {data['job']}")
  else:
      print(f"Error: {response.status_code}")
  ```

### Queues Dequeue

Removes a job from a specific queue.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/queues/dequeue
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "queue": "queue_name",
    "ref": "job_reference"
  }
  ```

- **Response**

    - **Success (200)**: Job dequeued successfully.
    - **Error (404)**: Job or queue not found.

- **Example Python Usage**

  ```python
  import requests

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/queues/dequeue"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "queue": "default_queue",
      "ref": "unique_queue_ref_456"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      print("Job dequeued successfully.")
  else:
      print(f"Error: {response.status_code}")
  ```

### Queues Fetch

Fetches jobs from a specific queue.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/queues/fetch
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "queue": "queue_name"
  }
  ```

- **Response**

  ```json
  [
    {
      "ref": "job_reference",
      "due": 1638316800,
      "key": "job_key"
    }
    // ... more items
  ]
  ```

- **Example Python Usage**

  ```python
  import requests

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/queues/fetch"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "queue": "default_queue"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      jobs = response.json()
      for job in jobs:
          print(job)
  else:
      print(f"Error: {response.status_code}")
  ```

### Queues Clear

Clears all jobs from a specific queue and marks it as inactive.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/queues/clear
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "queue": "queue_name"
  }
  ```

- **Response**

    - **Success (200)**: Queue cleared successfully.
    - **Error (404)**: Queue not found.

- **Example Python Usage**

  ```python
  import requests

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/queues/clear"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "queue": "default_queue"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      print("Queue cleared successfully.")
  else:
      print(f"Error: {response.status_code}")
  ```

## Notes

- Replace `<magic_id>` and `<access_key>` in the URLs with your actual `magic_id` and `access_key`.
- All timestamps are in UNIX time (seconds since the epoch).
- All time-related values in request bodies or responses are expected to be in microseconds (`time` fields) or
  seconds (`due` fields).
- Ensure that you handle HTTP status codes appropriately in your applications.