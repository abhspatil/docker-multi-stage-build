FROM golang:1.18.0-alpine3.15 AS builder
ARG GH_PAT

LABEL stage=builder

# Install os dependencies for builiding
RUN apk add --no-cache --update git && apk add build-base

# Set the working directory
WORKDIR /app

# Copy go.mod and go.sum files
COPY ./go.mod ./go.sum ./

# Add GOPRIVATE environment variable
ENV GOPRIVATE="github.com/abhspatil/*"

# Adding git config
RUN git config --global url.https://$GH_PAT@github.com/.insteadOf https://github.com/

# Download all go module dependencies
RUN go mod download

# Copy the source to the container
COPY ./ .

# Compile the code
RUN CGO_ENABLED=0 GOOS=linux go build -a -o bin/app cmd/app/main.go

FROM alpine:3.15 AS final

WORKDIR /app

COPY --from=builder /app/bin/app ./bin/
COPY --from=builder /app/.env.example ./.env
COPY --from=builder /app/config/config.yaml ./config/

# Expose the port for the server - Port should come from env variable
EXPOSE 8080

CMD ["/app/bin/app"]
