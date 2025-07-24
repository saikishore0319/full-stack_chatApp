FROM node:24.4-alpine3.21 as builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --only=production

COPY . .

FROM node:24.4-alpine3.21

WORKDIR /app

# create system group called appgroup and create a sytem user called appuser and add the user inside the group
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/node_modules ./node_modules

COPY --from=builder /app ./

ENV NODE_ENV=production

EXPOSE 5001

USER appuser

CMD ["npm","start"]

HEALTHCHECK --interval=30s --retries=3 --timeout=10s --start-period=5s \
    CMD curl -f http://localhost:5001/health || exit 1 