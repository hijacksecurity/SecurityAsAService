FROM python:3.11-slim

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install semgrep
RUN pip install --no-cache-dir semgrep==1.45.0

# Create non-root user with GitHub Actions runner UID
RUN useradd -m -u 1001 scanner

# Set working directory and fix permissions
WORKDIR /src
RUN chown scanner:scanner /src

# Switch to non-root user
USER scanner

# Default command
ENTRYPOINT ["semgrep"]
CMD ["--help"]