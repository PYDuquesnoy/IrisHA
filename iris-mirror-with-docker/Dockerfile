#FROM intersystems/iris:2020.2.0.211.0
FROM containers.intersystems.com/intersystems/iris:2024.1

USER root

COPY session.sh /

RUN mkdir -p /opt/demo && \
    chown ${ISC_PACKAGE_MGRUSER}:${ISC_PACKAGE_IRISGROUP} /opt/demo

COPY init_mirror.sh /opt/demo 
RUN chmod 777 /opt/demo/init_mirror.sh


#Change back to irisowner
USER irisowner

WORKDIR /opt/demo

ARG IRIS_MIRROR_ROLE=master

COPY Installer.cls /opt/demo
COPY src /opt/demo/src

#SHELL [ "/session.sh" ]
# RUN \
# do $SYSTEM.OBJ.Load("Installer.cls", "ck") \
# set sc = ##class(Demo.Installer).setup()

# Compile the application code
RUN iris start iris && \
    printf 'zn "USER" \n \
    do $system.OBJ.Load("/opt/demo/Installer.cls","fck")\n \
    do ##class(Demo.Installer).Run()\n \
    zn "%%SYS"\n \
    do ##class(SYS.Container).QuiesceForBundling()\n \
    h\n' | irissession IRIS \
&& iris stop iris quietly   



CMD ["-a", "/opt/demo/init_mirror.sh"]