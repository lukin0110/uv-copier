# syntax=docker/dockerfile:1
FROM ghcr.io/astral-sh/uv:python3.14-trixie AS dev

# Create and activate a virtual environment.
RUN python -m venv /opt/testing-env
ENV PATH=/opt/testing-env/bin:$PATH
ENV VIRTUAL_ENV=/opt/testing-env

# Install python dependencies for testing.
COPY testing/pyproject.toml . 
RUN uv sync

# Mark the workspace as safe for git
RUN git config --system --add safe.directory '*'
