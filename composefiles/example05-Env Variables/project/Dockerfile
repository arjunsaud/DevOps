# FROM python:3.4
ARG PYTHON_VERSION
# This args takes python version from docker-compose.yml files
# It reads value for that argument, it is treated as variable
FROM python:$PYTHON_VERSION
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD [ "python" ,"app.py" ]