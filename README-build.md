## Building the images

#### Build API image:

*Note: use `sudo` to run the docker commands* <br/>
Go to the project directory `/django-todo-react`

1. Build image for the REST API <br/>
`sudo docker build -t todo-api \backend`

    You can now find the image in the local registry (use `sudo docker images`). 

2. Tag image <br/>
`sudo docker tag todo-api todo-api:v1`
    
    You can now refer to the image by name and tag: todo-api:v1.
    
3. [OPTIONAL] As an optional step, you can test the REST API. Run a container with the newly created image: 
`sudo docker run -p 80:80 todo-api:v1`. <br/>
Before this, you need to setup a local database. The database connection information can be configured in `settings.py`, 
before building the image. You can now open a web browser on the same host and load the URL:
`http://localhost:80/api/todos`. You should now be able to see a list of todo items.
    
4. Tag image for microk8s registry <br/>
`sudo docker tag todo-api:v1 localhost:32000/todo-api:v1` <br/>

    Note: Make sure the microk8s registry add-on is enabled.<br/> 
    Ensure that microk8s is running: `microk8s status`. If it is not running, start it: `microk8s start`.
    Enable the registry add-on: `microk8s enable registry`.

5. Push image to the microk8s registry add-on (for microk8s to see the image) <br/>
`sudo docker push localhost:32000/todo-api:v1`

#### Build UI image:

1. Build image for the UI <br/>
`sudo docker build -t todo-ui \frontend`
    
    You can now find the image in the local registry (use `sudo docker images`). 
    You can run a container using: `sudo docker run -p 3000:3000 todo-ui`

2. Tag image <br/>
`sudo docker tag todo-ui todo-ui:v1`

3. [OPTIONAL] Test the UI by running the newly created image in a container.
`sudo docker run -p 3000:3000 todo-ui:v1`. <br/>
You can now open a web browser on the same host and load the URL:
`http://localhost:3000`

4. Tag image for microk8s registry <br/>
`sudo docker tag todo-ui:v1 localhost:32000/todo-ui:v1` <br/>

5. Push image to the microk8s registry <br/>
`sudo docker push localhost:32000/todo-ui:v1`
