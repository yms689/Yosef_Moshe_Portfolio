# Dockerfile: serve the verified blocklist to FortiGate
FROM python:3.9-slim
WORKDIR /app
COPY samples/malicious_ips.txt /app/malicious_ips.txt
EXPOSE 8080
CMD ["python", "-m", "http.server", "8080"]