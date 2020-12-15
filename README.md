# Machine Learning app dev with docker

Proyecto python para desarrollo de apps con flask y Docker. Si no usas Docker, escribe tu códe directamente en  [pflask-toheroku]

(opcional) Incluye un cliente VueJS https://github.com/202ml/pflask/tree/main/client

* [Resources sharing to docker](#resources-sharing-to-docker)
* [Runing form Docker](#runing-form-Docker)
* [Deploy en heroku](#deploy-en-heroku)
* [Testing predict desde un cliente vuejs](#testing-predict-desde-un-cliente-vuejs)
* [Testing api predict usando heroku o localhost](#testing-api-predict-usando-heroku-o-localhost)

* [License](#license)

## Resources sharing to docker

	Add D:\dockr

Dockerfile
```bash
FROM continuumio/anaconda3 

WORKDIR /usr/src/app

# add and install requirements
COPY ./requirements.txt /usr/src/app/requirements.txt
RUN pip install -r requirements.txt

COPY . /usr/src/app
# CMD flask run --host=0.0.0.0

```

docker-compose.yml
```bash
version: '3.7'

services:

  mlapp:
    build:
      context: ./services/mlapp
      dockerfile: Dockerfile
    volumes:
      - './services/mlapp:/usr/src/app'
    ports:
      - 5001:5000
    tty: true # para que se mantenga sin necesidad de run un server
    container_name: mlapp_py
    environment:
      - FLASK_APP=project/app.py 
      - FLASK_ENV=development
      - APP_SETTINGS=project.config.DevelopmentConfig 

```


### Runing form Docker

Get and runing docker project

```bash
PS D:\dockr\ppy>git clone https://github.com/202ml/pflask.git
PS D:\dockr\ppy>cd pflask 

PS D:\dockr\ppy\pflask> docker-compose up --build -d
PS D:\dockr\ppy\pflask> docker ps
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                    NAMES
0964fe0a5e0b        pflask_mlapp                  "/bin/bash"              3 minutes ago       Up 4 seconds        0.0.0.0:5001->5000/tcp   mlapp_py


```

Runing
```bash
PS D:\dockr\ppy\pflask> docker exec -it mlapp_py sh        
# exit
PS D:\dockr\ppy\pflask> docker exec -it mlapp_py bash 
(base) root@0964fe0a5e0b:/usr/src/app# flask run --host=0.0.0.0


```

Ir a http://localhost:5001

Este es el modelo https://github.com/202ml/202ml/blob/master/notebooks/202ml/tree/DTS.joblib usado para esta app, lo cual se generó con https://github.com/202ml/202ml/blob/master/notebooks/202ml/tree/DT.ipynb




Si quiere correr en back, ejecute
```bash
PS D:\dockr\ppy\pflask> docker-compose up -d

```

### Deploy en heroku

[pflask-toheroku]:      https://github.com/202ml/pflask-toheroku

Please go to [pflask-toheroku] es el proyecto para subir a heroku.
Copy the `services\mlapp\project\templates` folder and `services\mlapp\project\app.py` file and paste into [pflask-toheroku] root.

## Testing predict desde un cliente vuejs
```bash
PS D:\dockr\ppy\pflask> cd client
PS D:\dockr\ppy\pflask\client> npm install
PS D:\dockr\ppy\pflask\client> npm run serve
```
Mayor detalle en https://github.com/202ml/pflask/tree/main/client


### Testing api predict usando heroku o localhost
En POSTMAN
Ejecute `https://pflask-toheroku.herokuapp.com/api/predict` o `http://localhost:5001/api/predict` con método `POST`
En el body, colocar las features de entrada
```bash
{
   "glucosa": 485,
   "insulina": 8966
}
```

Obtendrá estos resultados
```bash
{
    "features": {
        "glucosa": 485,
        "insulina": 8966
    },
    "predictions": "diabetes"
}
```

Udsted ya puede generar su propio modelo ML `DTS.joblib` y hacer los cambios respectivos en este código


### License



GNU, see [LICENSE](LICENSE).

Equipo de investigación y desarrollo: 
- angeli@upeu.edu.pe, 
