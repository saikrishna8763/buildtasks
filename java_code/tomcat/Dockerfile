# -------- Stage 1: Build WAR file using Maven --------
FROM maven as builder

# Set working directory
WORKDIR /app



# RUN  git install -y && git clone <git-url>

# Copy Maven project files
COPY . .

# Build the WAR file
RUN mvn clean package -DskipTests

# -------- Stage 2: Run with Tomcat --------
FROM tomcat:11.0.7-jdk21-temurin

# Remove default webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy WAR from build stage to Tomcat webapps
COPY --from=builder /app/target/wwp-1.0.0.war /usr/local/tomcat/webapps/wwp.war

# Expose default port
EXPOSE 8080

# Start Tomcat (CMD is already defined in tomcat base image)





