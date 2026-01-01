---
title: 'Jobs API'
---

# Jobs API Documentation

This document provides an overview and usage examples for the provided APIs. Each API endpoint includes a description,
request parameters, and Python code examples to help you integrate these APIs into your applications.

## Table of Contents

- [Authentication](#authentication)
- [Endpoints](#endpoints)
    - [Jobs Add](#jobs-add)
    - [Jobs Fetch](#jobs-fetch)
    - [Jobs Delete](#jobs-delete)

## Authentication

All endpoints require two URL parameters for authentication:

- `magic_id`: A unique identifier for the magic resource.
- `access_key`: An access key associated with the magic resource.

## Endpoints

### Jobs Add

Adds a new job to the system.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/jobs/add
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "due": 1638316800,       // Optional, UNIX timestamp
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
  import time

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/jobs/add"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "due": int(time.time()) + 3600,  # Job due in 1 hour
      "body": {"task": "process_data"},
      "headers": {"priority": "high"},
      "ref": "unique_job_ref_123"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      data = response.json()
      print(f"Job added with key: {data['job']}")
  else:
      print(f"Error: {response.status_code}")
  ```

### Jobs Fetch

Fetches a list of jobs associated with the magic resource.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/jobs/fetch
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  *(No body required)*

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

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/jobs/fetch"
  headers = {
      "Content-Type": "application/json"
  }

  response = requests.post(url, headers=headers)

  if response.status_code == 200:
      jobs = response.json()
      for job in jobs:
          print(job)
  else:
      print(f"Error: {response.status_code}")
  ```

### Jobs Delete

Deletes a job based on its reference ID.

- **URL**

  ```
  POST /magics/<magic_id>/<access_key>/jobs/delete
  ```

- **Request Headers**

  ```
  Content-Type: application/json
  ```

- **Request Body**

  ```json
  {
    "ref": "job_reference"
  }
  ```

- **Response**

    - **Success (200)**: Job deleted successfully.
    - **Error (404)**: Job not found.
    - **Error (406)**: Job is running and cannot be cancelled.

- **Example Python Usage**

  ```python
  import requests

  url = "https://v226251f-b3f1-db.cablo.io/magics/<magic_id>/<access_key>/jobs/delete"
  headers = {
      "Content-Type": "application/json"
  }
  payload = {
      "ref": "unique_job_ref_123"
  }

  response = requests.post(url, json=payload, headers=headers)

  if response.status_code == 200:
      print("Job deleted successfully.")
  elif response.status_code == 404:
      print("Job not found.")
  elif response.status_code == 406:
      print("Job is running and cannot be cancelled.")
  else:
      print(f"Error: {response.status_code}")
  ```

## Notes

- Replace `<magic_id>` and `<access_key>` in the URLs with your actual `magic_id` and `access_key`.
- All timestamps are in UNIX time (seconds since the epoch).
- All time-related values in request bodies or responses are expected to be in microseconds (`time` fields) or
  seconds (`due` fields).
- Ensure that you handle HTTP status codes appropriately in your magics.