# Issue #5 investgation

## Requirements

* You need Python 3.11 with pyenv or installed directly. However, any
  version starting with 3.6 or 3.7 should work. Pyenv will activate
  automatically in this directory.
* Docker and Docker Compose for running OpenSearch

## Hot to set up this project

```shell

# Setup virtual environment (make sure proper Python 3 is activated)
python -m venv .venv

# Activate venv
source .venv/bin/activate

# Install requirements
pip install -r requirements.txt

# Launch OpenSearch with Docker compose
# Exposed on localhost:9200
docker compose up -d

# Go to Django app dir
cd mysite

# Launch Django app
python manage.py runserver

# You should get a lot of output indicating correct launch
```

## How to reproduce reported issue

Do the same steps as above, but do not install `opensearch-logger`.
When you run the application, you will get the following output.

```python
Watching for file changes with StatReloader
Exception in thread django-main-thread:
Traceback (most recent call last):
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 385, in resolve
    found = self.importer(used)
            ^^^^^^^^^^^^^^^^^^^
ModuleNotFoundError: No module named 'opensearch_logger'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 562, in configure
    handler = self.configure_handler(handlers[name])
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 724, in configure_handler
    klass = self.resolve(cname)
            ^^^^^^^^^^^^^^^^^^^
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 396, in resolve
    raise v from e
ValueError: Cannot resolve 'opensearch_logger.OpenSearchHandler': No module named 'opensearch_logger'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/threading.py", line 1038, in _bootstrap_inner
    self.run()
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/threading.py", line 975, in run
    self._target(*self._args, **self._kwargs)
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 64, in wrapper
    fn(*args, **kwargs)
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/core/management/commands/runserver.py", line 125, in inner_run
    autoreload.raise_last_exception()
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 87, in raise_last_exception
    raise _exception[1]
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/core/management/__init__.py", line 398, in execute
    autoreload.check_errors(django.setup)()
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 64, in wrapper
    fn(*args, **kwargs)
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/__init__.py", line 19, in setup
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
  File "/Users/vduseev/Projects/Playground/opensearch-logger-django-bug/.venv/lib/python3.11/site-packages/django/utils/log.py", line 76, in configure_logging
    logging_config_func(logging_settings)
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 812, in dictConfig
    dictConfigClass(config).configure()
  File "/Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/logging/config.py", line 569, in configure
    raise ValueError('Unable to configure handler '
ValueError: Unable to configure handler 'opensearch'
```

## What output is shown for the correct installation

When `opensearch-logger` is installed, then the setup above produces the
following output.

```python
2022-12-26 23:04:24,785 - django.utils.autoreload - INFO - Watching for file changes with StatReloader
2022-12-26 23:04:24,785 - django.utils.autoreload - INFO - Watching for file changes with StatReloader
Performing system checks...

2022-12-26 23:04:24,786 - django.utils.autoreload - DEBUG - Waiting for apps ready_event.
2022-12-26 23:04:24,786 - django.utils.autoreload - DEBUG - Waiting for apps ready_event.
2022-12-26 23:04:24,790 - django.utils.autoreload - DEBUG - Apps ready_event triggered. Sending autoreload_started signal.
2022-12-26 23:04:24,790 - django.utils.autoreload - DEBUG - Apps ready_event triggered. Sending autoreload_started signal.
2022-12-26 23:04:24,797 - django.utils.autoreload - DEBUG - Watching dir /Users/vduseev/Projects/Playground/opensearch-logger-django-bug/mysite/locale with glob **/*.mo.
2022-12-26 23:04:24,797 - django.utils.autoreload - DEBUG - Watching dir /Users/vduseev/Projects/Playground/opensearch-logger-django-bug/mysite/locale with glob **/*.mo.
System check identified no issues (0 silenced).
2022-12-26 23:04:24,811 - django.db.backends - DEBUG - (0.000) 
            SELECT name, type FROM sqlite_master
            WHERE type in ('table', 'view') AND NOT name='sqlite_sequence'
            ORDER BY name; args=None; alias=default
2022-12-26 23:04:24,811 - django.db.backends - DEBUG - (0.000) 
            SELECT name, type FROM sqlite_master
            WHERE type in ('table', 'view') AND NOT name='sqlite_sequence'
            ORDER BY name; args=None; alias=default

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
December 26, 2022 - 23:04:24
Django version 4.1.4, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
2022-12-26 23:04:24,844 - django.utils.autoreload - DEBUG - File /Users/vduseev/.pyenv/versions/3.11.0/lib/python3.11/lib-dynload/_datetime.cpython-311-darwin.so first seen with mtime 1668936488.0
2022-12-26 23:04:24,941 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.128s]
2022-12-26 23:04:24,959 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.018s]
...
...
...
2022-12-26 23:04:27,283 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.031s]
2022-12-26 23:04:28,317 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.032s]
2022-12-26 23:04:29,341 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.018s]
2022-12-26 23:04:30,377 - opensearch - INFO - POST https://localhost:9200/_bulk [status:200 request:0.029s]
...
...
...
```

## How to check in OpenSearch that logs actually appear

In OpenSearch Dashboards, go to Dev Tools and type

```shell
GET _cat/indices

# You will see an output similar to this
yellow open security-auditlog-2022.12.26 8N61Q9yuQmSYEz9kr11g3Q 1 1   78 0  230kb  230kb
green  open .kibana_92668751_admin_1     S4tM3cOvQbC68JMhAWLlug 1 0    1 0  5.1kb  5.1kb
yellow open my-logs-2022.12.26           B5chPpyZQEGhlZeeKVz3bQ 1 1 1537 0  569kb  569kb
green  open .kibana_1                    KRoOPlToSiaGkL1uYMelzg 1 0    0 0   208b   208b
green  open .opendistro_security         cL6wO3SaTPqC5Zag8iyG2g 1 0   10 0 71.7kb 71.7kb
```

Third line indicates that the log has been successfully created.

Check that logs actually have appeared:

```shell
GET my-logs-2022.12.26/_search
{
  "query": {
    "match_all": {}
  }
}
```

And it will print all the logs for this day.
