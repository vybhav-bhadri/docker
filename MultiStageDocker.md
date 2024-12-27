
# Multi-Stage Docker Build

Multi-stage Docker builds are a powerful way to optimize Docker images by separating the build process into multiple stages. This approach is particularly useful for creating small, production-ready images by excluding unnecessary files and dependencies from the final image.

## Advantages of Multi-Stage Docker Build Compared to Single-Stage Dockerfile

1. **Smaller Image Size**:
   - Multi-stage builds allow you to copy only the necessary artifacts (like binaries) into the final image, excluding development dependencies and tools.
   - Reduces storage usage and network transfer costs.

2. **Improved Security**:
   - The final image contains only what is required to run the application, minimizing vulnerabilities.
   - No residual build tools or temporary files are included.

3. **Better Separation of Concerns**:
   - Each stage can focus on a specific task (e.g., building, testing, or packaging), making the Dockerfile more maintainable and understandable.

4. **Easier Debugging and Testing**:
   - Intermediate stages can be run and tested independently during the build process.

5. **Flexibility**:
   - Allows for creating multiple variants of an image (e.g., a debug image with additional tools) without duplicating the entire build process.

## Example of Multi-Stage Docker Build

Here is an example of a multi-stage Dockerfile for a Go application:

```dockerfile
# Stage 1: Build the application
FROM golang:1.20 AS build

# Set the working directory
WORKDIR /app

# Copy go.mod and go.sum files for dependency management
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the source code
COPY . ./

# Build the application
RUN go build -o myapp .

# Stage 2: Create a smaller production image
FROM gcr.io/distroless/base-debian10

# Copy the built application from the previous stage
COPY --from=build /app/myapp /

# Command to run the application
CMD ["/myapp"]
```

### Explanation of Commands:
1. **`FROM`**: Specifies the base image for a stage. In this example, the first stage uses `golang:1.20` for building, and the second stage uses a `distroless` image.
2. **`WORKDIR`**: Sets the working directory inside the container.
3. **`COPY`**: Copies files from the local machine to the container.
4. **`RUN`**: Executes commands in the shell during the image build process.
5. **`CMD`**: Specifies the default command to run when the container starts.

## What are Distroless Images?
Distroless images are minimal Docker images that include only the application and its runtime dependencies, excluding common Linux utilities and package managers. These images are provided by Google and are available for various programming languages.

### Example Distroless Images:
- `gcr.io/distroless/base`
- `gcr.io/distroless/java`
- `gcr.io/distroless/nodejs`

## Advantages of Distroless Images

1. **Smaller Image Size**:
   - Distroless images are minimal, resulting in smaller image sizes.
   - Reduces storage and network transfer costs.

2. **Enhanced Security**:
   - Eliminates unnecessary tools and libraries, reducing the attack surface.
   - No shell or package manager means fewer vulnerabilities.

3. **Faster Start-up Times**:
   - Smaller size and reduced overhead lead to quicker container start-up.

4. **Compliance**:
   - Minimalistic design adheres to the principle of least privilege.

## Security Benefits of Distroless Images

- **Reduced Attack Surface**: No package managers, shells, or debugging tools for attackers to exploit.
- **Immutable Infrastructure**: Distroless images discourage runtime modifications, aligning with best practices.
- **Vulnerability Scanning**: Fewer components to scan and patch, simplifying maintenance.

## CMD vs ENTRYPOINT

Both `CMD` and `ENTRYPOINT` define the commands that run when a container starts. However, they have key differences:

1. **CMD**:
   - Specifies the default command and arguments for a container.
   - Can be overridden at runtime by providing additional arguments to `docker run`.
   - Example:
     ```dockerfile
     CMD ["python", "app.py"]
     ```
   - If `docker run` provides arguments, the `CMD` is entirely replaced.

2. **ENTRYPOINT**:
   - Defines the command that is always executed when the container starts.
   - Arguments provided to `docker run` are passed to the `ENTRYPOINT` as additional parameters.
   - Example:
     ```dockerfile
     ENTRYPOINT ["/entrypoint.sh"]
     ```
   - To override `ENTRYPOINT`, the `--entrypoint` flag must be used in `docker run`.

3. **Combining CMD and ENTRYPOINT**:
   - `ENTRYPOINT` specifies the executable, and `CMD` provides default arguments.
   - Example:
     ```dockerfile
     ENTRYPOINT ["python"]
     CMD ["app.py"]
     ```
   - This configuration runs `python app.py` by default but allows overriding `app.py` at runtime.

## Most Used Syntax in Dockerfile

1. **`FROM`**:
   - Declares the base image.
   - Example: `FROM python:3.10-slim`

2. **`WORKDIR`**:
   - Sets the working directory.
   - Example: `WORKDIR /usr/src/app`

3. **`COPY`**:
   - Copies files from the host to the container.
   - Example: `COPY requirements.txt ./`

4. **`RUN`**:
   - Executes commands during the build process.
   - Example: `RUN pip install -r requirements.txt`

5. **`CMD`**:
   - Specifies the default command for the container.
   - Example: `CMD ["python", "app.py"]`

6. **`EXPOSE`**:
   - Documents the port on which the container listens.
   - Example: `EXPOSE 8080`

7. **`ENV`**:
   - Sets environment variables.
   - Example: `ENV APP_ENV=production`

8. **`ENTRYPOINT`**:
   - Configures a container to run as an executable.
   - Example: `ENTRYPOINT ["/entrypoint.sh"]`

9. **`ARG`**:
   - Defines build-time variables.
   - Example: `ARG VERSION=1.0`

10. **`LABEL`**:
    - Adds metadata to the image.
    - Example: `LABEL maintainer="example@example.com"`

By leveraging multi-stage builds and distroless images, you can create secure, efficient, and production-ready containers.
