FROM debian:buster-slim

WORKDIR /app

RUN mkdir config
ARG TAG

RUN apt update && \
    apt install build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev make g++ tmux git jq wget libncursesw5 \
    nano screen \
    -y

RUN wget https://downloads.haskell.org/~cabal/cabal-install-3.2.0.0/cabal-install-3.2.0.0-x86_64-unknown-linux.tar.xz && \
    tar -xf cabal-install-3.2.0.0-x86_64-unknown-linux.tar.xz && \
    rm cabal-install-3.2.0.0-x86_64-unknown-linux.tar.xz cabal.sig && \
    mv cabal /usr/local/bin/
RUN cabal update

WORKDIR /app/ghc
RUN wget https://downloads.haskell.org/~ghc/8.6.5/ghc-8.6.5-x86_64-deb9-linux.tar.xz && \
    tar -xf ghc-8.6.5-x86_64-deb9-linux.tar.xz && \
    rm ghc-8.6.5-x86_64-deb9-linux.tar.xz
WORKDIR /app/ghc/ghc-8.6.5
RUN ./configure
RUN make install
WORKDIR /app
RUN rm -rf ghc

WORKDIR /app/git
RUN git clone https://github.com/input-output-hk/cardano-node.git

WORKDIR /app/git/cardano-node

RUN cabal build all && \
    cp -p dist-newstyle/build/x86_64-linux/ghc-8.6.5/cardano-node-1.12.0/x/cardano-node/build/cardano-node/cardano-node /usr/local/bin/ && \
    cp -p dist-newstyle/build/x86_64-linux/ghc-8.6.5/cardano-cli-1.12.0/x/cardano-cli/build/cardano-cli/cardano-cli /usr/local/bin/

WORKDIR /app

#CMD ["cardano-node", "run", "--topology", "config/$NODEDIR/topology.json", "--database-path", "db", "--socket-path", "db/node.sock", "--port", "$NODEPORT", "--config", "config/$NODEDIR/config.json", "--shelley-kes-key", "config/$NODEDIR/kes.skey", "--shelley-vrf-key", "config/$NODEDIR/vrf.skey", "--shelley-operational-certificate", "config/$NODEDIR/cert"]
#CMD $COMMAND

#THE COMMAND BELOW IS BAD PRACTICE, BUT FOR THIS TUTORIAL WE WANT TO KEEP THIS SERVICE/CONTAINER RUNNING. SO WE CAN LOGON TO DO OUR EXERCISE.
CMD while true; do sleep 10; done
