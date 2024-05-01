ARG BUILD_FROM
FROM $BUILD_FROM

# install iperf3 and create non-root user
RUN apk add --no-cache iperf3 \
  && adduser -S iperf

USER iperf

# Expose the default iperf3 server ports
EXPOSE 5201/tcp 5201/udp

# entrypoint allows you to pass your arguments to the container at runtime
# very similar to a binary you would run. For example, in the following
# docker run -it <IMAGE> --help' is like running 'iperf --help'
ENTRYPOINT ["iperf3"]

# iperf3 -s = run in Server mode
CMD ["-s"]