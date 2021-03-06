FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
  g++ \
  make \
  file \
  curl \
  ca-certificates \
  python \
  git \
  cmake \
  sudo \
  gdb \
  xz-utils

COPY scripts/dumb-init.sh /scripts/
RUN sh /scripts/dumb-init.sh

# emscripten
COPY scripts/emscripten.sh /scripts/
RUN bash /scripts/emscripten.sh
COPY disabled/wasm32/node.sh /usr/local/bin/node

COPY scripts/sccache.sh /scripts/
RUN sh /scripts/sccache.sh

ENV PATH=$PATH:/emsdk-portable
ENV PATH=$PATH:/emsdk-portable/clang/e1.37.13_64bit/
ENV PATH=$PATH:/emsdk-portable/emscripten/1.37.13/
ENV EMSCRIPTEN=/emsdk-portable/emscripten/1.37.13/
ENV BINARYEN_ROOT=/emsdk-portable/clang/e1.37.13_64bit/binaryen/
ENV EM_CONFIG=/emsdk-portable/.emscripten

ENV TARGETS=wasm32-unknown-emscripten

ENV RUST_CONFIGURE_ARGS --target=$TARGETS

ENV SCRIPT python2.7 ../x.py test --target $TARGETS

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
