### Python Project guide 

1. **Choose a Real-World Python Project**: We'll create a basic REST API using Flask that interacts with a database. This API will allow users to perform CRUD (Create, Read, Update, Delete) operations on a list of books.

2. **Set Up the Project Locally**: 
   - Create the Python code and dependencies.
   - Test it on your Ubuntu machine.

3. **Dockerize the Application**:
   - Write a `Dockerfile`.
   - Build and test the Docker image locally.

4. **Push the Code to GitLab**:
   - Push the Python project and `Dockerfile` to your GitLab repository.

5. **Push the Docker Image to Docker Hub**:
   - Use GitLab CI/CD with secrets to push the Docker image to Docker Hub.

6. **Run the Docker Container on an EC2 Instance**:
   - Set up an EC2 instance, pull the Docker image from Docker Hub, and run the application.

### Step 1: Choose a Real-World Python Project
We'll create a RESTful API for managing a collection of books using Flask and SQLite as a database.

### Step 2: Set Up the Project Locally

1. **Install Flask and SQLite**:
    - Create a project directory, e.g., `book_api`.
    - Inside the directory, create a virtual environment:

      ```bash
      python3 -m venv venv
      source venv/bin/activate
      ```

    - Install Flask and dependencies:

      ```bash
      pip install flask flask_sqlalchemy
      ```

    - Create a `requirements.txt` file:

      ```bash
      pip freeze > requirements.txt
      ```

2. **Create the Flask Application**:

    - Create a file named `app.py`:

      ```python
      from flask import Flask, jsonify, request
      from flask_sqlalchemy import SQLAlchemy

      app = Flask(__name__)
      app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///books.db'
      db = SQLAlchemy(app)

      class Book(db.Model):
          id = db.Column(db.Integer, primary_key=True)
          title = db.Column(db.String(80), nullable=False)
          author = db.Column(db.String(80), nullable=False)

      db.create_all()

      @app.route('/books', methods=['GET'])
      def get_books():
          books = Book.query.all()
          return jsonify([{'id': book.id, 'title': book.title, 'author': book.author} for book in books])

      @app.route('/books', methods=['POST'])
      def add_book():
          data = request.json
          new_book = Book(title=data['title'], author=data['author'])
          db.session.add(new_book)
          db.session.commit()
          return jsonify({'message': 'Book added!'}), 201

      @app.route('/books/<int:id>', methods=['PUT'])
      def update_book(id):
          data = request.json
          book = Book.query.get(id)
          if not book:
              return jsonify({'message': 'Book not found!'}), 404
          book.title = data['title']
          book.author = data['author']
          db.session.commit()
          return jsonify({'message': 'Book updated!'})

      @app.route('/books/<int:id>', methods=['DELETE'])
      def delete_book(id):
          book = Book.query.get(id)
          if not book:
              return jsonify({'message': 'Book not found!'}), 404
          db.session.delete(book)
          db.session.commit()
          return jsonify({'message': 'Book deleted!'})

      if __name__ == '__main__':
          app.run(host='0.0.0.0', port=5000)
      ```

3. **Test Locally**:
    - Run the application locally:

      ```bash
      python app.py
      ```

    - Access the API at `http://localhost:5000/books`.

### Step 3: Dockerize the Application

1. **Create a `Dockerfile`**:

    ```Dockerfile
    FROM python:3.8-slim

    WORKDIR /app

    COPY . /app

    RUN pip install --no-cache-dir -r requirements.txt

    EXPOSE 5000

    CMD ["python", "app.py"]
    ```

2. **Build and Test the Docker Image**:

    - Build the Docker image:

      ```bash
      docker build -t book_api .
      ```

    - Run the Docker container:

      ```bash
      docker run -d -p 5000:5000 --name book_api_container book_api
      ```

    - Test the API at `http://localhost:5000/books`.

### Step 4: Push the Code to GitLab

1. **Initialize a Git repository**:

    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    ```

2. **Create a GitLab repository and push your code**:

    ```bash
    git remote add origin <your-gitlab-repository-url>
    git push -u origin main
    ```

### Step 5: Push the Docker Image to Docker Hub

1. **Create a `.gitlab-ci.yml` file** in your project root:

    ```yaml
    stages:
      - build
      - deploy

    build:
      stage: build
      script:
        - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_REF_NAME .
      tags:
        - myec2runner

    deploy:
      stage: deploy
      script:
        - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
        - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_REF_NAME
      tags:
        - myec2runner
    ```

2. **Set up GitLab CI/CD variables**:

    - Go to your GitLab project settings and add the following variables:
      - `DOCKER_USERNAME`: Your Docker Hub username.
      - `DOCKER_PASSWORD`: Your Docker Hub password.

3. **Run the pipeline**: GitLab will build the Docker image and push it to Docker Hub.

### Step 6: Run the Docker Container on an EC2 Instance

1. **Launch an EC2 instance** with Docker installed.
2. **Pull the Docker image** from Docker Hub:

    ```bash
    docker pull <your-dockerhub-username>/book_api:<gitlab-branch-name>
    ```

3. **Run the container**:

    ```bash
    docker run -d -p 5000:5000 --name book_api_container <your-dockerhub-username>/book_api:<gitlab-branch-name>
    ```

4. **Access the API**: Use the public IP of your EC2 instance to access the API, e.g., `http://<your-ec2-ip>:5000/books`.

---

This guide takes you through the full process of developing, Dockerizing, and deploying a real-world Python project. Let me know if you need more details or assistance with any step!
