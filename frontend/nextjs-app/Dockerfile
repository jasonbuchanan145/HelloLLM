# --- build stage ---
FROM node:20.15.1-alpine3.19 AS builder
WORKDIR /app
COPY frontend/nextjs-app/package.json package-lock.json ./
RUN npm ci --prefer-offline
COPY . .
RUN npm run build

# --- runtime stage ---
FROM node:20-bookworm
WORKDIR /app
ENV NODE_ENV=production PORT=3000
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/package*.json ./
RUN npm ci --omit=dev --prefer-offline
EXPOSE 3000
CMD ["npm", "start"]