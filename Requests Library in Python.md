# Requests Library in Python

## Introduction

The `requests` library in Python is a powerful and user-friendly HTTP library for sending HTTP/1.1 requests. It abstracts the complexities of making requests and handling responses, making it easier to work with web APIs and HTTP communication.

## Installation

To use the `requests` library, install it using the following command:

```sh
uv add requests
```

## Making HTTP Requests

The `requests` library allows you to send various types of HTTP requests:

### 1. Sending a GET Request

A `GET` request retrieves data from a specified URL.

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts/1")
print(response.json())  # JSON response
```

#### Explanation:
- `requests.get(url)`: Sends a `GET` request.
- `.json()`: Converts the response to JSON format.

### 2. Sending a POST Request

A `POST` request is used to send data to a server.

```python
import requests

data = {"title": "foo", "body": "bar", "userId": 1}
response = requests.post("https://jsonplaceholder.typicode.com/posts", json=data)
print(response.json())
```

#### Explanation:
- `requests.post(url, json=data)`: Sends a `POST` request with JSON data.
- `.json()`: Parses the JSON response.

### 3. Sending a PUT Request

A `PUT` request updates an existing resource.

```python
import requests

data = {"title": "updated title", "body": "updated body", "userId": 1}
response = requests.put("https://jsonplaceholder.typicode.com/posts/1", json=data)
print(response.json())
```

#### Explanation:
- `requests.put(url, json=data)`: Sends a `PUT` request to update a resource.

### 4. Sending a DELETE Request

A `DELETE` request removes a resource.

```python
import requests

response = requests.delete("https://jsonplaceholder.typicode.com/posts/1")
print(response.status_code)  # 200 indicates success
```

#### Explanation:
- `requests.delete(url)`: Sends a `DELETE` request.
- `.status_code`: Returns the HTTP status code.

## Handling Response Objects

The `requests` library provides response objects with useful attributes:

```python
response = requests.get("https://jsonplaceholder.typicode.com/posts/1")

print(response.status_code)  # HTTP status code
print(response.headers)  # Response headers
print(response.text)  # Raw response text
print(response.json())  # JSON response
```

## Sending Custom Headers

Custom headers can be included in a request:

```python
headers = {"Authorization": "Bearer my_token"}
response = requests.get("https://api.example.com/data", headers=headers)
```

## Handling Timeouts

Timeouts prevent hanging requests:

```python
response = requests.get("https://jsonplaceholder.typicode.com/posts", timeout=5)  # 5 seconds timeout
```

## Handling Exceptions

To handle request failures, use exception handling:

```python
import requests

try:
    response = requests.get("https://invalid-url.com", timeout=5)
    response.raise_for_status()
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

## Summary

- `requests.get()`, `requests.post()`, `requests.put()`, `requests.delete()` for HTTP requests.
- Response objects contain status codes, headers, text, and JSON data.
- Custom headers can be included in requests.
- Timeouts and exception handling improve reliability.

**Refer to the Technical Notes Guidelines for formatting and best practices.**

