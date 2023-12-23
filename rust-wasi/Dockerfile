FROM --platform=wasi/wasm rust:1.74.1 as base
WORKDIR /src
RUN <<EOT bash
    set -ex
    apt-get update
    apt-get install -y \
        git \
        clang
    rustup target add wasm32-wasi
EOT

FROM base AS build
COPY Cargo.toml .
COPY src ./src

# Build the WASM binary
RUN cargo build --target wasm32-wasi --release

FROM scratch
COPY --link --from=build /src/target/wasm32-wasi/release/server.wasm /server.wasm
ENTRYPOINT [ "/server.wasm" ]