# FastAPI Setup Using `uv`

## Step 1: Initialize `uv`

Initialize the `uv` package manager to manage dependencies efficiently.

```sh
uv init
```

## Step 2: Add `ruff` (Linting Tool)

Add `ruff`, a fast Python linter, to ensure clean and optimized code.

```sh
uv add ruff
```

## Step 3: Activate Your Virtual Environment

Activate your virtual environment on Windows to manage dependencies separately.

```sh
.venv\Scripts\activate
```

## Step 4: Add FastAPI

Install FastAPI along with its standard dependencies to build APIs quickly.

```sh
uv add fastapi[standard]
```

## Step 5: Run Your FastAPI Server

Start the FastAPI development server to test and interact with your API.

```sh
fastapi dev fileName.py
```

## Step 6: Create a GET API Request

The following code sets up a basic FastAPI application with a simple `GET` endpoint that returns JSON data.

```python
# FastAPI se import kar rahe hain, taaki hum API bana sakein
from fastapi import FastAPI

# FastAPI ka ek instance bana rahe hain
app = FastAPI()

# Yeh ek GET request ka endpoint define kar raha hai jo root ("/") pe chalega
@app.get("/")
def get_function():  # Ek function define kiya jo execute hoga jab yeh endpoint hit hoga
  # logic
  # logic
  # logic 
return {"name": "Taha"}  # Yeh function ek JSON response return karega
```

## Step 7: Create a POST API Request

The following code adds a `POST` endpoint that accepts JSON data and returns a response message.

```python
from fastapi import FastAPI
from pydantic import BaseModel # request validation ke liye

app = FastAPI()

class Data(BaseModel):  
name: str          
age: int          

@app.post("/data")                    # URL
def add_data(data: Data):             # URL par jane se ye function chal jaye ga.
  # logic
  # logic
  # logic  
return {
      "message" : f"Data received: Name: {data.name}, Age: {data.age}"
  } # Json Object
```

## Step 8: Delete API Request

Define a `DELETE` endpoint to remove an item using its ID.

```python
@app.delete("/data/{item_id}")  # Item ko delete karne ka endpoint
def delete_data(item_id: int):
  # logic
  # logic
  # logic
return {"message": f"Item with ID {item_id} deleted successfully"}
```

## Step 9: Update API Request

Define a `PUT` endpoint to modify an existing record based on its ID.

```python
@app.put("/data/{item_id}")
def update_data(item_id: int, data: Data):
  # logic
  # logic
  # logic
return {
      "message": f"Item with ID {item_id} updated to Name: {data.name}, Age: {data.age}"
  }
```

## Step 10: Check Your API in Swagger UI

FastAPI provides a built-in interactive documentation interface using Swagger UI. After running the FastAPI server, open the following link in your browser:

```
http://127.0.0.1:8000/docs
```

## Step 11: Test Your API in Thunder Client or Postman

Besides Swagger UI, you can also test your API using **Thunder Client** (VS Code Extension) or **Postman**.

- Open Thunder Client/Postman.
- Enter the endpoint URL (`http://127.0.0.1:8000/` for GET, `/data` for POST, etc.).
- Select the appropriate HTTP method (`GET`, `POST`, `DELETE`, `PUT`).
- Send the request and verify the response.

---

### Comments

- The `uv` tool is used to manage dependencies efficiently.
- The `FastAPI` framework simplifies API creation with built-in support for request validation using `Pydantic`.
- The `DELETE` endpoint removes an item using an `ID`, while the `PUT` endpoint updates an existing record.
- Use **Swagger UI, Thunder Client, or Postman** to test API requests effectively.

---


# FastAPI Deploy Setup on `Vercel`

## Step 1: Create `requirements.txt`

Make a `requirements.txt` file in your project.

## Step 2: Write `requirements.txt`

Write/Mention all of your library names in your `requirements.txt` file.

## Step 3: Create a `vercel.json`

Make a `vercel.json` file in your project. Use the code below and modify according to your needs.

```json
{
  "version": 2,
  "builds": [
    {
      "src": "your_file_name.py",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/your_file_name.py"
    }
  ]
}
```

## Step 4: Push on GitHub

Push your project to GitHub.

## Step 5: Deploy on Vercel

Deploy your project on Vercel.

---
