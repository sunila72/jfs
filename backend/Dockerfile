FROM maven AS build
WORKDIR /build
COPY . .
RUN mvn package
RUN mv target/*.jar App.jar

FROM eclipse-temurin:21-alpine
WORKDIR /app
COPY --from=build /build/App.jar .
CMD ["java","-jar","App.jar"]
EXPOSE 8080
