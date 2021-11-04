# pull official base image
FROM jupyter/scipy-notebook

# set work directory
WORKDIR /usr/src/app

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

# name your environment and choose python 3.x version
ARG conda_env=python36
ARG py_ver=3.9

# you can add additional libraries you want mamba to install by listing them below the first line and ending with "&& \"
RUN mamba create --quiet --yes -p "${CONDA_DIR}/envs/${conda_env}" python=${py_ver} ipython ipykernel && \
    mamba clean --all -f -y

# alternatively, you can comment out the lines above and uncomment those below
# if you'd prefer to use a YAML file present in the docker build context

# COPY --chown=${NB_UID}:${NB_GID} environment.yml "/home/${NB_USER}/tmp/"
# RUN cd "/home/${NB_USER}/tmp/" && \
#     mamba env create -p "${CONDA_DIR}/envs/${conda_env}" -f environment.yml && \
#     mamba clean --all -f -y


# create Python 3.x environment and link it to jupyter
RUN "${CONDA_DIR}/envs/${conda_env}/bin/python" -m ipykernel install --user --name="${conda_env}" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# any additional pip installs can be added by uncommenting the following line
# RUN "${CONDA_DIR}/envs/${conda_env}/bin/pip" install

# prepend conda environment to path
ENV PATH "${CONDA_DIR}/envs/${conda_env}/bin:${PATH}"

RUN conda install --quiet --yes \
    'psycopg2' 'notebook' 'jupyter' 'ipykernel'

RUN jupyter serverextension enable --py jupyter_http_over_ws

# copy project
COPY . .
