FROM python:3.7

RUN groupadd --gid 10001 app && \
    useradd -g app --uid 10001 --shell /usr/sbin/nologin --create-home --home-dir /app app

RUN apt-get update \
 && apt-get install -y zipalign osslsigncode \
 && apt-get clean \
 && ln -s /app/docker.d/healthcheck /bin/healthcheck

COPY . /app
RUN chown -R app:app /app

USER app
WORKDIR /app

RUN python -m venv /app \
 && cd signingscript \
 && /app/bin/pip install -r requirements/base.txt \
 && /app/bin/pip install . \
 && cd /app \
 && ./bin/pip install https://github.com/rail/configloader/archive/8719e33f6ba65e79e188355efabbcf6c9e1f1780.tar.gz

CMD ["/app/docker.d/init.sh"]
