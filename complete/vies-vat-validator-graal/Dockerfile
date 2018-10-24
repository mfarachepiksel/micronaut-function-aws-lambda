FROM oracle/graalvm-ce:1.0.0-rc7
EXPOSE 8080
COPY build/libs/*-all.jar vies-vat-validator.jar
ADD . build
RUN java -cp vies-vat-validator.jar io.micronaut.graal.reflect.GraalClassLoadingAnalyzer
RUN native-image --no-server \
             --class-path vies-vat-validator.jar \
             -H:ReflectionConfigurationFiles=build/reflect.json \
             -H:EnableURLProtocols=http \
             -H:IncludeResources="logback.xml|application.yml|META-INF/services/*.*" \
             -H:Name=vies-vat-validator \
             -H:Class=io.micronaut.function.executor.FunctionApplication \
             -H:+ReportUnsupportedElementsAtRuntime \
             -H:+AllowVMInspection \
             --rerun-class-initialization-at-runtime='sun.security.jca.JCAUtil$CachedSecureRandomHolder,javax.net.ssl.SSLContext' \
             --delay-class-initialization-to-runtime=io.netty.handler.codec.http.HttpObjectEncoder,io.netty.handler.codec.http.websocketx.WebSocket00FrameEncoder,io.netty.handler.ssl.util.ThreadLocalInsecureRandom

ENTRYPOINT ["./vies-vat-validator"]
