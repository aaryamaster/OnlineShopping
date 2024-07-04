
# Web Application using Flask + Cloud Deployment Demo

<img width="1436" alt="Screenshot 2024-07-04 at 2 30 20 PM" src="https://github.com/aaryamaster/OnlineShopping/assets/75297750/3ec5ec2d-8762-45d6-874e-2d32894ac7d5">


## Open Visual Studio and select Create a new project

1. Select **Python Application** and click **Next**.
2. Configure your new project:
   - **Project name**: `OnlineShopping`
   - **Location**: Choose your desired directory
   - Click **Create**

## Set Up Flask Application

### Create a Virtual Environment

1. Open your terminal and navigate to your project directory.
2. Run the following command to create a virtual environment:

    ```bash
    python -m venv venv
    ```

3. Activate the virtual environment:

    **Windows:**

    ```bash
    .\venv\Scripts\activate
    ```

    **macOS/Linux:**

    ```bash
    source venv/bin/activate
    ```

4. Install Flask within the virtual environment:

    ```bash
    pip install flask
    ```

### Create the Flask App

1. In `OnlineShopping.py`, add the following code:

```python
from flask import Flask, request, render_template, redirect, url_for
import os

app = Flask(__name__)

# Load products from file
def load_products():
    products = []
    # Check if the file exists before trying to read it
    if os.path.exists('products.txt'):
        with open('products.txt', 'r') as file:
            for line in file:
                parts = line.strip().split(',')
                if len(parts) == 5:
                    product = {
                        'id': parts[0],
                        'name': parts[1],
                        'price': parts[2],
                        'type': parts[3],
                        'image': parts[4]
                    }
                    products.append(product)
    return products

# Save a new product to the file
def save_product(product):
    # Append a new product to the file with a newline
    with open('products.txt', 'a') as file:
        if os.path.getsize('products.txt') > 0:
            file.write("\n")
        file.write(f"{product['id']},{product['name']},{product['price']},{product['type']},{product['image']}")

        
@app.route('/')
def index():
    products = load_products()
    return render_template('index.html', products=products)

@app.route('/add', methods=['GET', 'POST'])
def add():
    if request.method == 'POST':
        new_product = {
            'id': request.form['id'],
            'name': request.form['name'],
            'price': request.form['price'],
            'type': request.form['type'],
            'image': request.form['image']
        }
        save_product(new_product)
        return redirect(url_for('index'))
    return render_template('add.html')

if __name__ == '__main__':
    app.run(debug=True)

 ```

### Create Templates Folder

1. In the project directory, create a folder named `templates`.
2. Inside `templates`, create two files: `index.html` and `add.html`.

## Create index.html

1. Inside `templates`, create `index.html` with the following content:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Online Shopping</title>
    <link
      rel="stylesheet"
      href="{{ url_for('static', filename='styles.css') }}"
    />
  </head>
  <body>
    <header>
      <div class="header-content">
        <h1>Online Shopping</h1>
        <a href="{{ url_for('add') }}" class="add-product-button"
          >Add New Product</a
        >
      </div>
    </header>
    <main>
      <div class="product-list">
        {% for product in products %}
        <div class="product">
          <img src="{{ product.image }}" alt="{{ product.name }}" />
          <div class="product-info">
            <h2>{{ product.name }}</h2>
            <p class="price">Price: ${{ product.price }}</p>
            <p class="type">Type: {{ product.type }}</p>
          </div>
        </div>
        {% endfor %}
      </div>
    </main>
    <footer>
      <p>&copy; 2024 Online Shopping. All rights reserved.</p>
    </footer>
  </body>
</html>

```

## Create add.html

1. Inside `templates`, create `add.html` with the following content:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Add Product</title>
    <link
      rel="stylesheet"
      href="{{ url_for('static', filename='styles.css') }}"
    />
  </head>
  <body>
    <header>
      <div class="header-content">
        <h1>Add a New Product</h1>
        <a href="{{ url_for('index') }}" class="back-button"
          >Back to Products</a
        >
      </div>
    </header>
    <main>
      <form action="{{ url_for('add') }}" method="post">
        <label for="id">ID:</label>
        <input type="text" id="id" name="id" required /><br />
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required /><br />
        <label for="price">Price:</label>
        <input type="text" id="price" name="price" required /><br />
        <label for="type">Type:</label>
        <input type="text" id="type" name="type" required /><br />
        <label for="image">Image URL:</label>
        <input type="text" id="image" name="image" required /><br />
        <input type="submit" value="Add Product" />
      </form>
    </main>
    <footer>
      <p>&copy; 2024 Online Shopping. All rights reserved.</p>
    </footer>
  </body>
</html>

 ```

## Add CSS for Better UI

### Create Static Folder

1. In the project directory, create a folder named `static`.
2. Inside `static`, create a file named `styles.css` with the following content:

