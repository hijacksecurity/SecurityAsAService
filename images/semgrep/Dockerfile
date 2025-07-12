FROM python:3.11-slim

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install semgrep
RUN pip install --no-cache-dir semgrep==1.45.0

# Create non-root user
RUN useradd -m -u 1000 scanner

# Set working directory
WORKDIR /src

# Switch to non-root user
USER scanner

# Default command
ENTRYPOINT ["semgrep"]
CMD ["--help"]