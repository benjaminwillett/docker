
### Dockerize your Flask Application

In this article, we’ll take a look at how to dockerize a Flask application. Flask is a microframework for Python, with a basis in Werkzeug and Jinja 2.

#### The Docker Hub image

Since Docker Hub doesn’t have an official Flask repository (at the time of this writing), we’ll explain how to build our own. While you can always use a non-official image, it’s generally recommended to make your own Dockerfile to ensure you know what is in the image. We’ll start off with the base of Ubuntu, but you can use any available distro you prefer.

#### Setting Up

We need to set up a basic app and Dockerfile.

#### Our basic app

Start with creating a new directory; let’s call it `flask_web`:

`mkdir flask_web`
You can use this basic app.py file for your basic application:

```
# flask_web/app.py

From flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world:
    return 'Hey, we have Flask in a Docker container!'


if __name == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

Now, we need to include Flask in our `requirements.txt` file:

```
Flask==0.10.1
```


pip installs from requirements.txt as normal. Since requirements.txt only references Flask 0.1.0, pip only installs Flask 0.1.0. If you are using Flask for your app, then you’re likely to have more modules specificed for installation.Build the imageNow that we have a Dockerfile, let’s verify it builds correctly:docker build -t flask-tutorial:latest .
After the build completes, we can run the container:docker run -d -p 5000:5000 flask-tutorial
Further informationEnsure you are using the right ports. Flask by default runs on port 5000 (not 8000 like Django or 80 like Apache). Check out Binding Docker Ports for more information.
