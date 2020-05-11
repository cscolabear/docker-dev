FROM elasticsearch:7.6.2



RUN ./bin/elasticsearch-plugin install -b https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.6.2/elasticsearch-analysis-ik-7.6.2.zip

RUN curl -fsSL 'https://github.com/sunghau/elasticsearch-analysis-ik-config-traditional-chinese/archive/master.zip' -o ik-tc.zip \
    && unzip ik-tc.zip \
    && rm ik-tc.zip \
    && mkdir -p /usr/share/elasticsearch/config/ik-tc \
    && mv elasticsearch-analysis-ik-config-traditional-chinese-master/config/ik/* /usr/share/elasticsearch/config/ik-tc \
    && rm -rf elasticsearch-analysis-ik-config-traditional-chinese-master \
    && mv /usr/share/elasticsearch/config/analysis-ik /usr/share/elasticsearch/config/analysis-ik-sc \
    && mv /usr/share/elasticsearch/config/ik-tc /usr/share/elasticsearch/config/analysis-ik


RUN rm -rf /tmp/*

USER elasticsearch
