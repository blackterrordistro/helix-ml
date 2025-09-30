# 🌀 HelixML Docker Image
FROM rust:1.90-slim-bullseye

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    libssl-dev \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /workspace

# Copy workspace files
COPY Cargo.toml Cargo.lock ./
COPY crates/ ./crates/
COPY lib/ ./lib/
COPY examples/ ./examples/
COPY configs/ ./configs/

# Build dependencies first (for better caching)
RUN cargo build --workspace --release

# Copy source code
COPY . .

# Build the project
RUN cargo build --workspace --release

# Create non-root user
RUN useradd -m -s /bin/bash helix
USER helix

# Set environment variables
ENV RUST_BACKTRACE=1
ENV CARGO_TARGET_DIR=/workspace/target

# Expose port for serving (if needed)
EXPOSE 8080

# Default command
CMD ["cargo", "run", "--example", "simple_example"]
