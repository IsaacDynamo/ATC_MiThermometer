# Build container for ATC_MiThermometer (Telink TLSR825x, TC32 toolchain).
#
# The Telink TC32 toolchain (v2.0, gcc 4.5.1, 64-bit Linux, ~72 MB) is copied out of
# the community image skaldo/telink-sdk:0.1; nothing else from that image is used.
#
# Build the image (from the repository root):
#   docker build -t tc32-build docker/
#
# Build a firmware, e.g. LYWSD03MMC (output: ATC_v59.bin in the repository root):
#   docker run --rm -u "$(id -u):$(id -g)" -v "$PWD":/src tc32-build \
#     make -s -j TC32_PATH=/opt/tc32/bin/ \
#       PROJECT_NAME=ATC_v59 POJECT_DEF="-DDEVICE_TYPE=DEVICE_LYWSD03MMC"
#
# TC32_PATH must be passed on the make command line: the makefile otherwise expects
# the toolchain in SDK/tools/linux/tc32/, which is not part of this repository.

FROM skaldo/telink-sdk:0.1 AS tc32

FROM debian:trixie-slim
RUN apt-get update \
 && apt-get install -y --no-install-recommends make python3 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=tc32 /opt/tc32 /opt/tc32
RUN /opt/tc32/bin/tc32-elf-gcc --version | head -1
ENV PATH=/opt/tc32/bin:$PATH
WORKDIR /src
