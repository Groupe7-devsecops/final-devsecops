# === build ===
FROM golang:1.22-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download


COPY . .


RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o backend .

# === run ===
FROM alpine:3.19


RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

COPY --from=builder /app/backend /app/backend

USER appuser
EXPOSE 8080

CMD ["./backend"]