```css
    body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #343a40;
    color: white;
    padding: 10px 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

header h1 {
    margin: 0;
}

.add-product-button, .back-button {
    background-color: #007bff;
    color: white;
    padding: 10px 20px;
    text-decoration: none;
    border-radius: 5px;
    transition: background-color 0.3s ease;
}

.add-product-button:hover, .back-button:hover {
    background-color: #0056b3;
}

main {
    padding: 20px;
}

.product-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
}

.product {
    background-color: white;
    border: 1px solid #ddd;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 220px;
    text-align: center;
    transition: transform 0.2s;
}

.product:hover {
    transform: scale(1.05);
}

.product img {
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    max-width: 100%;
    height: 150px;
    object-fit: cover;
}

.product-info {
    padding: 10px;
}

.product h2 {
    font-size: 1.2em;
    margin: 10px 0;
}

.price {
    color: #28a745;
    font-weight: bold;
}

.type {
    color: #6c757d;
}

form {
    display: flex;
    flex-direction: column;
    align-items: center;
}

form label, form input {
    margin: 5px;
}

footer {
    background-color: #343a40;
    color: white;
    text-align: center;
    padding: 10px 0;
    position: fixed;
    width: 100%;
    bottom: 0;
}

```

## Add Products to Text File

### Create products.txt

1. In the project directory, create a file named `products.txt`.
2. Add product details in the following format:

```plaintext
1,Product A,10.99,Type 1,https://images.unsplash.com/photo-1526170375885-4d8ecf77b99f
2,Product B,15.49,Type 2,https://images.unsplash.com/photo-1567306226416-28f0efdc88ce
3,Product C,8.99,Type 3,https://loremflickr.com/cache/resized/65535_51774384039_624f926674_q_150_150_nofilter.jpg
```

## Run the Application

### Run the Flask App

1. In Visual Studio, set the startup file to `OnlineShopping.py` if not already set.
2. Click **Run** or press **F5** to start the server.
3. Open your web browser and navigate to [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

## Push to GitHub

### Initialize Git

1. In your project directory, open a terminal and run:

    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    ```

### Create a GitHub Repository

1. Go to GitHub and create a new repository named `OnlineShopping`.

### Push to GitHub

1. Follow the instructions provided by GitHub to push your local repository to GitHub:

    ```bash
    git remote add origin https://github.com/yourusername/OnlineShopping.git
    git branch -M main
    git push -u origin main
    ```

## Deploy to Google Cloud Platform

1. **Install Google Cloud SDK**: Follow the instructions to install the Google Cloud SDK [https://cloud.google.com/sdk/docs/install-sdk].
2. **Create a New Project**: Go to the Google Cloud Console and create a new project.
3. **Enable Billing**: Ensure billing is enabled for your project.
4. **Deploy the Application**:

    1. In your project directory, create an `app.yaml` file with the following content:

        ```yaml
        runtime: python39

        entrypoint: gunicorn -b :$PORT OnlineShopping:app

        env_variables:
          PYTHON_ENV: 'development'
        ```

    2. Deploy your application:

        ```bash
        gcloud app deploy
        ```

5. Access your deployed application at `https://onlineshopping-428400.ue.r.appspot.com/`.


# Setting Up CI/CD Pipeline with GitHub Actions

## Introduction

In this tutorial, we will set up a Continuous Integration/Continuous Deployment (CI/CD) pipeline for our Python project using GitHub Actions. This pipeline will automatically run tests whenever we push code to the repository or create a pull request and deploy the application to Google Cloud.

## Step-by-Step Guide

### Step 1: Create the Workflow File

First, create a directory named `.github/workflows` in the root of your project. Inside this directory, create a file named `testcase.yml`. This file will define our CI/CD workflow.

```name: Auto Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.9'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run tests
      run: |
        python -m unittest discover
```
## Step 2: Create the Test Cases
Create a file named `test_app.py` in your project directory and add the following content:

```python
import unittest
from OnlineShopping import app

class BasicTests(unittest.TestCase):

    def setUp(self):
        self.app = app.test_client()
        self.app.testing = True

    def test_home_page(self):
        result = self.app.get('/')
        self.assertEqual(result.status_code, 200)

if __name__ == "__main__":
    unittest.main()
```

## Step 3: Ensure Dependencies are Listed
Make sure you have a `requirements.txt` file in your project directory listing all dependencies, including `pytest`.

```plaintext
Flask
pytest
gunicorn
```
## Step 5: Commit and Push Changes
Commit the new workflow file, test cases, and `requirements.txt` to your repository and push the changes.

```bash
git add .github/workflows/ci.yml test_app.py requirements.txt CI_CD_Pipeline.md app.yaml
git commit -m "Add CI/CD pipeline with GitHub Actions and test cases"
git push origin main
```

## Step 6: Verify the Workflow
After pushing the changes, go to your GitHub repository and navigate to the "Actions" tab. You should see the workflow running. If everything is set up correctly, the workflow will run and show the status of your tests, and then deploy the application to Google Cloud.

## Conclusion
By setting up this CI/CD pipeline and adding test cases, we ensure that our code is automatically tested on every push and pull request and deployed to Google Cloud. This helps us catch issues early and maintain a high-quality codebase.

## Verify the Workflow:

After pushing the changes, go to your GitHub repository and navigate to the "Actions" tab. You should see the workflow running. If everything is set up correctly, the workflow will run and show the status of your tests!


<img width="1436" alt="Screenshot 2024-07-04 at 2 30 37 PM" src="https://github.com/aaryamaster/OnlineShopping/assets/75297750/466e2f44-0b2a-4925-ae59-bc974ae2705e">


