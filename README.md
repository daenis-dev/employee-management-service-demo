# Employee Management Service Demo

- This code base is used to build an instance of the Employee Management Application

  - API Repository: https://github.com/daenis-dev/employee-service
  - UI Repository: https://github.com/daenis-dev/employee-interface

- Enable TLS by generating the keys for the Employee Service and the Authorization Server

  - Navigate to *employee-service/certs* and create the private key:

    ```
    keytool -genkeypair -alias employee-service -keyalg RSA -keysize 4096 -storetype PKCS12 -keystore employee-service.p12 -validity 3650 -storepass changeit
    ```

    - CN = employee-service

  - Export the certificate:

    ```
    keytool -export -keystore employee-service.p12 -alias employee-service -file employee-service.crt
    ```

  - Navigate to *authorization-server/certs* and create the private key:

    ```
    keytool -genkeypair -alias keycloak-server -keyalg RSA -keysize 4096 -storetype PKCS12 -keystore server.p12 -validity 3650 -storepass changeit
    ```

    - CN = keycloak-server

  - Export the certificate:

    ```
    keytool -export -keystore server.p12 -alias keycloak-server -file employee-service.crt
    ```

  - Copy *server.crt* into *employee-service/certs*, and copy *employee-service.crt* into *authorization-server/certs*

- The authorization configuration file needs to be updated to include the actual mail server properties

  - A free mail server can be created with https://mailtrap.io, and the corresponding code that needs to be updated is in the *Employee-Management-Service-realm.json* file:

    ```json
    "smtpServer" : {
        "replyToDisplayName" : "",
        "starttls" : "true",
        "auth" : "true",
        "envelopeFrom" : "",
        "ssl" : "false",
        "password" : "mailtrap-password",
        "port" : "587",
        "host" : "sandbox.smtp.mailtrap.io",
        "replyTo" : "",
        "from" : "someone@gmail.com",
        "fromDisplayName" : "Employee Management [Automated]",
        "user" : "mailtrap-user"
    }
    ```

- Once downloaded, change into the project root and run the command

  ```
  >> docker-compose up
  ```

  - Docker will have to download and compile source code, so the build will take a few minutes

- Once the application is running, configure the browser to accept the self signed certificates that are associated with the API and the authorization server
  
  - Use the browser to navigate to both https://localhost:8080 and https://localhost:9880. Select the *Advanced* options, and proceed to the sites; an error page should be displayed
  
- Use the browser to navigate to http://localhost:4200 and sign in with the username *michaelscott@mail.com* and the password *changeit*