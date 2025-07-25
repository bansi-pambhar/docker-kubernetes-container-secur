# Use an official minimal base image
FROM nginx:1.23-alpine

# Remove default nginx configs (optional for hardening)
RUN rm /etc/nginx/conf.d/default.conf

# Add your own secure config if needed
# COPY nginx.conf /etc/nginx/nginx.conf

# Add healthcheck to verify if nginx is responsive
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s CMD wget --spider http://localhost || exit 1

# Drop root privileges (if extended with scripts)
USER nginx

# Expose the default port
EXPOSE 80

# Start nginx (default CMD already set)

