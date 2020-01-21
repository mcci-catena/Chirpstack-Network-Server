#
# Dockerfile for building chirpstack-application-server with letsencrypt certs support
#


FROM chirpstack/chirpstack-application-server:3

# passing arguments to map letsencrypt certs
ARG ssl_cert
RUN echo ${ssl_cert}
RUN ls /etc/

# Linking letsencrypt certiifcates
RUN ln -sf /etc/letsencrypt/live/${ssl_cert}/cert.pem /etc/cert.pem
RUN ln -sf /etc/letsencrypt/live/${ssl_cert}/chain.pem /etc/chain.pem
RUN ln -sf /etc/letsencrypt/live/${ssl_cert}/privkey.pem /etc/privkey.pem

