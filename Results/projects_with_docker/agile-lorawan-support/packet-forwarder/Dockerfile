FROM resin/rpi-raspbian

ENV INSTALL_DIR /opt/ttn-gateway

RUN apt-get update && apt-get install -y \
  build-essential \
  gettext \
  git \
  iproute2 \
  iputils-ping \
  libftdi-dev \
  python-dev \
  swig \
  WiringPi; \
git clone -b spi https://github.com/ttn-zh/ic880a-gateway.git ~/ic880a-gateway; \
git clone https://github.com/devttys0/libmpsse.git $INSTALL_DIR/libmpsse; \
cd $INSTALL_DIR/libmpsse/src; \
./configure --disable-python; \
make; \
make install; \
ldconfig; \
git clone -b legacy https://github.com/TheThingsNetwork/lora_gateway.git $INSTALL_DIR/lora_gateway; \
cp $INSTALL_DIR/lora_gateway/libloragw/99-libftdi.rules /etc/udev/rules.d/99-libftdi.rules; \
sed -i -e 's/PLATFORM= kerlink/PLATFORM= imst_rpi/g' $INSTALL_DIR/lora_gateway/libloragw/library.cfg; \
cd $INSTALL_DIR/lora_gateway; \
make; \
git clone -b legacy https://github.com/TheThingsNetwork/packet_forwarder.git $INSTALL_DIR/packet_forwarder; \
cd $INSTALL_DIR/packet_forwarder; \
make; \
mkdir $INSTALL_DIR/bin; \
ln -s $INSTALL_DIR/packet_forwarder/poly_pkt_fwd/poly_pkt_fwd $INSTALL_DIR/bin/poly_pkt_fwd; \
cp -f $INSTALL_DIR/packet_forwarder/poly_pkt_fwd/global_conf.json $INSTALL_DIR/bin/global_conf.json; \
cp ~/ic880a-gateway/start.sh $INSTALL_DIR/bin/; \
rm -rf /var/lib/apt/lists/* /tmp/*;

COPY local_conf_template.json cmd.sh $INSTALL_DIR/bin/
WORKDIR $INSTALL_DIR/bin
CMD ./cmd.sh
