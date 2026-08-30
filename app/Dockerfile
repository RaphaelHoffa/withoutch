#### BUILD STAGE ####

FROM cgr.dev/chainguard/python:latest-dev AS build
ARG ${IDENTITY_ID}
ARG ${CHAINGUARD_PYTHON_TOKEN}

WORKDIR /app

RUN echo ${CHAINGUARD_PYTHON_TOKEN}
COPY requirements.txt requirements.txt

RUN pip install --index-url https://${IDENTITY_ID}:${CHAINGUARD_PYTHON_TOKEN}@libraries.cgr.dev/python-remediated/simple/ --extra-index-url https://libraries.cgr.dev/python/simple/ --no-cache-dir -r requirements.txt

#### RUNTIME STAGE ####

FROM cgr.dev/chainguard/python:latest AS runtime

WORKDIR /app


COPY --from=build /home/nonroot/.local /home/nonroot/.local
COPY app.py .

ENV PATH="/home/nonroot/.local/bin:${PATH}"

ENV FLASK_APP=app.py
ENV FLASK_ENV=development
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1



EXPOSE 8000

USER nonroot

CMD ["app.py"]