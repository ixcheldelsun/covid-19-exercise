# pull official base image
FROM jupyter/scipy-notebook

# set work directory
WORKDIR /usr/src/app

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

RUN conda install --quiet --yes \
    'psycopg2' 'notebook' 'jupyter' 'ipykernel'

RUN jupyter serverextension enable --py jupyter_http_over_ws

# copy project
COPY . .
